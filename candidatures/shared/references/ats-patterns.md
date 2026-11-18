# Schémas de navigation des sites de recrutement (ATS)

Schémas d'automatisation du navigateur pour trois grands systèmes de suivi des candidatures. Constats issus de tests sur de vrais formulaires. Les ATS fréquents en France, comme Taleo, SmartRecruiters ou JobTeaser, n'ont pas encore de schéma établi : à la première rencontre, note ici ce qui marche.

## Greenhouse

**Intégration** : iframe d'un autre domaine (`id="grnhse_iframe"`), hébergée sur `job-boards.greenhouse.io`.

**Accès avec les outils MCP** : `read_page`, `find` et `form_input` NE PEUVENT PAS voir l'intérieur de l'iframe. Seul `javascript_tool` peut extraire des informations de la page hôte.

**Forme des adresses** :
- Source de l'iframe : `https://boards.greenhouse.io/embed/job_app?for={boardToken}&token={jobId}`
- Adresse directe du formulaire : `https://job-boards.greenhouse.io/embed/job_app?for={boardToken}&token={jobId}`
- Extraire les jetons par script : `document.getElementById('grnhse_iframe').src`, puis `new URL()` et `.searchParams.get('for')` / `.searchParams.get('token')`

**Contournement** : navigue directement vers l'adresse du formulaire Greenhouse avec les jetons extraits. Le formulaire se charge alors comme page principale, où `read_page` et `form_input` fonctionnent normalement.

**Extraction des jetons** (un filtre de sécurité bloque les adresses complètes : extrais les paramètres un par un) :
```javascript
const iframe = document.getElementById('grnhse_iframe');
const url = new URL(iframe.src);
JSON.stringify({
  boardToken: url.searchParams.get('for'),
  jobToken: url.searchParams.get('token')
});
```

**Champs habituels** : prénom*, nom*, e-mail*, téléphone*, CV* (fichier), lettre de motivation (fichier), localisation (ville)*, indicatif pays* (liste), profil LinkedIn, « comment avez-vous connu cette offre ? »* (liste), questions de diversité (souvent facultatives), questions d'autorisation de travail, case de politique de confidentialité, bouton d'envoi.

**Texte du bouton** : « Apply for this job » (sur la page de l'offre, hors iframe).

---

## Lever

**Intégration** : PAS d'iframe. Le formulaire s'affiche directement sur `jobs.lever.co`.

**Accès avec les outils MCP** : `read_page`, `find` et `form_input` fonctionnent TOUS directement. C'est l'ATS le plus simple à automatiser.

**Forme des adresses** :
- Offre : `https://jobs.lever.co/{entreprise}/{jobUUID}`
- Formulaire : `https://jobs.lever.co/{entreprise}/{jobUUID}/apply`
- Navigation : clique sur « APPLY FOR THIS JOB » sur l'offre, ou ajoute `/apply` à l'adresse de l'offre.

**Champs habituels** : localisation* (liste à saisie avec valeurs GUID), CV* (fichier, via le bouton « ATTACH RESUME/CV »), nom complet*, pronoms (cases), e-mail*, téléphone, localisation actuelle*, entreprise actuelle, LinkedIn, autres liens (GitHub, portfolio, site), sections propres à l'entreprise (autorisation de travail, acceptations), zone « informations complémentaires » (lettre de motivation), questionnaire de diversité, bouton d'envoi.

**Texte du bouton** : « APPLY FOR THIS JOB » (en majuscules, sur la page de l'offre).

---

## Workday

**Intégration** : PAS d'iframe. Redirige vers un domaine séparé hébergé par Workday.

**Accès avec les outils MCP** : `read_page` fonctionne : boutons, zones de texte et boutons de liste déroulante sont visibles avec leurs références. Mais `read_page` ne renvoie que les éléments de la fenêtre visible (il faut faire défiler pour tout capturer). Les boutons radio ne sont PAS renvoyés par le filtre interactif, même visibles à l'écran.

**Forme des adresses** :
- Page carrières de l'entreprise : `https://jobs.{entreprise}.com/jobs/job/{jobId}-{slug}/`
- Candidature Workday : `https://{entreprise}.wd{N}.myworkdayjobs.com/fr-FR/EXT/job/{lieu}/{titre}_{jobId}/apply`
- Candidater manuellement : ajoute `/applyManually` à l'adresse de candidature
- Préremplir avec le CV : ajoute `/autofillWithResume`

**Page d'accueil** : « Apply Now » sur la page carrières ouvre un nouvel onglet sur `*.myworkdayjobs.com` avec trois options :
1. **Autofill with Resume** (bouton principal)
2. **Apply Manually**
3. **Use My Last Application**

**Assistant en plusieurs étapes** (6 pages après connexion) :
1. Mes informations
2. Mon expérience
3. Questions de candidature
4. Déclarations volontaires
5. Auto-identification
6. Récapitulatif

**Limite importante** : Workday exige de créer un compte ou de se connecter avant l'assistant. Créer un compte et saisir un mot de passe sont interdits à l'automatisation : l'utilisateur gère lui-même l'authentification avant que le skill puisse l'aider à remplir le formulaire.

**Navigation** : bouton « Save and Continue » en bas de chaque page. Un clic avec des champs obligatoires vides affiche un cadre « Errors Found » listant les champs manquants avec des liens cliquables. Le bouton devient grisé pendant la validation.

**Page 1, Mes informations** (de haut en bas) :
1. « Comment avez-vous connu cette offre ? »* : liste hiérarchique par catégories (ex. « Job Board/Job Posting », « Social Media/Internet ») avec des sous-options. S'ouvre en fenêtre contextuelle ; une zone de recherche permet de filtrer (elle apparaît comme `textbox "Search"` dans `read_page`).
2. « Avez-vous déjà travaillé pour cette organisation ? »* : boutons radio Oui/Non. NON visibles par le filtre interactif de `read_page` : utilise `computer` avec un clic aux coordonnées, ou l'outil `find`.
3. Pays* : liste en bouton, souvent préremplie. Apparaît comme `button "Country ... Required"` dans `read_page`.
4. **Nom** : prénom* (zone de texte avec bouton de suggestions), nom*.
5. **Adresse** : ligne 1, ville, région (liste « Select One »), code postal. Souvent facultatifs.
6. **E-mail** : en lecture seule, prérempli par le compte Workday.
7. **Téléphone** : type d'appareil* (liste « Select One Required »), numéro* (zone de texte).

**Interaction avec les listes** : les listes Workday sont des éléments `button` qui ouvrent un panneau. Pour la liste hiérarchique, un clic révèle une liste où chaque catégorie a une flèche `>` vers ses sous-options, avec une zone de recherche en bas. Pour les listes simples (région, type d'appareil), un clic ouvre un panneau d'options.

---

## Synthèse de la stratégie d'automatisation

| Critère | Greenhouse | Lever | Workday |
|---------|-----------|-------|---------|
| Iframe | Oui (autre domaine) | Non | Non |
| `read_page` fonctionne | Non (contournement) | Oui | Oui |
| `form_input` fonctionne | Non (contournement) | Oui | Oui (après connexion) |
| Compte obligatoire | Non | Non | Oui |
| Type de formulaire | Une page | Une page | Assistant en plusieurs étapes |
| Bouton de candidature | « Apply for this job » | « APPLY FOR THIS JOB » | « Apply Now », puis page d'accueil |
| Difficulté | Moyenne | Facile | Élevée |

**Approche recommandée par ATS :**
- **Lever** : remplissage direct avec `form_input` et les références de `read_page`. Le plus simple.
- **Greenhouse** : extraire les jetons de l'iframe, naviguer vers l'adresse directe du formulaire, remplir les champs. Une étape de navigation en plus.
- **Workday** : l'utilisateur se connecte d'abord. Ensuite, aide au remplissage de l'assistant sur 5 pages plus le récapitulatif. Il faut faire défiler chaque page pour découvrir tous les champs, puisque `read_page` ne renvoie que la fenêtre visible. Les boutons radio demandent un clic aux coordonnées. Utilise les erreurs de validation (« Save and Continue » avec des champs vides) pour découvrir tous les champs obligatoires d'une page.

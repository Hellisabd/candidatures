# candidatures

> Plugin [Claude Code](https://claude.ai/code) pour candidater en France à une **alternance ou à un stage** : un profil construit avec toi, un CV adapté à chaque offre, des lettres de motivation (sur offre ou spontanées), la recherche d'offres et l'aide aux formulaires. Rien n'est inventé, rien n'est envoyé sans ton accord.

**Statut :** version 1.0.0. `setup`, `cover-letter` (candidature spontanée) et `tailor-resume` ont été testés sur un profil fictif ; la lettre sur offre est moins éprouvée. `job-search` et `apply`, qui pilotent le navigateur, n'ont pas encore été essayés sur de vraies offres.

## Pourquoi ce plugin ?

- **Il connaît le marché français** : CV d'une page, format de la lettre, vocabulaire de l'alternance (apprentissage, rythme école/entreprise) et du stage (convention, gratification), candidature spontanée.
- **Il écrit avec tes vrais faits.** Le profil vient d'une interview : rôle réel dans chaque projet, chiffres que tu as donnés, compétences que tu maîtrises. Une compétence jamais pratiquée n'est jamais présentée comme acquise.
- **Il garde une trace** : chaque lettre est archivée avec ses sources, et le suivi des candidatures (statut, relance à J+7) se met à jour au fil de l'eau.
- **Il reste sous ton contrôle** : chaque envoi demande ton « oui ».

## Exemple

Une étudiante fictive, Camille, cherche une alternance en marketing digital. Elle a fait le `setup`, puis :

```
/candidatures:cover-letter spontanée Maison Exemple
```

Le plugin cherche d'abord l'entreprise dans le suivi, rassemble des faits publics avec leur source, pose **une** question (« quel poste visez-vous et qu'est-ce qui vous attire ? »), puis rédige :

> En 2025-2026, j'ai géré le compte Instagram d'une association étudiante. Je tenais seule le calendrier de publication et créais les visuels sous Canva. Le compte est passé de 400 à 520 abonnés en 4 mois. […]
>
> Votre atelier de la Croix-Rousse et ses pièces faites à la main m'attirent. Votre compte Instagram, suivi par environ 12 000 personnes, montre beaucoup de photos de l'atelier. Je voudrais participer au lancement de la collection « Terre », annoncée sur votre site en octobre 2026 pour le printemps 2027.

À la fin, il signale ce qui mérite ton attention (ici, un décalage entre la date de la collection et celle du stage), liste les faits utilisés avec leur source, archive la lettre en PDF et ajoute une ligne au suivi avec le statut `todo`. Le statut ne passe à `sent` que lorsque tu confirmes l'envoi.

## Comment ça marche

| Commande | Rôle |
|---|---|
| `/candidatures:setup` | À faire une fois : CV, préférences, interview sur ton parcours, réponses aux formulaires, ton des lettres |
| `/candidatures:job-search` | Cherche des offres d'alternance et de stage (La Bonne Alternance, Welcome to the Jungle, LinkedIn, Indeed) et les note selon tes préférences |
| `/candidatures:tailor-resume [offre]` | Adapte ton CV à une offre, sur une page |
| `/candidatures:cover-letter [offre ou spontanée entreprise]` | Rédige la lettre et l'archive avec ses sources |
| `/candidatures:apply [offres]` | Postule (LinkedIn, site carrière ou mail) après ta validation, puis met à jour le suivi |

Un parcours typique : `setup` une fois, puis `job-search`, `tailor-resume`, `cover-letter` et `apply` pour chaque offre.

## Démarrage rapide

Prérequis : Claude Code. Le dépôt est **privé** : ton compte GitHub doit y avoir accès (invitation du propriétaire) et être connecté sur ta machine, par SSH ou par jeton.

```
/plugin marketplace add Hellisabd/candidatures
/plugin install candidatures@mes-plugins
/reload-plugins
/candidatures:setup
```

Le `setup` te pose des questions (CV, préférences, interview sur ton parcours projet par projet) et crée tes fichiers.

**Pour aller plus loin :**
- **Navigateur** : `job-search` et `apply` utilisent l'extension **Claude in Chrome**. `tailor-resume` et `cover-letter` s'en servent seulement pour lire une offre depuis son adresse ; en collant le texte de l'offre, elles s'en passent.
- **PDF** : le PDF de la lettre est créé avec Chrome ou Chromium en ligne de commande. Il faut l'un des deux installé.

## Ce que le plugin refuse de faire

- **Inventer** : une expérience, un chiffre, une date, un fait sur l'entreprise. Si l'information manque, il te la demande ou l'omet.
- **Présenter comme acquise** une compétence que tu n'as jamais pratiquée. Une technologie « à apprendre » peut figurer dans une lettre comme envie d'apprendre, jamais dans la liste des compétences du CV.
- **Gonfler ton rôle** : si tu as fait une partie d'un projet d'équipe, la lettre dit laquelle.
- **Supposer les besoins d'une entreprise** en candidature spontanée, ou **deviner le nom** d'un destinataire.
- **Louer l'entreprise sans source** : « qui s'engage dans… » plutôt que « reconnue pour… ».
- **Envoyer sans ton « oui »**, saisir un mot de passe, créer un compte à ta place ou cocher des cases optionnelles.

## Tes données et la vie privée

- Tes fichiers (préférences, profil, CV, réponses types, offres, suivi) sont dans `~/.candidatures/`, ou dans un dossier `.candidatures/` du répertoire courant s'il existe. **Ils ne font pas partie du plugin** et ne sont jamais ajoutés au dépôt.
- Le plugin n'envoie rien de lui-même. Mais ce que Claude lit pendant une session (ton CV, ton profil) est traité comme dans toute conversation avec Claude.
- Les archives de lettres vont par défaut dans `~/Documents/Candidatures/Lettres/`, un sous-dossier par candidature.

## Suivi et ton des lettres

- **Suivi** : par défaut, un fichier `suivi.md`. Pour qui tient un suivi dans un artefact Claude avec une base de données, c'est possible aussi : voir `candidatures/shared/references/tracker.md`.
- **Ton** : au `setup`, tu choisis « classique et chaleureux », « direct et sobre », ou ton style (colle une lettre dont tu aimes le ton). Les règles d'honnêteté ne changent pas.

## FAQ

**Est-ce que ça postule à ma place ?** Non. Le plugin prépare et remplit, puis te présente un récapitulatif complet (CV réellement joint, texte, réponses). Il n'envoie qu'après ton « oui ». Pour un lot d'offres LinkedIn, un seul récapitulatif couvre le lot, que tu valides avant l'envoi.

**Et si un site bloque l'automatisation ?** Le plugin s'arrête, te le dit et passe à la source suivante. Il ne contourne ni captcha ni identification.

**Ça marche pour un autre métier que développeur ?** Les consignes sont générales, et un test sur un profil fictif en marketing digital a donné des résultats propres. Mais le plugin a été conçu et utilisé d'abord pour un profil d'informatique, et ses sites de recherche n'ont pas été éprouvés ailleurs.

**Puis-je changer le ton ou les préférences plus tard ?** Oui : modifie `preferences.md` dans ton dossier de données, ou relance `/candidatures:setup`.

## Limites

- Fait pour la **France** uniquement.
- Testé sous **Linux**. La création du PDF utilise la commande `google-chrome`, qu'il faut adapter sur macOS ou Windows.
- `job-search` et `apply` dépendent de la structure de sites qui changent. Au premier passage, ils peuvent demander des ajustements.
- Plusieurs sites carrière (Taleo, SmartRecruiters, JobTeaser, Welcome to the Jungle) n'ont pas de procédure établie : le plugin y avance prudemment.

## Contribuer

Les retours sont bienvenus : ouvre une *issue* sur le dépôt avec ce que tu as lancé, ce qui s'est passé et ce que tu attendais. N'y colle jamais de données personnelles (CV, adresse, téléphone).

## Crédits et licence

Licence **MIT** : voir [`LICENSE`](LICENSE) et [`candidatures/NOTICE`](candidatures/NOTICE). Plugin dérivé d'un projet open source publié sous licence MIT, largement réécrit pour le marché français.

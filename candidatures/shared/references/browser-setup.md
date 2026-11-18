# Pilotage du navigateur

Déroulé commun aux skills qui utilisent Claude in Chrome pour lire des pages web.

## Prérequis

Chrome doit être ouvert avec l'extension Claude in Chrome, et Claude Code lancé avec `claude --chrome`. Sans cela, les outils de navigateur n'existent pas dans la session : dis-le à l'utilisateur au lieu de simuler le travail.

## Ouverture d'un onglet

```
1. tabs_context_mcp (createIfEmpty) → voir l'état du navigateur
2. tabs_create_mcp → créer un nouvel onglet (jamais réutiliser un onglet de l'utilisateur)
3. navigate → adresse voulue
4. javascript_tool ou read_page → extraire ce dont tu as besoin
```

À la fin, ferme les onglets que tu as ouverts.

## Ne pas saturer le contexte

**N'utilise pas `get_page_text` sur une page volumineuse ou dynamique** (sites d'emploi, résultats de recherche, listes, tableaux de bord). Il renvoie toute la page et peut saturer le contexte jusqu'à rendre la conversation inutilisable.

Extrais plutôt de façon ciblée :
- `javascript_tool` avec un sélecteur, pour ne récupérer que ce qu'il te faut
- `read_page` pour obtenir des références d'éléments
- `get_page_text` seulement pour une page simple, avec une seule offre ou un seul article
- Quand les classes d'un site sont masquées (LinkedIn), lis la page visuellement : `zoom` sur la zone voulue, puis défilement

N'inclus jamais l'adresse complète de la page (`location.href`) dans un script : elle contient des paramètres et la réponse est bloquée.

## Cookies et connexions

- Refuse les cookies non essentiels.
- N'utilise que les sessions déjà ouvertes par l'utilisateur. Ne crée aucun compte, ne saisis aucun mot de passe.
- Si un site demande de créer un profil ou un compte pour afficher des résultats, dis-le à l'utilisateur et demande son accord avant toute saisie. Passe à la source suivante si besoin.

## Gestion des erreurs

- Si `tabs_context_mcp` ne renvoie aucun onglet ou une erreur, demande à l'utilisateur de vérifier que Chrome est ouvert et que l'extension est active et connectée au même compte.
- Si `navigate` échoue ou si la page ne charge pas, demande à l'utilisateur de coller le contenu.
- Si `get_page_text` renvoie un contenu vide ou inutilisable, essaie `read_page`, puis demande à coller le contenu si cela échoue aussi.
- N'insiste pas plus d'une fois sur une page qui échoue : passe à la suite et demande le contenu à l'utilisateur.

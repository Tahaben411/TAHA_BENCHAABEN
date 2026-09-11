# TP 2 — Rendre ExploMonde accessible

Liste des défauts relevés sur la version brute du TP 1, et correction apportée.

| # | Défaut constaté | Conséquence | Correction |
|---|-----------------|-------------|------------|
| 1 | Pas d'attribut `lang` sur `<html>` | Le lecteur d'écran prononce le français avec une voix anglaise | `<html lang="fr">` |
| 2 | `<title>` générique | L'onglet, l'historique et le premier élément lu ne disent pas de quelle page il s'agit | `<title>ExploMonde — Rechercher et explorer les pays du monde</title>` |
| 3 | Aucun lien d'évitement | L'utilisateur au clavier retraverse l'en-tête et la navigation avant d'atteindre le contenu | `<a class="lien-evitement" href="#contenu">` en tout premier élément du `<body>`, visible uniquement au focus |
| 4 | Champ de recherche sans `<label>` associé (seul un `placeholder` le renseignait) | Champ annoncé « zone d'édition, vide » ; le placeholder disparaît dès la saisie | `<label for="champ-pays">Nom du pays</label>` + `id="champ-pays"` sur l'`<input>` ; idem pour le `<select>` région |
| 5 | Aucune zone de statut | Le nombre de résultats, le chargement et les erreurs ne sont jamais annoncés | `<p id="statut" role="status">` placée **avant** les résultats et **présente dès le chargement**, condition pour que la région live soit réellement surveillée |
| 6 | Sections sans nom accessible | La navigation par régions ne permet pas de distinguer les blocs | `aria-labelledby` sur chaque `<section>`, pointant vers son `<h2>` |
| 7 | Formulaire non identifié comme recherche | Pas de raccourci « aller au formulaire de recherche » | `role="search"` sur le `<form>` |
| 8 | `<nav>` sans nom | Deux navigations seraient indiscernables dans la liste des repères | `aria-label="Navigation principale"` |
| 9 | Focus invisible sur fond coloré | Impossible de savoir où l'on se trouve au clavier | `:focus-visible { outline: 2px solid var(--couleur-accent); outline-offset: 2px }` + `box-shadow` de focus sur les champs et le bouton |
| 10 | Emoji drapeau lu en double du nom du pays | Verbosité inutile (« drapeau de la France, France ») | `aria-hidden="true"` sur l'emoji ; le code ISO reste en texte |
| 11 | Cartes sans nom accessible | Les `<article>` apparaissaient anonymes dans la liste des repères | `aria-labelledby` sur chaque `<article>` vers son `<h3>` |
| 12 | Cibles tactiles trop petites | Difficiles à activer au doigt (critère AA 2.5.8 Target Size) | `min-height: 44px` sur les champs et le bouton |
| 13 | Contraste insuffisant du bouton et des liens | Orange `#e2562c` sur blanc = **3,75:1**, sous le seuil AA de 4,5:1 pour du texte normal | Accent interactif assombri à `#c2410c` (**5,18:1** sur blanc, **4,75:1** sur le fond). La teinte vive `#e2562c` est conservée dans `--couleur-accent-vif` pour le `h1` seul, où le seuil est 3:1 |
| 14 | Préférences système ignorées | Animations imposées, thème clair imposé | `@media (prefers-reduced-motion: reduce)` et `@media (prefers-color-scheme: dark)` |

## Navigation au clavier — vérification

Aucun `tabindex` positif : l'ordre de tabulation suit le DOM, qui suit l'ordre visuel.

1. Lien d'évitement (apparaît en haut à gauche au premier `Tab`)
2. Recherche → Résultats → À propos (navigation principale)
3. Champ « Nom du pays »
4. Liste « Région du monde » (flèches / `Espace` pour l'ouvrir)
5. Bouton « Rechercher » (`Entrée` et `Espace`)
6. Les trois liens « Consulter la fiche de … »
7. Lien restcountries.com du pied de page

Aucun élément n'est actionnable à la souris sans l'être au clavier : pas de `div` cliquable ni de gestionnaire sur un élément non focusable — tout passe par `<a>`, `<button>`, `<input>` et `<select>` natifs.

## Contrastes mesurés (WCAG 2.1, niveau AA)

| Paire | Thème clair | Thème sombre | Seuil |
|-------|-------------|--------------|-------|
| Texte courant / fond | 15,35:1 | 16,53:1 | 4,5:1 |
| Texte courant / surface des cartes | 16,74:1 | 14,74:1 | 4,5:1 |
| Texte secondaire (labels, statut) / surface | 6,69:1 | 7,35:1 | 4,5:1 |
| Texte du bouton / bouton | 5,18:1 | 6,50:1 | 4,5:1 |
| Liens en accent / surface | 5,18:1 | 6,34:1 | 4,5:1 |
| Titre `h1` en accent vif / fond | 3,44:1 | 7,11:1 | 3:1 (texte ≥ 24 px) |

Toutes les paires passent AA dans les deux thèmes.

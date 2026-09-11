# ExploMonde — TP 1, 2 et 3

Projet fil rouge : explorateur de pays. Aucun framework, aucune bibliothèque,
HTML valide W3C, accessibilité AA, CSS Mobile First de 360 px à 1440 px.

## Arborescence

```
explomonde/
├── index.html                  TP 1 + TP 2 : squelette sémantique, corrigé pour l'accessibilité
├── css/
│   ├── tokens.css              TP 3 : les variables (couleurs, rayons, espacements, typo…)
│   ├── base.css                TP 3 : reset léger + style de base Mobile First
│   └── components.css          TP 3 : en-tête, formulaire, grille de cartes, pied
├── js/
│   └── app.js                  point d'entrée des modules ES (vide à ce stade)
├── assets/
├── captures/
│   ├── mobile-360-clair.png    capture 360 px, thème clair
│   ├── mobile-360-sombre.png   capture 360 px, thème sombre
│   ├── bureau-1280-clair.png   capture 1280 px, thème clair
│   ├── bureau-1280-sombre.png  capture 1280 px, thème sombre
│   └── validation-w3c.json     réponse brute du validateur : 0 erreur, 0 avertissement
├── DEFAUTS-ACCESSIBILITE.md    TP 2 : liste des défauts trouvés et corrigés
└── README.md
```

## TP 1 — Le squelette d'ExploMonde

- Arborescence de projet créée.
- `index.html` : `header` (h1 + nav), `main`, `footer`.
- Dans `main` : une section de recherche (h2 + champ + bouton) et une section de résultats (h2).
- Trois articles de démonstration : France, Allemagne, Espagne, avec nom, capitale, région et population
  (chacun dans une `<dl>`, la structure qui décrit vraiment des paires libellé/valeur).
- **Validation W3C : 0 erreur, 0 avertissement** (réponse du validateur dans `captures/validation-w3c.json`).

Le document a été soumis à `validator.w3.org/nu` en HTTP. Pour la capture d'écran demandée, ouvrir
<https://validator.w3.org/#validate_by_upload> et y déposer `index.html` : le résultat est
« Document checking completed. No errors or warnings to show. »

## TP 2 — Rendre ExploMonde accessible

Les cinq consignes sont appliquées dans `index.html` (`lang="fr"` + `title` explicite, lien d'évitement,
labels associés, zone `role="status"`, navigation clavier complète avec focus toujours visible).

La liste détaillée des défauts trouvés, leur conséquence et leur correction sont dans
[DEFAUTS-ACCESSIBILITE.md](DEFAUTS-ACCESSIBILITE.md), avec la vérification de l'ordre de tabulation
et les contrastes mesurés.

## TP 3 — Habiller ExploMonde en Mobile First

1. **`css/tokens.css`** : 6 familles de variables — couleurs, rayons, espacements, typographie,
   ombres, mise en page — soit une trentaine de tokens.
2. **Mobile First sans media query de largeur** : le style de base est écrit pour une colonne ;
   il n'existe **aucune** `@media (min-width: …)` dans le projet. Les seules media queries sont
   `prefers-color-scheme` et `prefers-reduced-motion`, qui portent sur les préférences, pas sur la taille.
   Le formulaire se réorganise avec `flex-wrap` + `flex: 1 1 16rem`, la grille avec `auto-fit`.
3. **Grille des cartes** : `grid-template-columns: repeat(auto-fit, minmax(var(--carte-min), 1fr))`
   avec `--carte-min: 260px`. Une colonne à 360 px, et jusqu'à quatre à 1440 px sans toucher au HTML
   (le conteneur y est borné à 1200 px : 4 × 260 px + 3 × 16 px de gouttière tiennent, 5 non).
   Les trois cartes de démonstration occupent donc trois colonnes sur grand écran.
4. **Titre fluide** : `--taille-titre: clamp(2rem, 1.4rem + 3vw, 3.5rem)` (et le même principe pour les h2).
5. **Thème sombre** : `@media (prefers-color-scheme: dark)` dans `tokens.css` ne redéfinit **que des
   variables** — aucune règle de mise en page n'est dupliquée.

## Vérifier soi-même

```sh
# validation W3C
curl -s -H "Content-Type: text/html; charset=utf-8" --data-binary @index.html \
  "https://validator.w3.org/nu/?out=json"

# aucune media query de largeur dans le CSS
grep -rn "min-width\|max-width:" css/ | grep -v "max-width: var"
```

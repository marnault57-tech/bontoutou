# Site Bon Toutou : mode d'emploi pour les mises à jour

Site vitrine des livres et produits « Bon Toutou » de Mathieu. Tout le site tient dans `index.html` (HTML, CSS et JS dans un seul fichier, pas de build).
Chaque `git push` sur `main` met le site en ligne automatiquement :
- https://bontoutou-plum.vercel.app (adresse principale)
- https://marnault57-tech.github.io/bontoutou/

## Sources des produits
- Boutique Etsy : https://www.etsy.com/fr/shop/BonToutouEditions (PDF à imprimer)
- Page auteur Amazon : https://www.amazon.fr/e/B0HR225QPC (auteur « Mathieu A » ; livres brochés et ebooks Kindle)
- Amazon et Etsy bloquent les requêtes directes : les lire avec Firecrawl (`firecrawl_scrape`, format `json`, `maxAge: 0`).
- Un produit est identifié par son numéro d'annonce Etsy (`etsy.com/fr/listing/<numéro>`) ou son ASIN Amazon (`amazon.fr/dp/<ASIN>`). Un produit est « nouveau » si ce numéro ou cet ASIN n'apparaît nulle part dans `index.html`.

## Organisation de la page (dans l'ordre)
1. `header.hero` : haut de page, avec le bandeau `a.noel` (à retirer après le 25 décembre).
2. `section#broches` : « Nos livres brochés », une étagère `ul.shelf` avec un `li` par livre broché Amazon.
3. `section#guides` : les guides numérotés 01, 02, 03 (PDF Etsy + Kindle Amazon).
4. `section#enfants` : livres pour enfants (album, Enquête de l'Avent, coloriage).
5. `section#imprimables` : « Le carnet et le quiz », produits pour les maîtres, grille `guides duo`.
6. Outils gratuits, approche, à propos, FAQ, fin de page, pied de page.

Où ranger un nouveau produit :
- Livre ou jeu pour enfants : `#enfants`.
- Guide d'éducation numéroté : `#guides` (numéro suivant, `div.num`).
- Produit pour les maîtres (carnet, quiz, jeu, cadeau) : `#imprimables`.
- Si c'est un livre broché Amazon, l'ajouter AUSSI à l'étagère `#broches`.
- Si une même œuvre existe sur Etsy (PDF) et sur Amazon (broché ou Kindle), c'est UNE seule fiche avec les deux boutons.

## Modèle d'une fiche produit (`article.guide`)
```html
      <article class="guide">
        <div class="cover cN">
          <img class="real" src="URL_IMAGE" alt="Couverture du livre « TITRE »" loading="lazy" decoding="async" onload="fitCover(this)" onerror="this.remove()">
          <div><div class="series">Bon Toutou · TYPE</div><div class="kind">ÂGE OU PUBLIC</div></div>
          <h3>TITRE COURT</h3>
        </div>
        <p>Une ou deux phrases qui donnent envie, tirées de la description du produit (rien d'inventé).</p>
        <p class="fmt">Livre broché · NN pages · DIMENSIONS</p>   <!-- seulement pour un broché -->
        <div class="buy"><a class="shop amz" href="https://www.amazon.fr/dp/ASIN" target="_blank" rel="noopener"><span>Broché</span>Amazon</a>
          <a class="shop etsy" href="https://www.etsy.com/fr/listing/NUMERO/slug" target="_blank" rel="noopener"><span>PDF</span>Etsy</a></div>
      </article>
```
Règles :
- Bouton Amazon toujours AVANT Etsy. Étiquette : `Broché` (livre papier) ou `Kindle` (ebook). Etsy : `PDF`.
- Image Amazon : `https://m.media-amazon.com/images/I/<ID>._SL800_.jpg`. L'ID de la couverture est la 2e image de la balise `og:image` de la page produit (après `CLa%7C1200,630%7C`). Image Etsy : la balise `og:image` de l'annonce.
- `cN` : couleur de secours si l'image ne charge pas. Les classes `.c1` à `.c8` existent ; pour une nouvelle, ajouter `.c9{background:#xxxxxx}` à côté de `.c8` dans le CSS.
- Nombre de pages et dimensions : les prendre sur la fiche Amazon (« Print length », « Dimensions »).
- Pas de prix sur le site (ils varient selon le pays et changent souvent).
- Titre court et en français ; les annonces Etsy sont parfois en anglais : utiliser le titre français du produit.

## Modèle d'un livre de l'étagère `#broches`
```html
        <li><a href="https://www.amazon.fr/dp/ASIN" target="_blank" rel="noopener">
          <span class="book"><img src="https://m.media-amazon.com/images/I/<ID>._SL500_.jpg" alt="Couverture du livre broché « TITRE »" loading="lazy" decoding="async" onerror="this.remove()"><b>TITRE</b></span>
          <strong>TITRE</strong><small>TYPE · NN pages</small></a></li>
```
L'étagère est prévue pour 4 livres par ligne ; au-delà, c'est correct aussi (nouvelle ligne).

## Après chaque modification
- Ajouter le produit à la description `<meta name="description">` et au bloc JSON-LD (`ItemList` de `Book`) si c'est un livre Amazon.
- Mettre à jour les textes qui listent les produits (FAQ « Existe-t-il des livres papier ? », intro de section) si besoin.
- Vérifier le rendu avec Playwright (bureau 1280 px et mobile 390 px, aucune largeur > viewport, pas d'erreur JS).
- Commit en français, clair (« Ajout de … »), puis `git push origin main`.
- Ne jamais supprimer un produit du site sans l'accord de Mathieu, même s'il a disparu d'Etsy ou d'Amazon : le signaler seulement.

## Améliorations hebdomadaires
Une fois par semaine, une amélioration du site est faite automatiquement. Règles :
- UNE amélioration par semaine, petite et concrète, terminée et vérifiée. Pas de refonte complète.
- Garder l'identité visuelle (couleurs, polices, logo patte, médaille), le contenu existant et tous les produits.
- Ne rien inventer : pas de faux avis, pas de fausses notes, pas de chiffres de ventes, pas de prix, pas de promesses de livraison. Les textes viennent des fiches Etsy et Amazon ou de ce que Mathieu a dit.
- Pas d'images de marques, de personnages connus ou de personnes réelles.
- Les outils payants en crédits (génération d'images, de vidéos, de voix) : seulement si l'amélioration de la semaine en a vraiment besoin, une seule génération, jamais de relance.
- Noter chaque amélioration dans AMELIORATIONS.md (date, ce qui a changé, pourquoi), et piocher l'idée suivante dans la liste d'idées de ce fichier, en commençant par le haut.
- Si une idée demande une décision de Mathieu (nouveau texte sur lui, nouvelle section importante, nom de domaine…), ne pas la faire : la lui proposer dans le message de fin.

# Flyer A5 — Prospection Web Nantes

Flyer deux faces (recto/verso), format A5 (148 × 210 mm), prêt à imprimer.

## Fichiers

```
flyer-a5/
├── flyer-front.html    # Face avant (recto)
├── flyer-back.html     # Face arrière (verso)
├── flyer.pdf           # PDF combiné recto + verso (2 pages A5)
├── thumb-*.png         # Miniatures des 3 sites démo
├── qr-portfolio.png    # QR code vers https://artlab.ovh
└── README.md           # Ce fichier
```

## Contenu

### Face avant (flyer-front.html)
- Logo + titre WebCraft Nantes
- 3 miniatures des sites démo (Salon Lumière, Barber Club, Artisan Services)
- QR code pointant vers le portfolio en ligne
- Tarifs : 149 € (one-page) / 299 € (multi-pages)
- Délai, hébergement, nom de domaine offerts
- Coordonnées (à personnaliser)

### Face arrière (flyer-back.html)
- "Pourquoi avoir un site web ?" — 3 arguments
- "Ce qu'on fait chez nous" — pitch commercial
- Badges : sur-mesure, SEO, mobile, délai, hébergement
- Processus en 4 étapes
- Témoignage client (placeholder)
- Contact (à personnaliser)

## Impression

### Chez vous (imprimante A4)
1. Ouvrez `flyer.pdf` dans votre lecteur PDF (Acrobat, Preview, Firefox…)
2. Imprimez en **RECTO-VERSO** (orientation : bord court)
   - Imprimantes HP/Lexmark : choisir "Bord court" dans les options de reliure
   - Vérifiez l'aperçu avant d'imprimer
3. Découpez au massicot si vous avez un massicot,
   ou à la règle + cutter le long des bords

### Chez un imprimeur (recommandé pour quantité)
- Format fichier : **A5** (148 × 210 mm)
- Format papier demandé : **A5** ou **A4** (avec fond perdu 3 mm)
- Mode couleur : **CMJN** ou **RGB** (les deux fonctionnent)
- Résolution : 300 DPI (déjà dans le PDF)
- Type de fichier : PDF haute résolution (le `.pdf` livré convient)

**Option recommandée :** impression sur papier couché mat 135 g/m² — rendu pro et économique.

### Impression N&B
Les deux faces sont pensées pour l'impression noir & blanc :
- Pas de fond coloré plein (gris clair `#f8f8f8` max)
- Tous les éléments utilisent des contrastes noir/gris
- Testez avant de lancer une impression couleur si vous avez un doute

## Personnalisation

1. **Ouvrez** `flyer-front.html` et `flyer-back.html` dans un éditeur (VS Code, Sublime Text…)
2. **Remplacez** :
   - `[Votre nom]` → votre prénom/nom
   - `06 XX XX XX XX` → votre vrai numéro
   - `contact@[votre-domaine].fr` → votre email
3. **Regénérez le PDF** :
   ```bash
   # Option A : Ouvrir dans Chrome et "Imprimer > Enregistrer en PDF"
   # Option B : en ligne avec l'URL data: (impossible directement)
   # Option C : chromium headless (voir ci-dessous)
   chromium --headless --disable-gpu --no-pdf-header-footer \
     --print-to-pdf=flyer.pdf --run-all-compositor-stages-before-draw \
     "file://$(pwd)/flyer-front.html"
   ```
   Puis combinez les deux PDF avec pymupdf (Python) :
   ```python
   import fitz
   f = fitz.open('flyer-front.pdf')
   f.insert_pdf(fitz.open('flyer-back.pdf'))
   f.save('flyer.pdf')
   ```

## Screenshots des sites
Les miniatures ont été générées avec chromium headless :
```
https://gougoule1.github.io/nantes-portfolio/coiffeur-vitrine/
https://gougoule1.github.io/nantes-portfolio/barbier-vitrine/
https://gougoule1.github.io/nantes-portfolio/artisan-vitrine/
```

Regénérez-les si les sites changent :
```bash
for site in coiffeur-vitrine barbier-vitrine artisan-vitrine; do
  chromium --headless --disable-gpu \
    --screenshot=thumb-${site}.png --window-size=1200,800 \
    "https://gougoule1.github.io/nantes-portfolio/${site}/"
  # Recadrer : 600x360px center crop
  python3 -c "from PIL import Image; ..."
done
```

## Notes techniques
- Fonts : Google Fonts (Inter + Playfair Display) — chargées via CDN, fonctionnent hors-ligne uniquement si le navigateur a mis en cache
- Images : miniatures des sites (PNG) + QR code — locales au dossier
- CSS print : `@page { size: 148mm 210mm; margin: 0; }` pour impression taille exacte
- Le PDF fait exactement 148 × 210 mm (A5) — vérifiable avec `pdfinfo flyer.pdf`

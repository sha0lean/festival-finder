# 09 — App Spec : Festival Finder 2026

> Spec technique de l'application `festivals-2026.html`.
> À lire avec : `03_SCHEMA.md` (schéma de données) · `05_SCORING.md` (compat & scores) · `festivals-DATA.md` (44 festivals en dur).
> Source consolidée depuis `v1 claude/festival-finder-BUILD.md` + `festival-finder-SETTINGS.md`.

---

## 1. Contraintes techniques (non négociables)

- **Un seul fichier `.html`**, ouvrable en double-clic. CSS dans `<style>`, JS dans `<script>`, aucune étape de build.
- **CDN uniquement** : Leaflet 1.9.4 (carte) + Google Fonts (Anton, JetBrains Mono, Familjen Grotesk).
- **localStorage** pour la persistance (clé `fest26`) — envelopper dans `try/catch` (bloqué dans certaines sandbox).
- **Vanilla JS** — aucun framework. Rendu par régénération de `innerHTML` via `render()` global.
- **Responsive** : burger ☰ mobile, sidebar en overlay sous 900px.
- Valider la syntaxe JS avant livraison (`new Function(scriptBody)`).

---

## 2. Design system

### Polices
| Rôle | Police |
|------|--------|
| Titres, noms de festival, scores | `Anton` (Anton, uppercase) |
| Données, labels, badges | `JetBrains Mono` |
| Corps de texte | `Familjen Grotesk` |

### Tokens CSS

```css
/* Marque */
--lime:    #c6ff3a
--magenta: #ff3ea5
--cyan:    #3ad8ff
--amber:   #ffb13a
--grey:    #9aa39a

/* Scores */
--good: #c6ff3a   /* ≥80 */
--mid:  #ffb13a   /* 60–79 */
--bad:  #ff5a5a   /* <60 */

/* Étoiles */
--star: #ffcf3a

/* Rayon */
--r: 13px

/* Thème dark (défaut) */
--bg:     #0b0d0c
--bg2:    #1a1f1c
--bg3:    #141816
--line:   #343d35
--ink:    #eef3ec
--muted:  #9aa79a
--dim:    #69746a
--accent: var(--lime)

/* Thème light */
--bg:     #f3f4ee
--bg2:    #ffffff
--bg3:    #e9ebe2
--line:   #d4d9cd
--ink:    #15201a
--muted:  #566055
--dim:    #9aa698
--accent: #2f7a00
/* Overrides light pour lisibilité */
--lime:    #3f8a00
--magenta: #cc1f76
--cyan:    #0c8ab2
--amber:   #b9760a
--good:    #3a8200
--mid:     #b9760a
--bad:     #cc3535
```

---

## 3. Architecture DOM

```
.app (grid 310px | 1fr)
├─ aside#sidebar
│  ├─ .brand + .tagline
│  ├─ sect Recherche & tri (input#q, select#sort)
│  ├─ sect Profil de goûts (#profTabs, input#profName, #weightWrap, #profNote)
│  ├─ sect Filtres
│  │     selects: month, season, venue, swim, com, starf
│  │     sliders: dist max, budget max, compat min
│  │     chips: #genreChips [15 styles] · #catChips [catégories dérivées]
│  │     .reset
│  └─ sect Légende (#styleLegend, scores, note compat)
└─ main
   ├─ .topbar (h1 + span#sub + #mapToggle + #calToggle + #themeToggle)
   ├─ div#map (Leaflet)
   ├─ div#calwrap (#calViewSeg [Tous/Favoris/Comparés] + div#cal)
   ├─ .sortbar (span#cnt + button#ratedToggle CTA + hint)
   └─ .grid#grid (cartes festival)
div.cmpbar#cmpbar   (barre flottante comparateur)
div.modal#modal     (comparatif + combo + timeline + tableau)
```

---

## 4. Modèle de données JS (objet festival `F`)

Les objets `F` utilisent des clés courtes pour minimiser la taille inline. Correspondance avec le schéma kit (03_SCHEMA.md) :

| Clé JS | Champ kit | Type |
|--------|-----------|------|
| `n` | `name` | string |
| `c` | `country_flag` | emoji |
| `r` | `city_region` | string |
| `gps` | `[lat, lng]` | array |
| `date` | libellé humain | string |
| `month` | `Juin`\|`Juillet`\|… | string |
| `ds` | jour de début (1–31) | int |
| `dur` | `duration_days` | int |
| `season` | `season` | `été`\|`arrière-saison`\|`automne` |
| `venue` | `event_type` | `outdoor`\|`indoor` |
| `rt` | `drive_km_return` | int |
| `h` | `drive_hours_oneway` | float |
| `et` | `road_steps` | `"0"`\|`"1"`\|`"1-2"`\|`"2+"` |
| `bk` | `genres_pct` | objet {15 clés → %} |
| `swim` | `swimming.allowed` | `oui`\|`proche`\|`non` |
| `swimT` | `"type/loc/autorisé/accès"` | string |
| `com` | `commercial_level` | `faible`\|`moyen`\|`élevé` |
| `size` | `capacity_approx` | string |
| `scene` | `setting_tags` | array |
| `cat` | — (inutilisé, dérivé au runtime) | array |
| `heat` | bool 🔥 | bool |
| `sleep` | couchage indicatif | string |
| `risk` | risque billetterie | `faible`\|`moyen`\|`élevé` |
| `cadre` | `cadre` | string |
| `meteo` | météo type (indicatif) | string |
| `price` | prix billet indicatif | string |
| `tot` | `estimated_budget_2p` | int |
| `budget` | `[[label, montant], …]` | array |
| `nat` | `nature_score` | int/100 |
| `bud` | `budget_score` | int/100 |
| `conf` | `comfort_score` | int/100 |
| `glob` | `global_score` | int/100 |
| `pros` | `pros` | string |
| `cons` | `cons` | string |
| `warn` | `warning` | string\|null |
| `lineup` | `headline_artists` | string |
| `src` | `source_type` | string |
| `url` | `source_url` | string |

**Calculés au runtime (ne pas stocker dans `F`) :**
- `swimType` = 1er segment de `swimT` (avant `/`)
- `doy = MO[month] + ds` avec `MO = {Juin:151, Juillet:181, Août:212, Septembre:243, Octobre:273, Novembre:304}`
- `doyEnd = doy + dur - 1`
- Compatibilité : calculée en direct, jamais stockée

**Convention `bk` :** ne lister que les styles non nuls ; la somme doit faire ~100.

---

## 5. Les 15 styles musicaux (clés `bk` + table JS `STY`)

```js
STY = [
  ["dnb",     "Drum & Bass",         10, "#c6ff3a"],
  ["neuro",   "Neurofunk",           10, "#8ee000"],
  ["jungle",  "Jungle",               8, "#49d96b"],
  ["liquid",  "Liquid DnB",           8, "#6effc0"],
  ["dub",     "Dub / Sound-System",  10, "#ff3ea5"],
  ["bass",    "Dubstep / Bass",       7, "#ff85c8"],
  ["psy",     "Psytrance",            8, "#3ad8ff"],
  ["darkpsy", "Darkpsy / Hi-Tech",    8, "#5b8cff"],
  ["forest",  "Forest",               8, "#27c2b0"],
  ["fullon",  "Full-On / Goa",        6, "#86d4ff"],
  ["acid",    "Acid",                 8, "#ffb13a"],
  ["techno",  "Techno / Hard",        8, "#ff7a2f"],
  ["house",   "House",                4, "#ffd84a"],
  ["melodic", "Techno mélodique",     2, "#b59b7a"],
  ["other",   "Autres / mainstream",  1, "#9aa39a"],
]
```

---

## 6. Formule de compatibilité

```js
// Poids W = tableau des poids du profil actif (index aligné sur STY)
compatW(f, W) = round( Σ_k ( (f.bk[STY[k][0]] || 0) × W[k] ) / 10 )

// Profil "Moyenne" : moyenne des compatW sur tous les profils
compatOf(f) = prof === 'avg'
  ? average( profiles.map(p => compatW(f, p.w)) )
  : compatW(f, profiles[prof].w)
```

`glob` (score global) = note pré-évaluée à la main, **fixe**, distincte de `compatOf`. Formule : voir `05_SCORING.md`.

---

## 7. Catégories dérivées (prédicats, jamais manuels)

| Chip | Règle |
|------|-------|
| 🎚 Hybride | `≥ 3 styles à ≥ 15 %` |
| 🌊 Baignade | `swim === 'oui'` |
| 📍 Proche (≤1100 km) | `rt ≤ 1100` |
| 💶 Petit budget | `tot ≤ 1200` |
| 🌲 Non commercial | `com === 'faible'` |
| 🏛 Indoor / city | `venue === 'indoor'` |

- Filtre style = **OR** sur `bk[style] ≥ 8 %`
- Filtre catégorie = **OR** (le festival passe s'il matche ≥ 1 catégorie cochée)

---

## 8. Table drapeaux → pays (`CN`)

```js
CN = {
  "🇨🇿":"Tchéquie", "🇭🇷":"Croatie",   "🇮🇹":"Italie",      "🇳🇱":"Pays-Bas",
  "🇧🇪":"Belgique",  "🇦🇹":"Autriche",  "🇩🇪":"Allemagne",   "🇫🇷":"France",
  "🇪🇸":"Espagne",   "🇬🇷":"Grèce",     "🇬🇧":"Royaume-Uni", "🇭🇺":"Hongrie",
  "🇸🇮":"Slovénie",  "🇵🇱":"Pologne",   "🇨🇭":"Suisse"
}
```

---

## 9. Spec comportement — composant par composant

### Sidebar — Profil de goûts
- Onglets dynamiques : un par personne + « Moyenne » + « ＋ personne ».
- Personne active : nom éditable (`oninput` → pas de re-render pour garder le focus ; `onchange` → re-render onglets) + 15 curseurs (0–10, pas de 0.5) avec valeur affichée live.
- « ＋ personne » : ajoute `{name:'Personne N', w: clone(W0)}` et la sélectionne.
- « × » : supprime le profil (toujours ≥ 1 profil). Réajuster l'index actif.
- « Moyenne » : masque curseurs + nom, affiche note ; compat = moyenne de tous les profils.
- Tout changement → `save()` + `render()`.

### Sidebar — Filtres
- Selects : Mois · Saison · Type (outdoor/indoor) · Baignade (oui/proche/non) · Commercial · Mes étoiles (5→1).
- Sliders : Distance A/R max (600–4000, ∞ au max) · Budget max (600–2700, ∞) · Compatibilité min (0–95). Afficher la valeur live.
- Chips 15 styles (présence `bk[k] ≥ 8 %`) + chips catégories dérivées. Toggle classe `.on`.
- Bouton Reset : efface localStorage + hash + `location.reload()`.

### Carte Leaflet (3 états)
- Bouton `🗺️ Carte ＋/−` cycle : masquée(0) → moitié 360px(1) → plein 82vh(2) → 0.
- `map.invalidateSize()` après tout changement de hauteur.
- Marqueurs `circleMarker` colorés par style dominant. Point Genève rose/magenta.
- Survol = tooltip large (nom, dates, durée, scène, compat, km A/R, baignade).
- Clic marqueur = `route()` (polyline pointillé Genève→festival + `fitBounds`) + `state.sel.add(id)` + remonte en tête de `pinned` + `render()` **sans scroll**.
- Tuiles OSM filtrées CSS en dark : `invert(1) hue-rotate(180deg) brightness(.92) contrast(.9) saturate(.85)`.

### Calendrier (2 états, cases-jours)
- Bouton `📅 Calendrier ＋/−` : afficher/masquer. **Pas de plein écran** (ancien anti-pattern retiré).
- Layout ligne : `[nom+dates 160px] [piste flex de cases-jours]`.
- 1 case = 1 jour ; cases allumées du `doy` au `doyEnd` (couleur = style dominant du festival).
- En-tête sticky : n° jour du mois si période ≤ 46 j, sinon abréviations de mois (au 1er du mois).
- **Overlay lignes continues** (`.cal-grid` absolu, `left = colonne-nom + gap`) : début de mois = trait accent 2px, lundis = trait fin 1px atténué. **Aucune bordure par-case** (effet pointillé = anti-pattern).
- Sélecteur **Tous / Favoris ★ / Comparés ☑** filtre les lignes affichées.
- Variable CSS `--calname` : 160px desktop / 104px mobile.

### Cartes festival (ordre de haut en bas)
1. ★★★★★ (22px, sans label)
2. Case à cocher comparateur (absolue, haut droite)
3. Nom (`Anton`, uppercase)
4. `🇫🇷 France · ville/région` (drapeau + **pays en toutes lettres**, 12.5px)
5. Dates + badge 🟢 CONFIRMÉ / INDOOR
6. 4 tuiles score (Compatibilité · Nature · Budget · Global)
7. Barre styles (segments colorés par style)
8. Météo
9. Badges (baignade · route · durée · type · commercial · 🔥 · risque billetterie · budget)
10. Détail masqué par défaut

- Clic corps de carte = épingle (remonte en tête) + déploie le détail. Étoiles & case exclues du clic.
- Carte cochée = liseré magenta · épinglée = liseré accent en haut · notée = liseré or à gauche.
- Détail : cadre · baignade · couchage · table budget 2p · +/− · alerte ⚠ · line-up (`<details>`) · source.
- **Pas de footer « détails »** sur la carte — un clic suffit.

### Comparateur & combo (modale)
- Barre flottante dès 1 coché : `"Comparer & planifier (N) · Σ budget €"`.
- Modale : verdict 17 j (jours festival + route estimée depuis `h` ; conflits via chevauchement `doy/doyEnd`).
- 4 stats combo : budget cumulé vs 1500 € cible · km · jours · nb festivals.
- Calendrier timeline des sélectionnés (chevauchements en rose).
- Tableau comparatif : meilleure valeur verte / pire rouge. Inclut styles détaillés (DnB, Neuro, Dub, Psy, Acid, Techno %).

### Étoiles & sélection
- 5 étoiles, clic = note 1–5 ; re-clic même valeur = retire la note.
- Tri « Mon classement ★ » (5→1 puis global).
- Filtre « Mes étoiles » (valeur exacte) + CTA doré `★ Ma sélection (N)` (n'affiche que les notés).

---

## 10. Persistance (localStorage)

```js
clé "fest26" = {
  theme,          // "dark" | "light"
  profiles,       // [{name, w:{dnb,neuro,…}}]
  prof,           // index actif ou "avg"
  pinned,         // [id, …]
  sel,            // [id, …]  (comparateur)
  ratings         // {id: 1–5}
}
```
`load()` au démarrage → vérifier qu'un champ style récent existe (`'neuro' in profiles[0].w`) avant d'appliquer un ancien format.

---

## 11. Anti-patterns documentés (leçons — ne pas refaire)

| ❌ Éviter | ✅ Faire |
|-----------|---------|
| Lumper des genres distincts (dub+bass, acid+techno) | 15 styles granulaires avec % |
| Générer les filtres de genre depuis `scene` tags | Filtrer via les `%` dans `bk` |
| Tags de saison incohérents | Saisons strictes : été juin–août · arrière-saison sept · automne oct–nov |
| Séparateurs de mois en bordures par-case | Overlay continu (`.cal-grid`) |
| Plein écran calendrier sans sortie évidente | 2 états uniquement (afficher/masquer) |
| Scroller la page au clic marqueur | `render()` sans scroll, pin en tête |
| Inventer dates/URL/GPS | Anti-hallucination : 🟢/🟡 · "non trouvée" si absent |
| Labels répétés inutiles ("mettre de côté", footer "détails") | CTA doré unique "Ma sélection" |
| Profils couple figé (Toi/Partenaire) | Profils multi-personnes dynamiques |
| Catégories A–F manuelles | Catégories dérivées automatiquement |

---

## 12. Checklist de build

**Fondations**
- [ ] Squelette HTML + `<style>` (tokens dark/light) + `<script>` + fonts CDN + Leaflet CDN.
- [ ] Tableau `F[]` (44+ festivals depuis `festivals-DATA.md`) + `F.forEach` → `doy/doyEnd/swimType`.
- [ ] Constantes : `STY`, `STYLE` (map clé→index), `W0` (poids défaut), `CN` (drapeaux→pays), `MO` (mois→doy).
- [ ] Utilitaires : `sc(v)` (couleur score), `numBud(tot)` (tranche budget).

**État & rendu**
- [ ] `state` : `{q, sort, filters, genres[], cats[], sel[], pinned[], ratings{}, ratedOnly, calMode, calView, mapSize, profiles[], prof}`.
- [ ] `compatW(f, W)` · `compatOf(f)` · `pass(f)` (filtre global) · `sorter` · `render()`.
- [ ] `save()` / `load()`.

**Sidebar**
- [ ] Recherche + tri (dont « Mon classement ★ »).
- [ ] Profils multi-personnes (`renderProfiles`).
- [ ] Filtres + sliders + chips styles + chips catégories + Reset.
- [ ] Légende styles (générée depuis `STY`) + scores + note compat.

**Zone principale**
- [ ] Carte Leaflet + marqueurs + tooltip + `route()` + clic sans scroll.
- [ ] Bouton Carte 3 états (`applyMap`).
- [ ] Calendrier cases-jours + en-tête sticky + overlay lignes (`buildCalendar`).
- [ ] Bouton Calendrier 2 états (`applyCal`) + sélecteur Tous/Favoris/Comparés.
- [ ] Cartes festival (étoiles, pays toutes lettres, badges, détail au clic).
- [ ] Épinglage : `togglePin` (avec scroll) · `markerPick` (sans scroll).
- [ ] Étoiles 1–5 + filtre + CTA Ma sélection.

**Comparateur**
- [ ] Cases à cocher + barre flottante + Σ budget.
- [ ] Modale : verdict 17 j + stats combo + timeline + tableau comparatif.

**Finitions**
- [ ] Responsive + burger mobile.
- [ ] Validation JS (`new Function(scriptBody)`) + vérification sommes `bk` = 100.
- [ ] Aucune URL inventée · dates flaggées 🟢/🟡.

---

## 13. Backlog (idées non implémentées)

- Fond léger pour samedi/dimanche dans le calendrier.
- Sous-genres fins en filtre (nécessite données exhaustives sourcées par festival).
- Synchro multi-appareils (nécessite hébergement + backend).
- Itinéraire chaîné multi-festivals (route optimisée).
- Alertes/échéances de billetterie datées.
- Overlay disponibilités personnelles sur le calendrier (voir `08_VOYAGE.md` §2).
- Couche POI en route — marqueurs distincts, toggle on/off (voir `08_VOYAGE.md` §3).

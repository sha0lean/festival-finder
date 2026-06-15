# 🧱 Schéma de données — Festivals 2026

> Format canonique pour la carte interactive.
> Markdown = édition humaine · YAML/JSON = export carte.

---

## Champs normalisés

| Champ | Type | Exemple | Obligatoire | Notes |
|---|---|---|---:|---|
| `id` | string | `let-it-roll-2026` | ✅ | slug stable, kebab-case |
| `name` | string | `Let It Roll` | ✅ | nom officiel |
| `country_code` | string | `CZ` | ✅ | ISO 2 lettres |
| `country_flag` | string | `🇨🇿` | ✅ | affichage UI |
| `city_region` | string | `Milovice, Bohême centrale` | ✅ | visible popup |
| `lat` | number | `50.238` | ✅ | marker carte |
| `lng` | number | `14.897` | ✅ | marker carte |
| `start_date` | date | `2026-07-30` | ✅ | format ISO 8601 |
| `end_date` | date | `2026-08-02` | ✅ | format ISO 8601 |
| `date_status` | enum | `confirmed` / `estimated` / `unknown` | ✅ | 🟢 / 🟡 / 🔴 |
| `source_url` | url | `https://letitroll.eu` | ✅ | source officielle ou "non trouvée" |
| `source_type` | enum | `official` / `ticketing` / `social` / `secondary` | ✅ | fiabilité de la source |
| `season` | enum | `ete` / `arriere_saison` / `automne` | ✅ | filtre calendrier |
| `event_type` | enum | `outdoor` / `indoor` / `city_event` / `hybrid` | ✅ | filtre type |
| `duration_days` | number | `4` | ✅ | durée |
| `capacity_approx` | number/null | `15000` | 🟡 | taille estimée |
| `commercial_level` | enum | `faible` / `moyen` / `eleve` | ✅ | filtre ambiance |
| `comfort_score` | number | `70` | ✅ | 0–100 |
| `compat_score` | number | `93` | ✅ | 0–100, calculé depuis `05_SCORING.md` |
| `nature_score` | number | `82` | ✅ | 0–100, calculé depuis `05_SCORING.md` |
| `budget_score` | number | `75` | ✅ | 0–100, calculé depuis `05_SCORING.md` |
| `global_score` | number | `88` | ✅ | 0–100, calculé depuis `05_SCORING.md` |
| `drive_km_return` | number/null | `2000` | ✅ | A/R depuis Genève |
| `drive_hours_oneway` | number/null | `11` | ✅ | aller réaliste |
| `road_steps` | enum | `0` / `1` / `1-2` / `2+` | ✅ | étapes nuit recommandées |
| `estimated_budget_2p` | number/null | `1250` | ✅ | couple, tout inclus |
| `ticket_price_min` | number/null | `140` | 🟡 | €/pers early bird |
| `ticket_price_max` | number/null | `180` | 🟡 | €/pers standard |
| `swim_type` | enum | `aucune` / `lac` / `riviere` / `mer` / `reservoir` / `plan_eau` | ✅ | filtre baignade |
| `swim_location` | enum | `site` / `proche` / `aucune` | ✅ | proximité |
| `swim_allowed` | enum | `oui` / `non` / `inconnu` | ✅ | fiabilité |
| `swim_access` | enum | `a_pied` / `voiture` / `aucun` | ✅ | accès |
| `setting_tags` | array | `["forêt", "lac", "underground"]` | ✅ | badges visuels |
| `genre_tags` | array | `["DnB", "Neuro", "Jungle"]` | ✅ | badges visuels |
| `headline_artists` | array | `["Noisia", "Black Sun Empire"]` | 🟡 | popup line-up |
| `pros` | string | `DnB massif, forêt, baignade sur site` | ✅ | point fort principal |
| `cons` | string | `loin, camping vite plein` | ✅ | point faible principal |
| `warning` | string/null | `camping prairie sold-out rapidement` | 🟡 | alerte |
| `map_ready` | boolean | `false` | ✅ | true = toutes données L1+L2 complètes |
| `source_checked_at` | date | `2026-06-13` | ✅ | fraîcheur des données |

---

## Répartition musicale — 15 styles granulaires

**La somme des 15 styles doit faire exactement 100. Pas d'exception.**
Un style ne figure que s'il représente une part significative du programme (pas une heure sur un stage secondaire).

```yaml
genres_pct:
  dnb: 0
  neuro: 0
  jungle: 0
  liquid: 0
  dub: 0
  bass: 0
  psy: 0
  darkpsy: 0
  forest: 0
  fullon: 0
  acid: 0
  techno: 0
  house: 0
  melodic: 0
  other: 0
  # TOTAL: 100
```

### Familles dérivées (calculées automatiquement, pour filtres carte)

Ces valeurs ne sont **jamais saisies manuellement** — elles sont calculées depuis `genres_pct` :

```yaml
families_pct:
  dnb_family: 0       # dnb + neuro + jungle + liquid
  psy_family: 0       # psy + darkpsy + forest + fullon
  sound_system: 0     # dub + bass
  acid_techno: 0      # acid + techno
```

---

## Structure YAML complète

```yaml
id: "festival-slug-2026"
name: "Festival Name"
country_code: "CZ"
country_flag: "🇨🇿"
city_region: "Ville, Région"
lat: 50.238
lng: 14.897
start_date: "2026-07-30"
end_date: "2026-08-02"
date_status: "confirmed"   # confirmed | estimated | unknown
source_url: "https://..."
source_type: "official"    # official | ticketing | social | secondary
source_checked_at: "2026-06-13"
season: "ete"              # ete | arriere_saison | automne
event_type: "outdoor"      # outdoor | indoor | city_event | hybrid
duration_days: 4
capacity_approx: null
commercial_level: "faible" # faible | moyen | eleve
comfort_score: 70
compat_score: null         # calculé depuis 05_SCORING.md
nature_score: null
budget_score: null
global_score: null
drive_km_return: null      # calculé depuis Genève
drive_hours_oneway: null
road_steps: null           # 0 | 1 | 1-2 | 2+
estimated_budget_2p: null  # couple, tout inclus
ticket_price_min: null
ticket_price_max: null
swimming:
  type: "aucune"           # aucune | lac | riviere | mer | reservoir | plan_eau
  location: "aucune"       # site | proche | aucune
  allowed: "inconnu"       # oui | non | inconnu
  access: "aucun"          # a_pied | voiture | aucun
genres_pct:
  dnb: 0
  neuro: 0
  jungle: 0
  liquid: 0
  dub: 0
  bass: 0
  psy: 0
  darkpsy: 0
  forest: 0
  fullon: 0
  acid: 0
  techno: 0
  house: 0
  melodic: 0
  other: 0
genre_tags: []
setting_tags: []
headline_artists: []
pros: ""
cons: ""
warning: null
map_ready: false
```

---

## Exemple JSON propre — Let It Roll (référence)

```json
{
  "id": "let-it-roll-2026",
  "name": "Let It Roll",
  "country_code": "CZ",
  "country_flag": "🇨🇿",
  "city_region": "Milovice, Bohême centrale",
  "lat": 50.238,
  "lng": 14.897,
  "start_date": "2026-07-30",
  "end_date": "2026-08-02",
  "date_status": "estimated",
  "source_url": "https://letitroll.eu",
  "source_type": "official",
  "source_checked_at": "2026-06-13",
  "season": "ete",
  "event_type": "outdoor",
  "duration_days": 4,
  "capacity_approx": 30000,
  "commercial_level": "moyen",
  "comfort_score": 72,
  "compat_score": 89,
  "nature_score": 15,
  "budget_score": 60,
  "global_score": 62,
  "drive_km_return": 2000,
  "drive_hours_oneway": 11,
  "road_steps": "1-2",
  "estimated_budget_2p": 1400,
  "ticket_price_min": 130,
  "ticket_price_max": 160,
  "swimming": {
    "type": "aucune",
    "location": "aucune",
    "allowed": "non",
    "access": "aucun"
  },
  "genres_pct": {
    "dnb": 40,
    "neuro": 25,
    "jungle": 10,
    "liquid": 10,
    "dub": 0,
    "bass": 5,
    "psy": 0,
    "darkpsy": 0,
    "forest": 0,
    "fullon": 0,
    "acid": 0,
    "techno": 5,
    "house": 0,
    "melodic": 0,
    "other": 5
  },
  "genre_tags": ["DnB", "Neuro", "Jungle", "Liquid"],
  "setting_tags": ["plaine", "open-air", "grande scène"],
  "headline_artists": ["Noisia", "Mefjus", "Forbidden Society"],
  "pros": "meilleure concentration DnB/neuro d'Europe, line-up de référence",
  "cons": "grand, commercial, camping basique, loin",
  "warning": null,
  "map_ready": true
}
```

---

## Niveaux de complétude — Règle map_ready

### Niveau 1 — indispensable pour afficher un marker

`id` · `name` · `country_code` · `city_region` · `lat` · `lng` · `start_date` · `end_date` · `date_status` · `source_url` · `event_type` · `season`

### Niveau 2 — indispensable pour les filtres

`genres_pct` (somme = 100) · `compat_score` · `global_score` · `commercial_level` · `comfort_score` · `swimming.*` · `drive_km_return` · `drive_hours_oneway` · `estimated_budget_2p`

### Niveau 3 — enrichissement popup

`headline_artists` · `capacity_approx` · `setting_tags` · `ticket_price_min/max` · `pros` · `cons` · `warning`

Un festival passe en `map_ready: true` uniquement quand L1 + L2 sont complets.

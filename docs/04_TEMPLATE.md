# 🧩 Template — Fiche festival

> Copier-coller ce template pour chaque nouveau festival.
> Les champs marqués `[AUTO]` sont calculés par l'app depuis Genève — laisser vides.

---

## Version Markdown (édition humaine)

```markdown
## 🎪 NOM DU FESTIVAL 🇺🇳

**Pays :** Nom du pays
**Région / Ville :** Région, Ville
**GPS :** `lat, lng` *(approx)* ou `lat, lng` *(vérifié Google Maps)*
**Dates 2026 :** 🟢 DD mois – DD mois 2026 *(confirmées)* ou 🟡 ~DD mois 2026 *(estimation éditions précédentes)*
**Durée :** X jours
**Type :** outdoor / indoor / city_event / hybrid
**Saison :** été / arrière-saison / automne
**Édition :** Xème édition ou Première édition ou Inconnue

---

### 🎧 Styles musicaux

> Somme OBLIGATOIRE = 100. Ne remplir que les styles significatifs dans la programmation.
> Source : line-up officiel ou page artistes.

| Style | % |
|---|---:|
| dnb | 0 |
| neuro | 0 |
| jungle | 0 |
| liquid | 0 |
| dub | 0 |
| bass | 0 |
| psy | 0 |
| darkpsy | 0 |
| forest | 0 |
| fullon | 0 |
| acid | 0 |
| techno | 0 |
| house | 0 |
| melodic | 0 |
| other | 0 |
| **TOTAL** | **100** |

---

### 🌿 Cadre & logistique

**Cadre :** forêt / lac / rivière / plage / montagne / urbain / industriel / campagne *(préciser)*
**Baignade :** oui sur site / proche à pied / proche en voiture / non / inconnu
**Taille :** petit (< 3 000) / moyen (3 000–10 000) / grand (10 000–30 000) / très grand (> 30 000)
**Ambiance :** alternative / tribe / forest / underground / structurée / familiale / commerciale
**Commercial :** faible / moyen / élevé

**Camping :** oui / non / options premium disponibles
**Parking :** sur site / navette / payant séparé / inconnu
**Sanitaires / douches :** fiables / basiques / inconnu

---

### 💶 Tarifs

**Prix billet :** ~XX€ early bird / ~XX€ standard *(ou "non trouvé")*
**Prix camping :** ~XX€ ou inclus *(ou "non trouvé")*

---

### 🚗 Route & budget [AUTO — calculé par l'app]

**Distance A/R depuis Genève :** [AUTO]
**Temps aller :** [AUTO]
**Étapes nuit :** [AUTO]
**Budget essence :** [AUTO] (× 0,19 €/km)
**Péages / vignettes :** [AUTO]
**Budget total 2p :** [AUTO]

---

### 📊 Scores

**Score compat :** [AUTO]
**Score nature :** [AUTO]
**Score budget :** [AUTO]
**Score confort :** [calculer manuellement via 05_SCORING.md — grille confort]
**Score global :** [AUTO]

---

### 🔗 Sources

**URL officielle :** https://... *(ou "non trouvée")*
**Resident Advisor :** https://ra.co/events/... *(ou "non trouvé")*
**Billetterie :** https://... *(ou "non trouvée")*
**Source type :** official / ticketing / social / secondary

**Statut recherche :** 🟢 Vérifié / 🟡 Partiel / 🔴 Non trouvé

---

### 📝 Notes

**+ Point fort :** ...
**− Point faible :** ...
**⚠️ Warning :** ... *(supprimer si rien)*

<details><summary>🎧 Line-up</summary>
[Têtes d'affiche uniquement — ou "non annoncé"]
</details>
```

---

## Version YAML (export carte)

```yaml
id: "festival-slug-2026"
name: "Festival Name"
country_code: "XX"
country_flag: "🇽🇽"
city_region: "Ville, Région"
lat: 0.000
lng: 0.000
start_date: "2026-XX-XX"
end_date: "2026-XX-XX"
date_status: "unknown"     # confirmed | estimated | unknown
source_url: "non trouvée"
source_type: "official"    # official | ticketing | social | secondary
source_checked_at: "2026-06-13"
season: "ete"              # ete | arriere_saison | automne
event_type: "outdoor"      # outdoor | indoor | city_event | hybrid
duration_days: null
capacity_approx: null
commercial_level: "faible" # faible | moyen | eleve
comfort_score: null
compat_score: null         # [AUTO]
nature_score: null         # [AUTO]
budget_score: null         # [AUTO]
global_score: null         # [AUTO]
drive_km_return: null      # [AUTO]
drive_hours_oneway: null   # [AUTO]
road_steps: null           # [AUTO] 0 | 1 | 1-2 | 2+
estimated_budget_2p: null  # [AUTO]
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
  # TOTAL: 0  ← doit faire 100 avant map_ready: true
genre_tags: []
setting_tags: []
headline_artists: []
pros: ""
cons: ""
warning: null
map_ready: false
```

---

## Règles de remplissage

| Règle | Détail |
|---|---|
| `genres_pct` somme = 100 | Obligatoire avant toute intégration |
| Pas d'URL inventée | Si non trouvée → écrire `"non trouvée"` |
| `date_status: confirmed` | Seulement si source officielle 2026 trouvée |
| `swim_allowed: inconnu` | Par défaut si pas de source claire |
| `map_ready: true` | Uniquement si L1 + L2 complets (voir 03_SCHEMA) |
| `[AUTO]` | Jamais remplir ces champs manuellement |

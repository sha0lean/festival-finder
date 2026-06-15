# 🗂️ Kit de recherche — Festival Finder 2026

> Kit fusionné — meilleur de Claude + GPT
> Départ : Genève / Haute-Savoie · Volvo V40 · 0,19 €/km
> Fenêtre : juin → novembre 2026

---

## Fichiers du kit

| Fichier | Rôle |
|---|---|
| `01_STATUS.md` | État actuel de la base : festivals intégrés, lots, leads |
| `02_GAPS.md` | Gaps par style et par zone + matrice de priorités |
| `03_SCHEMA.md` | Schéma de données normalisé (15 styles + YAML/JSON) |
| `04_TEMPLATE.md` | Template fiche par festival (Markdown + YAML) |
| `05_SCORING.md` | Grille de scoring : compat / nature / budget / confort / global |
| `06_PROMPTS.md` | Prompts prêts à coller pour trouver et vérifier des festivals |
| `07_SOURCES.md` | Sources, labels, promoteurs et requêtes web par genre/zone |
| `08_VOYAGE.md` | Planning perso (disponibilités, créneaux) + couche POI en route (future) |
| `09_APP-SPEC.md` | Spec technique de l'app HTML (design system, DOM, composants, anti-patterns) |

---

## Workflow recommandé

1. **Identifier les trous** → `02_GAPS.md`, matrice P1/P2/P3
2. **Générer des candidats** → `06_PROMPTS.md`, Prompt 1 ou Prompt 2
3. **Vérifier chaque candidat** → `06_PROMPTS.md`, Prompt 3
4. **Remplir la fiche** → `04_TEMPLATE.md`
5. **Calculer les scores** → `05_SCORING.md`
6. **Vérifier les sources** → `07_SOURCES.md`
7. **Mettre à jour l'état** → `01_STATUS.md`

---

## Convention de statut

| Statut | Signification |
|---|---|
| 🟢 confirmé | dates + source officielle 2026 trouvée |
| 🟡 estimation | date probable basée sur éditions précédentes, pas encore officielle |
| 🔴 à vérifier | donnée trop incertaine, ne pas intégrer à la carte |
| `n/c` | donnée absente, à compléter |
| `?` | donnée partielle, à confirmer |

---

## Les 15 styles musicaux

**Règle absolue : la somme des 15 styles doit faire exactement 100 pour chaque festival.**
Un style n'est inclus que s'il représente une part significative de la programmation — pas une heure sur un stage annexe.

| Style | Description |
|---|---|
| `dnb` | Drum & Bass classique / big room |
| `neuro` | Neurofunk — DnB sombre, complexe (Eatbrain, Blackout) |
| `jungle` | Jungle / old school breakbeat DnB |
| `liquid` | Liquid DnB — mélodique, fluide (Hospitality, Sofa Sound) |
| `dub` | Dub / Sound System — reggae deep, 45 rpm |
| `bass` | Bass music large — dubstep sub, bass engineering |
| `psy` | Psytrance classique / full power |
| `darkpsy` | Darkpsy — psytrance sombre, chaosbass |
| `forest` | Forest psy — psychédélique, organique, obscur |
| `fullon` | Full-On — psytrance mélodique, plus commercial |
| `acid` | Acid — TB-303, rave culture |
| `techno` | Techno — Berlin/Detroit/industriel |
| `house` | House — deep, tech house, afrohouse |
| `melodic` | Techno/house mélodique — Afterlife, éthéré |
| `other` | Tout le reste (ambient, world, pop, etc.) |

### Familles de filtres (carte uniquement — pas dans les fiches)

Ces familles sont **calculées automatiquement** depuis les 15 styles granulaires. Elles servent uniquement aux filtres visuels de la carte — les données source restent granulaires.

| Famille filtre | = somme de | Pourquoi ce regroupement |
|---|---|---|
| `dnb_family` | dnb + neuro + jungle + liquid | Même scène, même BPM (~170), même culture |
| `psy_family` | psy + darkpsy + forest + fullon | Même BPM (~145), même esthétique psychédélique |
| `sound_system` | dub + bass | Culture sound system et basses profondes |
| `acid_techno` | acid + techno | Cousins historiques, même underground électronique |

`house`, `melodic`, `other` → standalone, pas regroupés.

---

## Goûts musicaux — référence scoring

| Style | Poids |
|---|---:|
| dnb | 10 |
| neuro | 10 |
| dub | 10 |
| jungle | 8 |
| liquid | 8 |
| psy | 8 |
| darkpsy | 8 |
| forest | 8 |
| acid | 8 |
| techno | 8 |
| bass | 7 |
| fullon | 6 |
| house | 4 |
| melodic | 2 |
| other | 1 |

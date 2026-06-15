# 📊 Grille de scoring — Compat / Nature / Budget / Confort / Global

---

## Score de compatibilité musicale

Basé sur les 15 styles granulaires et les goûts personnels.

### Formule

```
compat_score = (
  dnb      × 10 +
  neuro    × 10 +
  dub      × 10 +
  jungle   ×  8 +
  liquid   ×  8 +
  psy      ×  8 +
  darkpsy  ×  8 +
  forest   ×  8 +
  acid     ×  8 +
  techno   ×  8 +
  bass     ×  7 +
  fullon   ×  6 +
  house    ×  4 +
  melodic  ×  2 +
  other    ×  1
) / 10
```

Où chaque style est son pourcentage dans `genres_pct` (0–100), la somme = 100.
Score résultant : 0–100. Un festival 100% DnB = compat 100. Un festival 100% melodic = compat 20.

### Tableau des poids

| Style | Poids | Rationale |
|---|---:|---|
| dnb | 10 | Goût n°1 |
| neuro | 10 | Goût n°1 (sub-genre DnB sombre) |
| dub | 10 | Goût n°1 (sound system) |
| jungle | 8 | DnB roots, très apprécié |
| liquid | 8 | DnB mélodique, fort intérêt |
| psy | 8 | Goût fort |
| darkpsy | 8 | Goût fort |
| forest | 8 | Goût fort |
| acid | 8 | Goût fort |
| techno | 8 | Goût fort |
| bass | 7 | Bass music large, légèrement en dessous du dub pur |
| fullon | 6 | Goût moyen |
| house | 4 | Goût faible |
| melodic | 2 | Très faible intérêt |
| other | 1 | Par défaut, quasi neutre |

### Exemples de référence

| Festival | Profil simplifié | Compat estimé |
|---|---|---:|
| Let It Roll | 75% dnb/neuro/jungle | ~89 |
| Dub Camp | 70% dub/bass | ~92 |
| Ozora | 60% psy/darkpsy/forest | ~78 |
| Nature One | 40% acid/techno + 30% psy | ~77 |
| Dimensions | 50% techno + 30% bass | ~75 |

---

## Score nature

| Critère | Points |
|---|---:|
| Baignade sur site (autorisée, accessible à pied) | +25 |
| Baignade proche accessible à pied (< 15 min) | +18 |
| Baignade proche en voiture (< 30 min) | +10 |
| Cadre forêt / rivière / lac / mer | +25 |
| Cadre montagne / vallée / campagne | +15 |
| Cadre urbain | +5 |
| Cadre industriel | +0 |
| Camping immersif dans la nature | +15 |
| Peu commercial / ambiance underground | +15 |
| **Maximum réel** | **80** *(baignade site + forêt/mer + camping immersif + underground — les groupes baignade et cadre sont mutuellement exclusifs)* |

---

## Score budget

Basé sur le budget total estimé pour 2 personnes depuis Genève (billets + essence + péages + camping + bouffe + imprévus).

| Budget 2p | Score |
|---:|---:|
| < 600 € | 100 |
| 600–800 € | 92 |
| 800–1 000 € | 82 |
| 1 000–1 300 € | 72 |
| 1 300–1 600 € | 60 |
| 1 600–2 000 € | 45 |
| 2 000–2 500 € | 28 |
| > 2 500 € | 15 |

**Malus budget :**

| Situation | Malus |
|---|---:|
| Ferry obligatoire (Sardaigne, UK si bateau) | −10 |
| Eurotunnel / Manche obligatoire | −8 |
| Logement obligatoire (pas de camping) | −15 |
| Billet > 250 €/pers | −10 |
| Ville chère sans camping (London, Amsterdam, Paris) | −10 |
| Vignette(s) requise(s) | −3 |

---

## Score confort

| Critère | Points |
|---|---:|
| Sanitaires / douches fiables | +20 |
| Eau potable / refill gratuit | +15 |
| Camping inclus dans le billet | +15 |
| Camping premium disponible en option | +10 |
| Parking clair / sécurisé | +10 |
| Infos pratiques claires (cashless, app, plan) | +10 |
| Navette / gare proche | +10 |
| Hébergement alternatif facile à trouver | +10 |
| **Total max** | **100** |

---

## Score global

```
global_score =
  compat_score × 0.40 +
  nature_score × 0.25 +
  budget_score × 0.20 +
  comfort_score × 0.15
```

### Seuils de décision

| Global | Décision |
|---:|---|
| ≥ 88 | Top priorité — réserver dès que possible |
| 80–87 | Très bon candidat |
| 70–79 | Intéressant selon contrainte du moment |
| 60–69 | Secondaire |
| < 60 | À éviter sauf raison forte (baignade exceptionnelle, line-up unique) |

---

## Malus manuels (post-calcul)

| Situation | Malus global |
|---|---:|
| Déjà fait l'année précédente, envie de nouveauté | −3 à −8 |
| Conflits de date avec un autre festival prioritaire | −5 |
| Dates non confirmées officiellement | −8 |
| Aucune source officielle trouvée | −15 |
| Line-up très incomplet ou non annoncé | −5 |
| Ambiance commerciale élevée signalée par la communauté | −10 |
| Annulé en 2025 sans confirmation 2026 (ex. Shankra) | −12 |

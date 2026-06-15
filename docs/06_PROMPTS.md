# 🧠 Prompts de recherche — Festivals 2026

> Prompts à copier-coller dans GPT-4o, Perplexity, Gemini, ou toute autre IA.
> Règles anti-hallucination intégrées dans chaque prompt.

---

## Règles anti-hallucination (communes à tous les prompts)

Ces règles doivent toujours s'appliquer — les inclure ou les rappeler dans chaque session :

1. 🟢 = dates confirmées sur site officiel ou Resident Advisor. 🟡 = estimation basée sur éditions précédentes.
2. **Ne jamais inventer une URL.** Si non trouvée → écrire `"non trouvée"`.
3. **Ne jamais inventer des % de styles** sans base dans le line-up réel.
4. Si un festival n'a pas d'édition 2026 confirmée, le dire clairement plutôt que d'inventer.
5. Si la localisation GPS est incertaine → noter `*(approx)*`.
6. **La somme des 15 styles doit faire exactement 100**, pas 99, pas 101.
7. Les champs `[AUTO]` (distance, route, `estimated_budget_2p`, `budget_score`, `compat_score`, `nature_score`, `global_score`) ne sont jamais à remplir. `comfort_score` se calcule manuellement via la grille `05_SCORING.md` et se saisit.

---

## Prompt 1 — Trouver de nouveaux festivals (usage principal)

> **Sortie à coller directement à la fin de `data/festivals-2026.md`** sous la section `🔜 En attente d'intégration`.

```
Tu es expert en scènes électroniques alternatives européennes.

Objectif : trouver de NOUVEAUX festivals/events 2026 à ajouter à ma base, sans doublons.

Déjà dans ma base — NE PAS redoubler :
Spirit Base, Darkshire In The Woods, Fusion, Burning Mountain, Dub Station, Hospitality On The Beach,
Beats for Love, Rampage Open Air, Shankra, Astropolis, Reisefieber XX, Masters of Puppets,
Noisily, Subset Festival, Dub Camp, S.U.N., 7 Chakras, Dour, Sea Sound, VooV Experience,
Paléo, Outlook Origins, Liquicity Festival, Ozora, Let It Roll, Garbicz, Nature One, PsyRiver,
Mo:Dem, Pietra Sonica, Drops, Boomtown Fair, Free Earth, Indian Spirit, Outback, Hadra,
Dimensions, Rampage Roadshow Toulouse, NIBIRII, Liquicity Paris, Own Spirit, Modular Festival,
PSYLÂN, Sun and Bass, Liquicity Hamburg, Alive Dub, Liquicity Vienna, Walking Bass, Breakin Science Amsterdam.

Périmètre : juin → novembre 2026, départ voiture depuis Genève.
Rayon prioritaire ≤ 1000 km mais accepte jusqu'à 1500 km si l'event est vraiment exceptionnel.

Genres prioritaires (ordre décroissant) :
- DnB / Neurofunk / Jungle / Liquid DnB
- Dub / Sound-System / Bass music
- Psytrance / Darkpsy / Forest psy
- Acid / Techno underground
Exclusions strictes : EDM mainstream, Hardstyle, techno mélodique dominante.

Zones à ratisser en priorité :
- Suisse entière
- France : Rhône-Alpes, Auvergne, Sud-Est, Occitanie, PACA
- Italie du Nord : Piémont, Lombardie, Vénétie, Ligurie
- Allemagne Sud : Bade-Wurtemberg, Bavière
- Autriche : Innsbruck, Salzbourg, Vienne si line-up DnB/psy sérieux
- Slovénie / Croatie : seulement si nature + baignade + gros son
- Pays-Bas / Belgique : seulement si weekender DnB ou psy majeur
- UK / Wales : festivals DnB ou psy de référence (logistique lourde mais acceptée)

Sources obligatoires : site officiel, billetterie officielle, page organisateur, RA.
Ne jamais inventer une date. Si non confirmé → 🟡 estimation.
Ne jamais inventer une URL. Si non trouvée → écrire "non trouvée".
La somme des 15 styles doit faire exactement 100.

Pour chaque festival vérifié, produis un bloc dans ce format exact (prêt à coller dans le fichier de données) :

## 🇺🇳 NOM DU FESTIVAL
- **Lieu** : Ville, Région · GPS lat, lng *(approx)* ou lat, lng
- **Dates** : DD–DD mois (Mois, X j) · saison été / arriere_saison / automne · outdoor / indoor / hybrid
- **Styles** : Style XX% · Style XX% · Autres XX% *(somme = 100 — utilise les 15 clés : dnb, neuro, jungle, liquid, dub, bass, psy, darkpsy, forest, fullon, acid, techno, house, melodic, other)*
- **Compatibilité** : [AUTO]
- **Trajet** : [AUTO]
- **Baignade** : oui (type/site ou proche/✅ ou à confirmer/accès) / non (aucune) / inconnu · chaleur : oui/non
- **Commercial** : faible / moyen / élevé · taille : petit / moyen / grand / très grand · risque billetterie : faible / moyen / élevé
- **Cadre** : description courte (cadre naturel, ambiance, particularités)
- **Météo** : [AUTO]
- **Couchage** : Camping inclus / sur site / options ; hôtels si hors site
- **Prix billet** : ~XX€/pers early / ~XX€ standard (ou "non trouvé") · **Budget total 2p** : [AUTO]
- ✅ points forts (max 3, séparés par ·)
- ❌ points faibles (max 3, séparés par ·)
- ⚠️ alerte si applicable (sold-out, annulation historique, condition spéciale) — supprimer la ligne sinon
- 🎧 Artiste 1 · Artiste 2 · Artiste 3 (ou "n/c" si aucun annoncé)
- 🔗 https://url-officielle (ou "non trouvée")

Sépare chaque bloc par ---

À la fin, ajoute :
### 🟡 Leads à vérifier
Une ligne par lead (nom, pays, dates estimées, raison de l'intérêt).

### 🔴 Écartés
Une ligne par festival écarté et la raison (hors dates, trop commercial, pas d'édition 2026...).
```

---

## Prompt 2 — Vérifier une liste ciblée

```
Tu es expert en scènes électroniques alternatives européennes.

Vérifie chacun des festivals ci-dessous pour l'édition 2026.

Festivals à vérifier :
[COLLER LA LISTE — ex. : Hospitality in the Park, Detonate, Balter, Liquicity, DNA Music, Sonica, Kappa FuturFestival, Terraforma, Atman, Audioriver]

Pour chaque festival, vérifie :
- Dates 2026 sur site officiel ou Resident Advisor
- Lieu exact (ville + GPS si possible)
- Line-up ou artistes annoncés
- Cadre (nature, forêt, urbain, plage…)
- Baignade possible ?
- Taille approximative
- Prix billets + camping si applicable

Règles :
- 🟢 = dates confirmées. 🟡 = estimation éditions précédentes.
- Ne jamais inventer une URL.
- Si le festival n'existe pas en 2026 → le dire clairement.
- Somme des 15 styles = exactement 100.

Même format de sortie que Prompt 1.
```

---

## Prompt 3 — Recherche indoor / automne (DnB / Neuro / Dub)

```
Cherche uniquement des events indoor / club / weekender entre septembre et novembre 2026
avec line-up DnB, neurofunk, jungle, dubstep, dub ou sound-system sérieux.

Promoteurs et labels à surveiller en priorité :
Liquicity, Rampage, Hospitality, Blackout Music, Eatbrain, Critical Music, Vision,
1985 Music, Overview, Flexout, Sofa Sound, Shogun Audio, Dispatch, Dubquake, O.B.F, Iration Steppas.

Zones : Suisse, France, Italie du Nord, Allemagne Sud, Autriche.
Pays-Bas / Belgique seulement si weekender vraiment gros.

Exclus : soirées locales sans line-up solide, EDM, techno mélo, house dominante.

Classe par pertinence pour un couple en voiture depuis Genève.

Pour chaque event : date, lieu, GPS, source officielle, line-up, prix, format, durée, intérêt réel.
```

---

## Prompt 4 — Vérifier et corriger une fiche existante

```
Vérifie cette fiche festival et corrige uniquement les données incertaines.

Règles :
- Ne change pas le style de la fiche.
- Ne rajoute rien sans source vérifiable.
- Si la date 2026 n'est pas officielle → marquer 🟡 estimation.
- Si une info est absente → écrire `n/c`.
- Si la baignade est incertaine → écrire `inconnu` et expliquer pourquoi.
- Priorité absolue aux sources officielles.
- Somme des 15 styles doit rester = 100.

Fiche à vérifier :
[COLLER LA FICHE]

Retour attendu :
1. Liste des corrections effectuées.
2. Fiche Markdown corrigée complète.
3. Données encore manquantes ou à vérifier.
```

---

## Prompt 5 — Identifier les trous géographiques

```
Voici ma base actuelle de festivals 2026 (départ Genève) :
[COLLER LISTE]

Cherche les trous géographiques évidents pour une carte festivals électroniques alternatifs 2026.

Je veux uniquement :
- Régions sous-représentées et pourquoi.
- Promoteurs / labels locaux à surveiller par pays.
- Festivals qui peuvent annoncer tardivement (open-airs associatifs, free parties légales).
- Mots-clés de recherche par pays et par langue.

Ne donne pas de festival si tu n'as pas de source officielle 2026.
Si pas encore annoncé, donne une piste de veille, pas une fiche.
Genres cibles : DnB / Neuro / Dub / Sound-system / Psytrance / Darkpsy / Forest / Acid / Tekno.
```

---

## Prompt 6 — Compléter les données carte d'un festival incomplet

```
Complète les données manquantes pour rendre ce festival compatible avec la carte interactive.

Champs obligatoires à remplir si absents :
- id (slug kebab-case stable)
- lat / lng
- start_date / end_date (ISO 8601)
- date_status (confirmed / estimated / unknown)
- source_url + source_type
- event_type (outdoor / indoor / city_event / hybrid)
- season (ete / arriere_saison / automne)
- duration_days
- genres_pct : 15 styles, somme = 100
- swim_type / swim_location / swim_allowed / swim_access
- commercial_level
- pros / cons

Champs [AUTO] à NE PAS remplir :
- drive_km_return / drive_hours_oneway / road_steps
- estimated_budget_2p / budget_score
- compat_score / nature_score / global_score

Ne devine pas les données critiques (dates, GPS) — mettre null si non trouvé.

Festival :
[COLLER NOM + FICHE]
```

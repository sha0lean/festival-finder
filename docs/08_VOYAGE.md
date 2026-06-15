# 🗺️ Couche voyage — planning personnel & POI en route

> Complément au kit festival.
> Départ : Genève / Haute-Savoie · Fenêtre : juin → novembre 2026
>
> **Statut :** §1–§2 prêts pour le one-shot · §3–§5 stubs pour la recherche POI future (app séparée)

---

## 1. Disponibilités — créneaux à placer

**Contrainte globale** : 3 semaines à placer entre juin et novembre 2026.

### Créneaux prioritaires (semaine = lun → dim)

| Priorité | Semaine | Dates | Statut |
|---|---|---|---|
| 🔴 MUST | Semaine centrale | 20–26 juil | Fixer en premier |
| 🟠 FORT | Semaine adjacente | 13–19 juil OU 27 juil–2 août | Une des deux avec la centrale |
| 🟡 ALT | Semaine alternative | 13–19 juil OU 27 juil–3 août | Si la centrale tombe seule |
| 🟢 OPEN | Troisième semaine | Autre créneau juin–nov | Ouvert — à déterminer selon festivals retenus |

### Scénarios de placement

```
Option A (3 semaines bloc)  : 13–19 juil + 20–26 juil + 27 juil–2 août
Option B (2+1 split)        : [13–19 juil + 20–26 juil] + [une semaine sep/oct]
Option C (2+1 split)        : [20–26 juil + 27 juil–2 août] + [une semaine sep/oct]
Option D (1+1+1)            : 20–26 juil + une semaine juil/août + une semaine automne
```

---

## 2. Vue calendrier — couche disponibilités

Superposer sur le calendrier festival existant (cases-jours colorées par style) :

| Élément | Rendu suggéré |
|---|---|
| Créneau MUST (sem. 20 juil) | fond vert lime semi-transparent `#c6ff3a33` |
| Créneau FORT (sem. adjacente) | fond ambre `#ffb13a22` |
| Créneau OPEN (3e sem.) | fond gris doux `#9aa39a18` |
| Hors disponibilité | aucun fond — cases grises normales |

**Règle UX** : c'est un overlay read-only sur le calendrier existant. Pas de saisie interactive pour l'instant. On veut juste voir visuellement quels festivals tombent dans les fenêtres vertes.

---

---

> **⬇️ Sections suivantes — RECHERCHE FUTURE (app séparée)**
> À compléter quand les données POI seront rassemblées. Ne pas intégrer dans le festival finder V1.

---

## 3. Vue carte — couche POI "en route" (légère)

### Philosophie

- **PAS** un annuaire d'activités exhaustif
- **OUI** une couche indicative : "y a-t-il quelque chose d'intéressant à proximité du trajet ?"
- Exemples de POI utiles : gorges, lacs de baignade, cols, sommets accessibles en journée, sites naturels notables
- Usage : quand je fais 2h de route vers un festival, je veux savoir si je peux faire un détour utile

### Données POI — format à définir

```yaml
poi:
  - id: lac-serre-poncon
    label: Lac de Serre-Ponçon
    type: baignade          # baignade | rando | nature | vue | autre
    gps: [44.55, 6.33]
    note: "baignade + camping possible, sur la route Genève→sud"
    relevance: [trajet-provence, trajet-alpes-sud]  # pour quels trajets c'est utile

  - id: vercors-hauts-plateaux
    label: Hauts Plateaux du Vercors
    type: rando
    gps: [44.85, 5.45]
    note: "randonnée sauvage, aucun réseau, 1h de détour depuis A49"
    relevance: [trajet-sud-france]
```

### Affichage sur la carte

- Marqueurs POI distincts des marqueurs festival (forme différente, couleur neutre)
- Masqués par défaut → activable via toggle `🏕️ POI en route`
- Tooltip simple : type + note courte
- **Pas de filtre complexe** — juste on/off

### Périmètre data (pour le one-shot)

Pour commencer : uniquement les POI **sur les couloirs de route fréquents** depuis Genève :
- Axe Genève → Italie du Nord (A5/Mont-Blanc)
- Axe Genève → sud France (A40/A7/A8)
- Axe Genève → Allemagne/Autriche (A1)
- Axe Genève → Espagne (A40/A9)
- Axe Genève → Balkans (A1 → Slovénie/Croatie)

Pas besoin d'exhaustivité. 5–10 POI par axe suffisent pour valider le concept.

---

## 4. Liens avec le build existant

| Feature | Fichier BUILD | Section |
|---|---|---|
| Calendrier cases-jours | `festival-finder-BUILD.md` | §8 Calendrier |
| Marqueurs carte | `festival-finder-BUILD.md` | §8 Carte |
| Backlog itinéraire | `festival-finder-BUILD.md` | §12 Backlog |

À ajouter dans `festival-finder-BUILD.md §12` :
- Overlay disponibilités personnelles sur le calendrier
- Couche POI en route (toggle, marqueurs distincts, YAML embarqué)

---

## 5. Données POI à remplir (stub)

> À compléter avant le one-shot. Format YAML embarqué dans le JS.

```js
const POI = [
  // Axe Genève → Italie du Nord
  // { id, label, type, gps:[lat,lng], note, axes:[] }

  // Axe Genève → Sud France

  // Axe Genève → Allemagne / Autriche

  // Axe Genève → Espagne

  // Axe Genève → Balkans
];
```

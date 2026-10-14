<div align="center">

# 🏔️ Grand Raid 2026 — Monitor Live

**Moniteur 3D en temps réel des 5 courses du Grand Raid de La Réunion**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/fr/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/fr/docs/Web/JavaScript)
[![CesiumJS](https://img.shields.io/badge/CesiumJS-3D-48b2e8?logo=cesium&logoColor=white)](https://cesium.com/)
[![LiveTrail](https://img.shields.io/badge/API-LiveTrail-0055A4)](https://grandraid-reunion.v3.livetrail.net/)
[![Made in Réunion](https://img.shields.io/badge/🇷🇪-La_Réunion-EF4135)](https://www.regionreunion.com/)

**Suivi GPS · 5 parcours officiels · Classements en direct · Mode clair/sombre · Boussole interactive**

[🚀 Démo en ligne](https://gunout.github.io/monitor-grr/) · [📖 Documentation](#-documentation) · [🐛 Signaler un bug](https://github.com/gunout/monitor-grr/issues)

</div>

## 📖 Sommaire

- [✨ Aperçu](#-aperçu)
- [🎯 Fonctionnalités](#-fonctionnalités)
- [🛰️ Suivi en direct](#️-suivi-en-direct)
- [🚀 Installation](#-installation)
- [📡 API utilisées](#-api-utilisées)
- [🗺️ Les 5 courses](#️-les-5-courses)
- [⌨️ Raccourcis clavier](#️-raccourcis-clavier)
- [🎨 Design](#-design)
- [🛠️ Stack technique](#️-stack-technique)
- [📄 Licence](#-licence)

## ✨ Aperçu

Ce moniteur permet de **suivre en temps réel** les coureurs du **Grand Raid de La Réunion 2026** (34ᵉ édition) sur une **carte 3D interactive** propulsée par **CesiumJS**.

Il affiche les **5 parcours officiels** avec leurs tracés GPS précis (issus de l'API LiveTrail), les **classements live** des 20 premiers hommes et femmes, et la **progression des coureurs** sur la carte.

<div align="center">

| 🇫🇷 Design tricolore | 🧭 Boussole 360° | 🟢 Suivi GPS live |
|:---:|:---:|:---:|
| Bleu · Blanc · Rouge | Rotation fluide | Temps réel |

</div>

## 🎯 Fonctionnalités

### 🗺️ Carte 3D interactive

- **5 tracés officiels** chargés depuis l'API LiveTrail (S3)
- **Imagerie satellite Esri** haute résolution
- **Relief 3D** de La Réunion (Piton des Neiges, cirques, remparts)
- **Zoom, rotation, inclinaison** à 360°
- **Boussole interactive** avec clic-glisser

### 🏆 Classements en direct

- **Top 20 hommes** et **Top 20 femmes**
- Tri par index de performance
- Drapeaux des pays, dossards, catégories
- Rafraîchissement automatique (60 s)

### 🟢 Suivi GPS des coureurs

- **20 marqueurs bleus** (hommes) et **20 marqueurs rouges** (femmes)
- Position sur le **dernier checkpoint** passé
- **Popups cliquables** : nom, dossard, km, classement
- Rafraîchissement toutes les **30 secondes**

### 🎨 Interface soignée

- **Mode clair** (blanc dominant) et **mode sombre**
- **Panneaux escamotables** (sidebar, intelligence, classements)
- **Plein écran** pour la carte (raccourci `F`)
- **Responsive** mobile / tablette / desktop

## 🛰️ Suivi en direct

Le suivi GPS s'appuie sur l'API LiveTrail :

    GET https://api.v3.livetrail.net/api/events/runners/{bib}/detail
    Header: X-Tenant: grandraid-reunion_2026

**Champ `passings`** : contient la liste des checkpoints passés par le coureur (km, heure, classement, vitesse).

**Fonctionnement** :

1. Le moniteur charge les 20 premiers H/F via `/api/events/runners?raceId=GRR&sex=MALE&limit=20`
2. Pour chaque coureur, il interroge `/api/events/runners/{bib}/detail`
3. Il extrait le **dernier passage** (`passings[0]`) et le mappe sur le **checkpoint officiel** (lat/lon)
4. Un **marqueur mobile** est placé sur la carte
5. Le processus se répète toutes les **30 secondes**

> ⚠️ **Note** : tant que la course n'est pas lancée (`RACE_NOT_LAUNCHED`), le champ `passings` est vide et les marqueurs ne s'affichent pas.

## 🚀 Installation

### Option 1 — Utilisation directe

1. Clone le dépôt :

       git clone https://github.com/gunout/monitor-grr.git
       cd monitor-grr

2. Ouvre `index.html` dans un navigateur moderne (Chrome, Firefox, Safari, Edge).

3. C'est tout ! Le moniteur charge automatiquement les tracés et les données live.

### Option 2 — Serveur local (recommandé)

Pour éviter les restrictions CORS :

    # Python 3
    python3 -m http.server 8000

    # ou Node.js
    npx serve

Puis ouvre [http://localhost:8000](http://localhost:8000).

### Option 3 — GitHub Pages

Le moniteur est déployable sur **GitHub Pages** en activant Pages dans les paramètres du dépôt (branche `main`, dossier racine).

## 📡 API utilisées

| Endpoint | Description | Tenant |
|----------|-------------|--------|
| `/api/events/races/lives` | Statut live des 5 courses | `grandraid-reunion_2026` |
| `/api/events/races/{raceId}` | Infos course + URL du tracé | `grandraid-reunion_2026` |
| `/api/events/races/{raceId}/live` | Live détaillé d'une course | `grandraid-reunion_2026` |
| `/api/events/runners?raceId={id}&sex={sex}` | Liste des coureurs (pagination) | `grandraid-reunion_2026` |
| `/api/events/runners/{bib}/detail` | Détail d'un coureur (passings, palmarès) | `grandraid-reunion_2026` |
| S3 `tracks/{uuid}.json` | Tracé GPS officiel d'une course | — |

**Documentation** : [LiveTrail API](https://grandraid-reunion.v3.livetrail.net/)

## 🗺️ Les 5 courses

| Code | Nom | Distance | D+ | Départ | Heure |
|:----:|-----|:--------:|:--:|--------|:-----:|
| **GRR** | 🏔️ Diagonale des Fous | 180 km | 10 200 m | Saint-Pierre | 15/10 · 22h00 |
| **ZEM** | 🔁 Zembrocal Trail | 160 km | 8 460 m | Saint-Joseph | 15/10 · 17h00 |
| **TDB** | 💧 Trail de Bourbon | 106 km | 6 200 m | Cilaos | 16/10 · 20h00 |
| **MAS** | 🌿 Mascareignes | 74 km | 3 800 m | Hell-Bourg | 16/10 · 01h00 |
| **MTR** | 🌅 Métis Trail | 51 km | 2 500 m | Saint-Paul | 17/10 · 06h00 |

**Arrivée commune** : Saint-Denis · Stade de La Redoute

## ⌨️ Raccourcis clavier

| Touche | Action |
|:------:|--------|
| `F` | Carte plein écran |
| `L` | Masquer/afficher la sidebar gauche |
| `R` | Masquer/afficher le panneau Intelligence |
| `←` `→` | Rotation caméra (5° par pas) |
| `Esc` | Reset tous les panneaux |

## 🎨 Design

### Palette de couleurs

| Couleur | Usage | Hex |
|---------|-------|-----|
| 🔵 Bleu France | Primaire | `#0055A4` |
| ⚪ Blanc | Fond mode clair | `#FFFFFF` |
| 🔴 Rouge France | Accent, femmes | `#EF4135` |
| 🟢 Vert | Suivi actif | `#00A854` |
| 🟡 Jaune | Alertes | `#E88B00` |

### Modes

- **Mode clair** (défaut) : fond blanc, texte bleu nuit
- **Mode sombre** : fond bleu nuit, texte blanc
- **Persistance** : le choix est mémorisé dans `localStorage`

## 🛠️ Stack technique

| Technologie | Usage |
|-------------|-------|
| [CesiumJS 1.145](https://cesium.com/) | Carte 3D |
| [Inter](https://fonts.google.com/specimen/Inter) | Typographie |
| [JetBrains Mono](https://www.jetbrains.com/lp/mono/) | Chiffres, monospace |
| [LiveTrail API](https://grandraid-reunion.v3.livetrail.net/) | Données live |
| [Esri World Imagery](https://www.arcgis.com/) | Imagerie satellite |
| Vanilla JS | Aucun framework |

**Aucune dépendance npm**, tout est chargé via CDN.

## 📄 Licence

Ce projet est sous licence **MIT**. Voir [LICENSE](LICENSE) pour plus de détails.

**Avertissement** : Ce moniteur est un **outil non officiel**. Les données sont fournies par l'API publique LiveTrail. Le Grand Raid de La Réunion est une marque déposée de ses organisateurs.

<div align="center">

**Fait avec ❤️ à La Réunion 🇷🇪**

*Liberté · Égalité · Fraternité*

[![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?logo=github)](https://github.com/gunout)

</div>

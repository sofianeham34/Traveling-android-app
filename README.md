# Traveling — TravelShare & TravelPath

![Android](https://img.shields.io/badge/Android-Java-3DDC84?logo=android&logoColor=white)
![MVVM](https://img.shields.io/badge/Architecture-MVVM-blue)
![Firebase](https://img.shields.io/badge/Firebase-Auth%20%2B%20Firestore-FFCA28?logo=firebase&logoColor=black)
![Room](https://img.shields.io/badge/Room-v31-orange)

Application Android complète autour du voyage, composée de deux modules complémentaires : **TravelShare**, un réseau social de photos de voyage, et **TravelPath**, un générateur d'itinéraires personnalisés.

> UE Programmation Mobile — Université de Montpellier 2025-2026  
> Sofiane Hammar · Anouar Soufyani

---

## Modules

### TravelShare — Réseau social de voyages
- Inscription / connexion Firebase Auth (email ou anonyme)
- Publication multi-images avec **note vocale AAC** et **tagging IA** (ML Kit)
- Feed avec 8 filtres (Nature, Urbain, Culture, Magasin, Localisation, Likes…)
- Recherche texte + vocale en temps réel
- Profil public style Instagram — follow/unfollow, abonnés cliquables
- Groupes publics et privés + **chat temps réel** (Firestore, 4 types de messages)
- Conversations directes entre utilisateurs
- Carte interactive **OSMDroid** avec pins photos géolocalisés
- Notifications in-app synchronisées Firestore

### TravelPath — Générateur d'itinéraires
- Formulaire : ville, 5 activités, budget, durée, niveau d'effort
- **3 types de parcours** générés : économique, équilibré, confort
- Vrais lieux via **Overpass API** (OpenStreetMap), fallback Nominatim
- Tracé routier calculé par **OSRM** avec durées réelles
- Météo temps réel **Open-Meteo** par étape
- Export **PDF natif** (Canvas Android, format A4)
- Partage de parcours dans les groupes de discussion

### Passerelle bidirectionnelle
- TravelShare → TravelPath : destination transmise automatiquement depuis une photo
- TravelPath → TravelShare : "Voir des photos" par étape de parcours

---

## Architecture

```
MVVM — Pattern recommandé par Google

Vue (Fragments / Activities)
    │ observe LiveData
    ▼
ViewModel (SharedViewModel · TravelPathViewModel)
    │ Repository pattern
    ├──► FirebaseRepository ──► Firebase Auth · Firestore · Cloudinary
    └──► AppDatabase (Room v31)
              12 entités locales
              Pool de 4 threads
```

| Donnée | Local (Room) | Cloud (Firestore) |
|---|---|---|
| Photos | Cache vidé au démarrage | Source principale (snapshot listener) |
| Plans TravelPath | Source principale | Copie si liké |
| Groupes & messages | Cache local | Source de référence + temps réel |
| Notifications | Stockage principal | Propagation inter-appareils |

---

## Stack technique

| Couche | Technologies |
|---|---|
| **Langage** | Java |
| **Architecture** | MVVM + LiveData + ViewModel |
| **Base de données locale** | Room v31 — 12 entités |
| **Authentification** | Firebase Auth (email + anonyme) |
| **Cloud / temps réel** | Firebase Firestore |
| **Stockage images** | Cloudinary |
| **Cartographie** | OSMDroid |
| **Routage** | OSRM |
| **Lieux** | Overpass API + Nominatim |
| **Météo** | Open-Meteo |
| **IA (tags images)** | ML Kit Image Labeling |
| **Build** | Gradle Kotlin DSL |

---

## Lancer le projet

1. Cloner le repo et ouvrir dans **Android Studio**
2. Attendre la synchronisation Gradle
3. Le fichier `app/google-services.json` est inclus — Firebase est prêt
4. Lancer sur un émulateur ou appareil physique

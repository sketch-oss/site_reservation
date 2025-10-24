Voici un **README complet et clair** pour ton projet “Espace Bien-Être”, à la fois technique et accessible pour ceux qui voudraient le déployer, l’administrer ou le maintenir 👇

---

# 🧘‍♀️ Espace Bien-Être — Système de Réservation

## 🌍 Présentation

Ce projet est une **application web complète** permettant la gestion des réservations de l’**Espace Bien-Être du CPN (Centre Psychothérapique de Nancy)**.
Elle comprend :

* une **interface publique** (réservations des usagers),
* une **interface d’administration** (configuration et supervision),
* un **serveur Node.js / Express** pour le stockage, les emails et la logique métier.

---

## ⚙️ Fonctionnalités principales

### 👥 Côté utilisateur (index.html)

* Réservation d’un créneau pour un service (Massage AMMA, Réalité Virtuelle, Luminothérapie, etc.)
* Création automatique d’un fichier **.ics** (invitation calendrier)
* Envoi d’emails de **confirmation** et d’**annulation**
* Blocage automatique des créneaux complets ou fermés

### 🛠️ Côté administration (config.html)

* Gestion des **services** (libellé, durée, capacité)
* Configuration des **jours ouverts / fermés**
* Gestion des **plages horaires par jour et par service**
* Définition des **périodes de fermeture** (vacances, congés)
* Blocage ponctuel d’un créneau précis (date + heure + service)
* Paramétrage des **emails automatiques**
* Enregistrement persistant dans un fichier `config.json`
* Interface visuelle moderne (mode clair/sombre, thèmes de couleur)

### 📧 Notifications & calendrier

* Envoi d’emails via **Exchange SMTP**
* Génération et envoi d’invitations **ICS (iCalendar)** compatibles Outlook / Gmail
* Synchronisation entre réservant et responsable via le calendrier

---

## 🧩 Architecture technique

```
project-root/
├── public/                   # Contient le front-end (HTML/CSS/JS)
│   ├── index.html             # Vue utilisateur (réservations)
│   ├── config.html            # Vue administrateur
│   └── assets/
│       └── app.css            # Feuille de style principale
│
├── data/                     # Données persistantes
│   ├── config.json            # Configuration complète
│   └── reservations.json      # Réservations effectives
│
├── server.js                 # Serveur Node.js / Express
└── package.json
```

---

## 🚀 Installation & exécution

### 1️⃣ Cloner le dépôt

```bash
git clone https://github.com/ton-compte/espace-bien-etre.git
cd espace-bien-etre
```

### 2️⃣ Installer les dépendances

```bash
npm install
```

### 3️⃣ Créer les dossiers de données

```bash
mkdir data
```

### 4️⃣ Lancer le serveur

```bash
node server.js
```

Le serveur démarre par défaut sur :
👉 [http://localhost:8080](http://localhost:8080)

---

## 🔐 Variables d’environnement

Tu peux créer un fichier `.env` (ou exporter les variables directement dans ton shell) :

| Variable            | Description                                 | Valeur par défaut                    |
| ------------------- | ------------------------------------------- | ------------------------------------ |
| `PORT`              | Port HTTP d’écoute                          | `8080`                               |
| `ADMIN_PIN`         | Code PIN d’accès à l’administration         | `1234`                               |
| `ADMIN_SECRET`      | Jeton API pour sauvegarder la configuration | `INFO0035`                           |
| `RESA_DATA_DIR`     | Dossier où sont stockées les données JSON   | `./data`                             |
| `MAIL_FROM`         | Adresse d’expédition des mails              | `reservation.bienetre@cpn-laxou.com` |
| `RESPONSIBLE_EMAIL` | Adresse du responsable bien-être            | `mathieu.dreyer@cpn-laxou.com`       |
| `EXCHANGE_HOST`     | Hôte SMTP Exchange                          | `mail.cpn-laxou.com`                 |
| `EXCHANGE_PORT`     | Port SMTP Exchange                          | `25`                                 |
| `TZID`              | Fuseau horaire pour les fichiers .ics       | `Europe/Paris`                       |

---

## 🧾 API – routes principales

| Méthode  | Route                  | Description                                                |
| -------- | ---------------------- | ---------------------------------------------------------- |
| `GET`    | `/api/config`          | Récupère la configuration                                  |
| `PUT`    | `/api/config`          | Enregistre la configuration (avec header `X-Admin-Secret`) |
| `GET`    | `/api/week?from&to`    | Renvoie les réservations d’une période                     |
| `POST`   | `/api/slot/:id/add`    | Réserve un créneau                                         |
| `POST`   | `/api/slot/:id/remove` | Annule la réservation d’un utilisateur                     |
| `DELETE` | `/api/slot/:id`        | Supprime un créneau (admin)                                |
| `DELETE` | `/api/reset`           | Réinitialise toutes les réservations (admin)               |
| `POST`   | `/admin/login`         | Connexion administrateur via code PIN                      |
| `GET`    | `/admin/me`            | Vérifie le statut de session administrateur                |

---

## 📁 Structure du fichier `config.json`

```json
{
  "services": [
    { "key": "amma", "label": "Massage AMMA Assis", "capacity": 1, "grey": false },
    { "key": "rv", "label": "Réalité Virtuelle", "capacity": 1, "grey": false },
    { "key": "lumo", "label": "Luminothérapie", "capacity": 2, "grey": false },
    { "key": "doin", "label": "Do In", "capacity": 0, "grey": true }
  ],
  "openDays": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
  "closed": [["2025-12-25","2026-01-03"]],
  "days": {
    "Tuesday": {
      "amma": ["11:30", "12:00", "12:30–13:00"],
      "rv": ["12:00–12:30"],
      "lumo": ["11:30–12:00"]
    }
  },
  "blocks": [
    { "date": "2025-06-21", "service": "rv", "slot": "13:00" }
  ],
  "mail": {
    "from": "resa@cpn-laxou.com",
    "responsible": "Mathieu.Dreyer@cpn-laxou.com",
    "subject_user": "Réservation confirmée — {{service}} — {{date}} {{slot}}",
    "body_user": "Bonjour,\nVotre réservation est confirmée...",
    "cancel_subject_user": "Annulation — {{service}} — {{date}} {{slot}}",
    "cancel_body_user": "Votre réservation a été annulée..."
  }
}
```

---

## 💌 Exemple d’emails générés

### Confirmation (client)

```
Objet : Réservation confirmée — Massage AMMA — Mardi 25/02 — 11:30

Bonjour,

Votre réservation a bien été enregistrée :
• Service : Massage AMMA
• Date : Mardi 25/02
• Créneau : 11:30

Cordialement,
Espace Bien-Être
```

### Notification (service)

```
Nouvelle réservation enregistrée :
• Service : Massage AMMA
• Date : Mardi 25/02
• Créneau : 11:30
• Réservant : jean.dupont@cpn-laxou.com
```

---

## 🧠 Notes techniques

* Les jours “fermés” sont définis dans `openDays` : ils sont **masqués visuellement** et **non affichés** dans les plannings.
* L’application supporte les **modes clair/sombre** et plusieurs **palettes (teal, sage, sand, slate)**.
* Les fichiers `.json` dans `/data` peuvent être sauvegardés manuellement pour des **backups rapides**.
* Le code côté client ne nécessite **aucune dépendance externe** (pur HTML/CSS/JS).

---


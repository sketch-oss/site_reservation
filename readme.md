# 🧘‍♀️ Espace Bien-Être — Système de Réservation

## 🌍 Présentation

**Espace Bien-Être** est une application web interne développée pour le **Centre Psychothérapique de Nancy (CPN)**.  
Elle permet de **gérer les réservations** des activités bien-être du personnel (Massage AMMA, Réalité Virtuelle, Luminothérapie, etc.) via une interface simple, moderne et configurable.

L’application inclut :
- une **interface publique** pour les réservations ;
- une **interface d’administration** complète ;
- un **serveur Node.js / Express** pour la gestion des données et des mails.

---

## ⚙️ Fonctionnalités principales

### 👥 Utilisateur (interface publique)
- Réservation d’un créneau de service en quelques clics  
- Génération automatique d’une **invitation .ics** (Outlook / Gmail)  
- Envoi d’emails de **confirmation et d’annulation**  
- Blocage automatique des créneaux complets ou fermés  

### 🛠️ Administrateur (interface `config.html`)
- Gestion des **services** (libellé, durée, capacité)  
- Configuration des **jours ouverts/fermés**  
- Création rapide de **créneaux horaires** par jour et service  
- Définition de **périodes de fermeture** et de **blocages ponctuels**  
- Personnalisation des **modèles d’emails automatiques**  
- Interface responsive avec **mode clair/sombre** et **thèmes de couleur**  
- Sauvegarde automatique dans un fichier JSON (`config.json`)  

---

## 🧩 Architecture technique


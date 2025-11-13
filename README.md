# README.md - AutoDSF 🚀

**Automatisation de la Déclaration Statistique et Fiscale pour PME**

---

## 📋 Description

AutoDSF est une application web moderne qui automatise la préparation, le calcul et la soumission de la Déclaration Statistique et Fiscale (DSF) pour les petites et moyennes entreprises. Elle intègre un audit intelligent détectant les risques fiscaux et garantit une archivage sécurisé des déclarations.

**MVP en cours** : Version 1.0 ciblant la génération automatisée depuis fichiers Excel (API Genuka en préparation).

---

## ✨ Fonctionnalités clés

- ✅ **Multi-entreprises** : Gestion centralisée pour comptables et conseillers
- 📊 **Import** : Upload Excel ou synchronisation ERP (Genuka, Odoo, QuickBooks)
- 🧮 **Calcul automatique** : Produits, charges, TVA, résultat fiscal
- 🔍 **Audit intelligent** : Détection d'incohérences et anomalies fiscales
- 📄 **Génération PDF** : Déclaration au format réglementaire certifié
- 🔒 **Archivage sécurisé** : Historique complet avec traçabilité (audit trail)
- 🔔 **Notifications** : Relances échéances, anomalies détectées, statuts
- 👥 **Rôles utilisateurs** : Administrateur, comptable, consultant externe

---

## 🛠️ Stack Technique

| Composant | Technologie |
|-----------|-------------|
| **Backend** | PHP 8.3 + Laravel 12 |
| **Frontend** | React 18 + Inertia.js (Breeze) + TailwindCSS |
| **Base de données** | MySQL 8.0+ ou PostgreSQL 15+ |
| **API** | Laravel HTTP Client (pour Genuka) |
| **Génération PDF** | DomPDF |
| **Files** | Laravel Excel (Maatwebsite) |
| **Notifications** | Laravel Mail + SMS (Twilio) |
| **Activity Log** | Spatie Laravel Activity Log |
| **Queue** | Redis ou database driver |
| **Tests** | PHPUnit + Pest |

---

## 📋 Prérequis

- PHP ≥ 8.3 avec extensions : `mbstring`, `xml`, `zip`, `gd`, `pdo_mysql`
- Composer ≥ 2.5
- Node.js ≥ 20.x + npm
- MySQL ≥ 8.0 ou PostgreSQL ≥ 15
- Redis (optionnel, pour les queues)

---

## 🚀 Installation

```bash
# 1. Cloner et installer les dépendances
composer create-project laravel/laravel AutoDSF
cd AutoDSF

# 2. Installer Laravel Breeze avec React
composer require laravel/breeze --dev
php artisan breeze:install react
npm install

# 3. Configurer la base de données
cp .env.example .env
# Éditer .env (voir section Configuration ci-dessous)

# 4. Lancer les migrations
php artisan migrate

# 5. Lancer le serveur de développement
npm run dev
php artisan serve
```

---

## ⚙️ Configuration du fichier `.env`

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=autodsf
DB_USERNAME=root
DB_PASSWORD=

# Mail (pour les notifications)
MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025

# Queue (décommenter si Redis)
# QUEUE_CONNECTION=redis

# Genuka API (laisser vide pour l'instant, utilisera le mock)
GENUKA_BASE_URL=
GENUKA_API_KEY=
```

**Créer la base de données** :
```sql
CREATE DATABASE autodsf CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

---

## 📁 Structure des dossiers (modulaire)

```
app/
├── Modules/
│   ├── Core/          # Modèles Company, UserRole
│   ├── DSF/           # Calcul, génération, déclarations
│   ├── Audit/         # Règles, résultats d'audit
│   ├── Import/        # Upload Excel, intégration ERP
│   ├── Notification/  # Classes de notification
│   └── Reporting/     # Rapports de performance fiscale
├── Services/
│   ├── GenukaMock/        # Mock API Genuka
│   └── PDFGenerator/      # Service génération PDF
├── Http/
│   ├── Middleware/        # RequireCompanyAccess
│   └── Controllers/API/   # Routes API protégées
database/
├── factories/         # Factories pour tests
│   ├── DSF/
│   └── Audit/
└── seeders/           # RoleSeeder, AuditRuleSeeder
resources/js/
├── Pages/             # Composants React par module
│   ├── Dashboard.jsx
│   ├── Import/
│   ├── DSF/
│   └── Audit/
└── Components/        # CompanySelector, NotificationBell
```

---

## 🔧 Commandes Artisan essentielles

```bash
# Générer une déclaration pour une entreprise
php artisan dsf:calculate {company_id} {year}

# Lancer les workers de queue (en dev, dans un terminal séparé)
php artisan queue:work

# Vider le cache après modification des règles
php artisan cache:clear

# Lancer les tests
php artisan test --filter=DSFCalculation

# Créer un admin (à implémenter)
php artisan autodsf:create-admin
```

---

## 🗺️ Roadmap des modules

| Sprint | Module | Durée | Description |
|--------|--------|-------|-------------|
| **1** | **Fondations & Auth** | 3 jours | Multi-entreprises, rôles, middleware |
| **2** | **Import données** | 5 jours | Upload Excel + mock API Genuka |
| **3** | **Moteur DSF** | 4 jours | Calcul des indicateurs fiscaux |
| **4** | **Système d'audit** | 5 jours | Détection anomalies + notifications |
| **5** | **Génération PDF** | 4 jours | Template PDF + archivage |
| **6** | **Notifications** | 3 jours | Email, SMS, journal d'activité |
| **7** | **Frontend React** | 7 jours | Dashboard, UI, responsive design |
| **8** | **Tests & Docs** | 2 jours | Tests unitaires + doc API |

---

## 🎮 Lancement en développement

**Terminal 1** (Frontend) :
```bash
npm run dev
```

**Terminal 2** (Backend) :
```bash
php artisan serve
```

**Terminal 3** (Queues) :
```bash
php artisan queue:work
```

**Mailpit** (pour catcher les emails) :
```bash
docker run -d -p 1025:1025 -p 8025:8025 axllent/mailpit
# Consulter les emails à http://localhost:8025
```

---

## 📚 Documentation complémentaire

- [Guide d'installation détaillé](docs/INSTALL.md)
- [Documentation API](docs/API.md)
- [Règles de calcul DSF](docs/REGLES_DSF.md)
- [Seeding de données de test](docs/SEEDING.md)

---

## 🤝 Contribution

1. Créer une branche `feature/nom-du-module`
2. Commiter avec messages clairs : `feat: ajoute calcul TVA #3`
3. Pousser et créer une Pull Request
4. S'assurer que les tests passent : `php artisan test`

---

## 📄 Licence

MIT License - Voir le fichier `LICENSE` pour plus de détails.

---

**Made with ❤️ pour simplifier la vie des PME et des experts-comptables**

# 📊 Projet Power BI – SecurSenegal

Ce projet illustre la création d’un **dashboard interactif** pour une entreprise fictive de sécurité et surveillance au Sénégal.  
Il combine **SQL/PostgreSQL** pour la gestion des données et **Power BI** pour la visualisation.

---

## 🚀 Objectifs
- Centraliser les données des **clients, installations, contrats, incidents et revenus**.  
- Créer des visuels dynamiques (KPI, cartes, courbes, filtres interactifs).  
- Faciliter la **prise de décision rapide** dans le domaine de la sécurité.

---

## 🛠️ Outils utilisés
- **SQL / PostgreSQL** : modélisation et requêtes analytiques  
- **Power BI** : visualisation et reporting  
- **Excel/CSV** : préparation des données  

---

## 🗄️ Script SQL complet

```sql
-- Création de la base
CREATE DATABASE secur_senegal;
\c secur_senegal;

-- Table Clients
CREATE TABLE clients (
    id_client VARCHAR(10) PRIMARY KEY,
    nom_client VARCHAR(100),
    secteur VARCHAR(50),
    ville VARCHAR(50),
    date_inscription DATE,
    statut VARCHAR(20)
);

-- Table Installations
CREATE TABLE installations (
    id_installation VARCHAR(10) PRIMARY KEY,
    id_client VARCHAR(10) REFERENCES clients(id_client),
    type_camera VARCHAR(50),
    quantite INT,
    date_installation DATE,
    technicien VARCHAR(50),
    cout_total NUMERIC
);

-- Table Contrats
CREATE TABLE contrats (
    id_contrat VARCHAR(10) PRIMARY KEY,
    id_client VARCHAR(10) REFERENCES clients(id_client),
    type_contrat VARCHAR(50),
    duree_mois INT,
    date_signature DATE,
    montant NUMERIC,
    statut VARCHAR(20)
);

-- Table Incidents
CREATE TABLE incidents (
    id_incident VARCHAR(10) PRIMARY KEY,
    id_client VARCHAR(10) REFERENCES clients(id_client),
    type_incident VARCHAR(100),
    date_incident DATE,
    statut_resolution VARCHAR(20),
    technicien VARCHAR(50),
    delai_resolution INT
);

-- Table Revenus
CREATE TABLE revenus (
    mois VARCHAR(20) PRIMARY KEY,
    total_revenus NUMERIC,
    depenses NUMERIC,
    benefice NUMERIC
);

-- Insertions exemples
INSERT INTO clients VALUES
('C001','Banque Atlantique','Banque','Dakar','2024-02-10','Actif'),
('C002','Hôtel Teranga','Hôtellerie','Saly','2024-03-05','Actif'),
('C003','Supermarché Azur','Commerce','Thiès','2024-04-12','Actif'),
('C004','École Excellence','Éducation','Dakar','2024-05-02','Inactif');

INSERT INTO installations VALUES
('I001','C001','Caméra PTZ 360°',8,'2024-02-15','Mamadou Diop',2400000),
('I002','C002','Caméra thermique',4,'2024-03-10','Awa Ndiaye',1800000),
('I003','C003','Caméra IP HD',12,'2024-04-15','Cheikh Ba',3600000),
('I004','C004','Caméra dôme',6,'2024-05-05','Fatou Sall',1500000);

INSERT INTO contrats VALUES
('CT001','C001','Maintenance annuelle',12,'2024-02-20',600000,'En cours'),
('CT002','C002','Maintenance trimestrielle',3,'2024-03-12',150000,'Terminé'),
('CT003','C003','Maintenance semestrielle',6,'2024-04-18',300000,'En cours');

INSERT INTO incidents VALUES
('IN001','C001','Coupure alimentation caméra','2024-03-05','Résolu','Mamadou Diop',2),
('IN002','C002','Défaut capteur','2024-04-10','En cours','Awa Ndiaye',5),
('IN003','C003','Intrusion détectée','2024-05-20','Résolu','Cheikh Ba',1);

INSERT INTO revenus VALUES
('Janvier',1200000,400000,800000),
('Février',2400000,600000,1800000),
('Mars',3000000,800000,2200000),
('Avril',3600000,900000,2700000),
('Mai',4200000,1000000,3200000);

-- Requêtes analytiques
-- Top 3 clients par coût d’installation
SELECT c.nom_client, SUM(i.cout_total) AS total_installations
FROM clients c
JOIN installations i ON c.id_client = i.id_client
GROUP BY c.nom_client
ORDER BY total_installations DESC
LIMIT 3;

-- Taux de résolution des incidents
SELECT statut_resolution, COUNT(*) AS nb_incidents
FROM incidents
GROUP BY statut_resolution;

-- Évolution des revenus mensuels
SELECT mois, total_revenus, benefice
FROM revenus
ORDER BY mois;

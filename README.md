# 📊 Analyse du Stock et des Ventes - Bottleneck

## 🎯 Objectif

Ce projet fournit une analyse complète et approfondie des stocks et des ventes du site Bottleneck. Il utilise **Python** et **Jupyter Notebook** pour explorer, nettoyer et analyser des données multi-sources, identifier les anomalies et générer des insights exploitables.

**Objectif principal**: Fiabiliser les données de Bottleneck avant intégration ERP
**Données analysées** : Ventes, stocks, produits
**Enjeux** : Pilotage du CA, optimisation des stocks, conformité réglementaire

---


## 🔍 Structure de l'Analyse

### **Étape 1 - Importation et Chargement des Données**
- Importation des librairies (`pandas`, `plotly.express`, etc.)
- Chargement de 3 fichiers sources :
  - `web.xlsx` : Données de ventes web
  - `erp.xlsx` : Données ERP (stocks, prix, marges)
  - `liaison.xlsx` : Table de liaison entre les sources

### **Étape 2 - Analyse Exploratoire des Données**

#### 2.1 Fichier ERP
- Analyse des variables clés :
  - 💰 **PRIX** : Distribution, statistiques descriptives
  - 📦 **STOCK** : Niveaux d'inventaire
  - 🌐 **ONSALE_WEB** : Disponibilité en ligne
  - 💵 **Prix d'achat** : Coûts et marges

#### 2.2 Fichier Web
- Analyse des données de ventes en ligne
- Comportements d'achat clients

#### 2.3 Fichier Liaison
- Vérification des clés de jointure
- Intégrité des données

### **Étape 3 - Fusion des Données**
- 🔗 Fusion `df_erp` + `df_liaison`
- 🔗 Fusion du résultat + `df_web`
- Création d'une base de données unifiée

### **Étape 4 - Analyse Univariée des Prix**

#### 4.1 Visualisation des Données
- Histogrammes et distribution des prix
- Identification des tendances

#### 4.2 Analyse Statistique
- **Z-index** : Identification des valeurs aberrantes
- **Intervalle interquartile (IQR)** : Détection des outliers

### **Étape 5 - Analyse Multivariée**

#### 5.1 Analyse des Ventes en CA
- Chiffre d'affaires par produit, catégorie, période

#### 5.2 Analyse des Quantités Vendues
- Volumes de vente et tendances

#### 5.3 Analyse des Stocks
- Niveaux d'inventaire
- Rotation des stocks
- Risques de surstock/rupture

#### 5.4 Analyse du Taux de Marge
- Marges brutes et nettes
- Rentabilité produits

#### 5.5 Corrélations
- Relations entre **stock**, **ventes** et **prix**
- Insights sur la dynamique produit

#### 5.6 Export des Résultats
- Génération d'un fichier Excel avec les données consolidées

---

## 📊 Technologies Utilisées

| Technologie | Usage |
|-------------|-------|
| **Python 3** | Langage de programmation principal |
| **Pandas** | Manipulation et transformation des données |
| **Plotly Express** | Visualisations interactives et graphiques |
| **Jupyter Notebook** | Environnement d'analyse exploratoire |
| **Excel** | Importation et export de données |

---
## 📊 Guide des Fichiers de Données

- [**erp.xlsx**](./erp.xlsx)- Données ERP Interne
- **Taille:** 38 KB
- **Lignes:** 825 produits
- **Colonnes:** 6 (product_id, price, stock_quantity, stock_status, etc.)
- **Format:** Données structurées, sans valeurs manquantes
- **Clé primaire:** `product_id`
- **Utilisation:** Source de vérité pour les prix, stocks et coûts

**À savoir:**
- Tous les 825 produits ont des données complètes
- Le `stock_status` est binaire: "instock" ou "outofstock"
- Le `onsale_web` indique si le produit est visible en ligne

---


- [**web.xlsx**](./web.xlsx) - Données e-Commerce 
- **Taille:** 331 KB
- **Lignes:** 1513 références
- **Colonnes:** 29 (SKU, titre, ventes, avis, metadata, etc.)
- **Format:** Données partielles (certaines colonnes vides)
- **Clé unique:** `sku`
- **Utilisation:** Source pour les ventes, avis clients, métadonnées produits

**À savoir:**
- Contient à la fois des produits ET des pièces jointes 
- Seuls ~1430 enregistrements sont des produits réels
- Les colonnes de contenu (post_content) sont vides
- Les avis (average_rating) ne sont disponibles que pour ~1430 produits

---

- [**liaison.xlsx**](./liaison.xlsx) - Table de Mapping
- **Taille:** 22 KB
- **Lignes:** 825 correspondances
- **Colonnes:** 2 (product_id ERP ↔ id_web )
- **Format:** Simple mapping 1-to-1
- **Utilisation:** Clé de jointure entre ERP et Web

**À savoir:**
- Permet de relier les produits ERP aux SKU web
- 734 id_web remplis sur 825 (91% de couverture)
- Certains produits ERP n'ont pas d'équivalent en ligne

---

[**df_merge_sorted.xlsx**](./df_merge_sorted.xlsx)  - RÉSULTAT FINAL 
- **Taille:** 1.3 MB
- **Lignes:** 9183 observations
- **Colonnes:** 41 (fusion complète + calculs)
- **Format:** Dataset prêt pour analyse
- **Clé:** `product_id`
- **Utilisation:** Tous les analyses statistiques et visualisations

**Contenu:**

| Catégorie | Colonnes | Exemples |
|-----------|----------|----------|
| **ERP** | 6 | product_id, price, stock_quantity, purchase_price |
| **Web** | 23 | post_title, total_sales, average_rating, product_type |
| **Calculs** | 8 | CA, taux_marge, prix_HT, valorisation_stock, part_quantite |
| **Liaison** | 1 | id_web |

**Colonnes Analytiques Clés:**
- `CA` - Chiffre d'affaires (Prix × Quantités)
- `taux_marge` - Rentabilité produit (%)
- `prix_HT` - Prix hors taxes
- `valorisation_stock` - Valeur du stock au coût
- `part_quantite` - % du volume de ventes du produit
- `part_quantite_cum` - Cumul % (pour analyse ABC)

---

## 📈 Flux de Données

```
┌─────────────┐
│  erp.xlsx   │  825 produits
│ (Vérité)    │  Stocks, Prix, Coûts
└──────┬──────┘
       │
       │  JOIN sur product_id
       ▼
   ┌────────────────────┐
   │   liaison.xlsx     │  825 mappings
   │  (Clé de liaison)  │  ERP ↔ Web
   └────────┬───────────┘
            │
            │  JOIN sur product_id
            ▼
       ┌────────────┐
       │  web.xlsx  │  1513 références
       │ (E-commerce)│ Ventes, Avis, Metadata
       └────┬───────┘
            │
            │  FUSION COMPLÈTE (avec LEFT JOIN)
            │
            ▼
  ┌──────────────────────────┐
  │ df_merge_sorted.xlsx     │  ⭐ RÉSULTAT FINAL
  │ (9183 lignes × 41 cols)  │  Dataset consolidé
  │                          │  Prêt pour analyse
  └──────────────────────────┘
            │
            ├─ Graphiques Plotly
            ├─ Analyses statistiques
            ├─ Détection d'anomalies
            └─ Recommandations stratégiques



### Consulter les résultats
- Visualisations dans le notebook
- Présentation PowerPoint pour un résumé executive
- Fichier Excel exporté avec les données consolidées

---

## 📈 Principaux Insights

L'analyse permet de répondre à des questions critiques :

✅ **Quels produits performent le mieux ?** (CA, quantités)  
✅ **Quels sont les stocks problématiques ?** (surstock, rupture)  
✅ **La stratégie tarifaire est-elle optimale ?** (corrélation prix/ventes)  
✅ **Où se cachent les anomalies ?** (prix aberrants, données manquantes)  
✅ **Quelles sont les marges réelles ?** (par produit, catégorie)  

---

## 📁 Livrables

### 📌 Fichiers Inclus

- [**Notebook Jupyter**](./Kamoune_Assia_1_notebook_122025.ipynb) - Analyse détaillée avec code Python, visualisations et résultats
-  [**Présentation**](./Kamoune_Assia_2_presentation_122025.pptx) - Présentation executive avec findings et recommandations

### 📊 Outputs Générés

- Graphiques et visualisations interactives (dans le notebook)
- Fichier Excel consolidé avec les résultats d'analyse
- Présentation avec recommandations stratégiques

---

## 🎓 Points Clés d'Apprentissage

Ce projet démontre :

- 📖 Étapes complètes d'une **analyse de données** (exploration → nettoyage → analyse → visualisation)
- 🔗 Techniques de **fusion de données** depuis plusieurs sources
- 📊 **Visualisations** interactives pour la communication d'insights
- 🔍 Méthodes de détection des **valeurs aberrantes** (Z-index, IQR)
- 📈 **Analyse statistique** et corrélations entre variables
- 💼 Structuration d'un travail analytique pour un auditoire business

---


## 📝 Notes

- Le notebook est structuré de manière pédagogique avec explications à chaque étape
- Les commentaires dans le code guident les utilisateurs sur les approches recommandées
- Il existe souvent plusieurs solutions valides pour un même problème analytique
- Les graphiques Plotly offrent une interactivité riche pour l'exploration

---

## 🔗 Ressources Utiles

- [Documentation Pandas](https://pandas.pydata.org/docs/)
- [Documentation Plotly Express](https://plotly.com/python/plotly-express/)
- [Jupyter Notebook Guide](https://jupyter.org/)
- [Stack Overflow](https://stackoverflow.com/) - Pour trouver des solutions



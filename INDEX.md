# 📑 Index des Livrables - Analyse Bottleneck

## 🗂️ Structure du Projet

```
📦 Analyse_Bottleneck_Stock_Ventes/
│
├── 📄 README.md                          (⭐ START HERE)
├── 📄 INDEX.md                           (Ce fichier)
│
├── 📊 FICHIERS DE DONNÉES
│   ├── erp.xlsx                          (825 produits - ERP)
│   ├── web.xlsx                          (1513 références - e-commerce)
│   ├── liaison.xlsx                      (825 mappings ERP↔Web)
│   └── df_merge_sorted.xlsx              (9183 lignes - RÉSULTAT FINAL)
│
├── 📓 ANALYSES
│   ├── Kamoune_Assia_1_notebook_122025.ipynb    (Notebook complet avec code)
│   └── Kamoune_Assia_2_presentation_122025.pptx (Présentation executive)
```

---

## 🎯 Accès Rapide

### 1️⃣ **Je veux comprendre le projet**
→ Lire [README.md](./README.md)

### 2️⃣ **Je veux voir l'analyse complète**
→ Ouvrir [Kamoune_Assia_1_notebook_122025.ipynb](./Kamoune_Assia_1_notebook_122025.ipynb)

### 3️⃣ **Je veux la présentation executive**
→ Consulter [Kamoune_Assia_2_presentation_122025.pptx](./Kamoune_Assia_2_presentation_122025.pptx)

### 4️⃣ **Je veux travailler avec les données**
→ Télécharger et utiliser [df_merge_sorted.xlsx](./df_merge_sorted.xlsx)

---

## 📊 Guide des Fichiers de Données

### 🔷 **erp.xlsx** - Données ERP Interne
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

### 🔶 **web.xlsx** - Données e-Commerce WooCommerce
- **Taille:** 331 KB
- **Lignes:** 1513 références
- **Colonnes:** 29 (SKU, titre, ventes, avis, metadata, etc.)
- **Format:** Données partielles (certaines colonnes vides)
- **Clé unique:** `sku`
- **Utilisation:** Source pour les ventes, avis clients, métadonnées produits

**À savoir:**
- Contient à la fois des produits ET des pièces jointes (images)
- Seuls ~1430 enregistrements sont des produits réels
- Les colonnes de contenu (post_content) sont vides
- Les avis (average_rating) ne sont disponibles que pour ~1430 produits

---

### 🔗 **liaison.xlsx** - Table de Mapping
- **Taille:** 22 KB
- **Lignes:** 825 correspondances
- **Colonnes:** 2 (product_id ERP ↔ id_web WooCommerce)
- **Format:** Simple mapping 1-to-1
- **Utilisation:** Clé de jointure entre ERP et Web

**À savoir:**
- Permet de relier les produits ERP aux SKU web
- 734 id_web remplis sur 825 (91% de couverture)
- Certains produits ERP n'ont pas d'équivalent en ligne

---

### ✨ **df_merge_sorted.xlsx** - RÉSULTAT FINAL ⭐
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
```

---

## 🔍 Questions que Vous Pouvez Répondre

### Avec **erp.xlsx** seul:
- Quel est le prix moyen des produits?
- Combien de produits sont en rupture?
- Quel est la valeur totale du stock?

### Avec **web.xlsx** seul:
- Quels produits ont les meilleures évaluations?
- Quel est le chiffre d'affaires web total?
- Quel est le produit le plus vendu?

### Avec **liaison.xlsx** seul:
- Quel est le taux de couverture web/ERP?
- Quels produits ERP n'existent pas en ligne?

### Avec **df_merge_sorted.xlsx** (COMPLET):
✅ Analyse ABC - Produits par importance de CA
✅ Corrélations - Prix vs Ventes vs Évaluations
✅ Anomalies - Prix ou stocks aberrants
✅ Rentabilité - Marges par produit/catégorie
✅ Rotation stock - Produits lents vs rapides
✅ Optimisation - Recommandations de repricing
✅ Performance - Top/Flop produits

---

## 🚀 Cas d'Usage

### 📊 Analyste Données
1. Charger `df_merge_sorted.xlsx`
2. Créer des dashboards/visualisations
3. Identifier les tendances et anomalies
4. Proposer des optimisations

### 💼 Responsable Commercial
1. Consulter la présentation PowerPoint
2. Analyser la performance par catégorie
3. Identifier les opportunités de cross-sell
4. Optimiser la stratégie tarifaire

### 🛠️ Data Scientist
1. Utiliser le notebook comme base
2. Appliquer du ML (prédiction de stock, demand forecasting)
3. Créer des modèles de pricing dynamique
4. Tester des recommandations

### 📦 Responsable Stocks
1. Analyser les produits en rupture
2. Identifier les surstocks
3. Optimiser la rotation des stocks
4. Prévoir les besoins

---

## 📚 Ressources Incluses

### Dans le Notebook
- ✅ Code Python réutilisable
- ✅ Exemples d'analyse
- ✅ Visualisations interactives
- ✅ Bonnes pratiques Jupyter

### Dans la Présentation
- ✅ Insights clés
- ✅ Visualisations executive
- ✅ Recommandations
- ✅ Conclusions

### En Bonus (ce fichier)
- 📋 Architecture des données
- 🎯 Guide d'utilisation rapide
- 💡 Cas d'usage pratiques

---

## 🔗 Navigation Rapide

| Vous êtes... | Allez à... | Objectif |
|---|---|---|
| 🆕 Nouveau | [README.md](./README.md) | Comprendre le projet |
| 👨‍💼 Décideur | [Présentation PPTX](./Kamoune_Assia_2_presentation_122025.pptx) | Vue executive |
| 👨‍💻 Développeur | [Notebook](./Kamoune_Assia_1_notebook_122025.ipynb) | Code et détails |
| 📊 Data Analyst | [df_merge_sorted.xlsx](./df_merge_sorted.xlsx) | Données prêtes |
| 🔧 Tech Lead | [Tous les fichiers](#structure-du-projet) | Vue globale |

---

## ✅ Checklist de Démarrage

- [ ] Lire le README.md
- [ ] Télécharger les fichiers Excel
- [ ] Ouvrir le Notebook Jupyter
- [ ] Exécuter les cellules du notebook
- [ ] Consulter la présentation PowerPoint
- [ ] Charger df_merge_sorted.xlsx pour analyse poussée

---

## 📞 Support

Pour toute question sur:
- **Les données** → Consulter le README et les descriptions de colonnes
- **L'analyse** → Voir le Notebook avec commentaires détaillés
- **La stratégie** → Lire la présentation PowerPoint

---

**Version:** Décembre 2025  
**Auteur:** Assia Kamoune  
**Mise à jour:** 2025-12-02

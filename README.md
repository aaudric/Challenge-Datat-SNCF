# 🚉 Prédiction des Validations Journalières par Gare - SNCF Transilien

Projet réalisé dans le cadre du Master 2 MIASHS (Université Montpellier III) pour le challenge organisé par SNCF-Transilien sur [Challenge Data](https://challengedata.ens.fr/participants/challenges/149/).

---

## 📌 Contexte

SNCF-Transilien, opérateur des trains de banlieue en Île-de-France, fait circuler plus de 6 200 trains et transporte 3,2 millions de voyageurs chaque jour. Ces derniers valident leur titre de transport en moyenne **2,3 millions de fois par jour**.

🎯 **Objectif** : Prédire à moyen-long terme le **nombre de validations par jour et par gare**, afin de :

- Anticiper les volumes de voyageurs
- Mieux comprendre les dynamiques d’affluence
- Accompagner les évolutions du réseau de transport

---

## 🗃️ Données

- **Train.csv** : 1 237 971 lignes (2015–2022)
- **Test.csv** : 78 652 lignes (janvier–juin 2023)
- **Nombre de gares** : 448 gares distinctes
- **Colonnes** :
  - `date` : jour de validation
  - `station` : identifiant anonymisé de la gare
  - `job`, `ferie`, `vacances` : indicateurs contextuels
  - `y` : nombre de validations

### 🛠 Feature Engineering

- Variables temporelles : `weekday`, `weekofyear`, `month`, `quarter`, etc.
- Variables de retard : `lag_1_log`, `lag_7_log`, `lag_30_log`, `lag_365_log`
- Encodage des stations
- Transformation logarithmique de `y`

---

## 📊 Analyse Exploratoire

- Tendance haussière entre 2015 et 2019
- Chute brutale en 2020 (COVID-19), reprise à partir de 2021
- Saisonnalité hebdomadaire forte (pics tous les 7 jours)
- Autocorrélation significative jusqu’à 60 jours

---

## 🧠 Modélisation

### Méthodes explorées

- Approches classiques : SARIMA, ARIMA, Prophet
- Machine Learning : XGBoost, LightGBM
- Deep Learning : CNN, LSTM, GRU
- Transformers pour séries temporelles multivariées

### Stratégies COVID

- Ajout d’une variable indicatrice COVID
- Remplacement ou lissage des valeurs anormales
- Apprentissage uniquement post-COVID (2021+)

---

## 🧮 Métrique d'Évaluation

La métrique utilisée pour évaluer les performances est le **MAPE** (*Mean Absolute Percentage Error*), définie comme : 
$$
\text{MAPE} = \frac{1}{n} \sum_{i=1}^{n} \left| \frac{y_i - \hat{y}_i}{y_i} \right|
$$
---

## 🏆 Résultats

### 🥇 Meilleur Modèle

- **Modèle** : `XGBoost` par station (post-COVID uniquement)
- **Features** : Feature engineering + indicateurs contextuels
- **Score public (MAPE)** : **143.64**
- **Classement final** : **66ᵉ sur 200 participants**
- **Benchmark officiel du challenge** : 177.08 (91ᵉ place)

### Classement des soumissions par score public (MAPE)

| Rang | Méthode                            | Features                                      | MAPE |
|------|------------------------------------|-----------------------------------------------|------|
| 1    | XGBoost par station post-COVID     | Feature engineering + indicateurs contextuels | **143.64** |
| 2    | XGBoost par station                | Feature engineering + indicateurs contextuels | 150.22 |
| 3    | XGBoost par station                | Indicateurs contextuels uniquement            | 182.66 |
| 4    | XGBoost post-COVID                 | Feature engineering + indicateurs contextuels | 217.43 |
| 5    | SARIMA                             | Feature engineering + indicateurs contextuels | 231.04 |
| 6    | LightGBM post-COVID                | Feature engineering + indicateurs contextuels | 252.35 |
| 7    | Projection tendances 2021–2022     | Copie 2022 + ∆(2021−2022)                     | 349.52 |

---

## 🔍 Analyse des Résidus

- Erreurs plus élevées pour les faibles affluences → instabilité (hétéroscédasticité)
- Bonne précision pour les fortes affluences (≥ 100 000 validations)
- Erreurs globalement symétriques mais présence d’outliers

---

## 🔮 Perspectives

- Clustering des gares pour des modèles spécialisés
- Intégration de données externes (événements, météo…)
- Analyse des outliers pour comprendre les anomalies
- Expérimentation de modèles hybrides (LSTM + XGBoost, etc.)

---

## 🖼️ Poster du Projet

📎 Un résumé visuel du projet est disponible ici : [`poster.pdf`](./poster/poster.pdf)

---

## 📚 Références
Voir le dossier [Littérature](./Littérature/) du dépôt.


---

© **Audric Girondin** — Master 2 MIASHS, Université Montpellier III


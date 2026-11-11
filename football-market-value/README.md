# Analyse de la valeur marchande des joueurs de Premier League (2025-2026)

## Objectif du projet

L'objectif de ce projet est d'étudier les principaux facteurs permettant d'expliquer la valeur marchande des joueurs de Premier League lors de la saison 2025-2026 à l'aide de leurs caractéristiques individuelles et de leurs performances sportives.

Ce projet a pour finalité de répondre à la problématique suivante :
**Comment expliquer la valeur marchande des joueurs de Premier League lors de la saison 2025-2026 ?**

Pour répondre à cette question, nous allons utiliser des méthodes d'analyse de données, de statistiques descriptives et d'économétrie à l'aide de Python.

---

## Description générale

Ce projet s'articule autour de trois grandes étapes :

1. **Collecte et préparation des données** : collecte des données sur les joueurs via Transfermarkt pour les caractéristiques et FBref pour leurs performances, nettoyage de ces données puis fusion pour aboutir à un seul jeu de données.
2. **Analyse descriptive** : analyse de la distribution des valeurs marchandes et de leurs relations avec les caractéristiques des joueurs.
3. **Analyse économétrique** : estimation de plusieurs modèles de régression linéaire, réalisation de tests statistiques et analyse de la contribution des variables explicatives.

À l'issue du traitement des données, nous obtenons un jeu de données final comprenant **505 joueurs de Premier League**.

---

## Arborescence du projet

Voici l'organisation générale du projet :

```text
football-market-value/
│
│
├── data/
│   ├── interim/                     # Données nettoyées et intermédiaires
│   │   ├── df_merge_clean.csv
│   │   ├── fbref_clean.csv
│   │   └── transfermarkt_clean.csv
│   │   
│   ├── processed/                   # Jeu de données final
│   │   └── dataset_final.csv
│   │
│   └── raw/                         # Données brutes collectées
│       ├── fbref.csv
│       └── premier_league_2025_2026.csv
│
├── notebooks/
│   ├── 01_data_collection/
│   │   └── 01_scrapping_transfermarkt.ipynb
│   │
│   ├── 02_data_preparation/
│   │   └── 02_merging_and_cleaning.ipynb
│   │
│   └── 03_analyse/
│       ├── 03_descriptive_analysis.ipynb
│       └── 04_econometric_analysis.ipynb
│
├── README.md                        # Présentation du projet
└──  requirements.txt                 # Dépendances Python
```

Les dossiers `data/` distinguent les différentes étapes de traitement des données, tandis que les notebooks retracent la démarche suivie tout au long du projet.

---

## 1. Collecte et préparation des données

### Sources utilisées

Les données proviennent de deux sources principales.

**Transfermarkt**

https://www.transfermarkt.com/premier-league/startseite/wettbewerb/GB1/saison_id/2025

Transfermarkt permet notamment de récupérer :

- L'identité des joueurs.
- Leur âge.
- Leur poste.
- Leur valeur marchande estimée.

**FBref**

https://fbref.com/en/comps/9/2025-2026/stats/2025-2026-Premier-League-Stats?__cf_chl_tk=QRbqiEhJ78f4fmnmJ8nfJSWGmwX1Op_28Ka2.HWL2X4-1791233279-1.0.1.1-AaactjuIlnwgRXrL.McETwBb5D7PF77yQZTjEDbdayc

FBref fournit les statistiques sportives utilisées dans notre analyse, notamment :

- Le nombre de matchs joués.
- Le nombre de titularisations.
- Les minutes jouées.
- Les buts marqués.
- Les passes décisives.
- Différentes statistiques rapportées à 90 minutes.

### Méthodologie

La préparation des données repose sur plusieurs opérations successives :

1. Collecte des informations disponibles sur Transfermarkt via web scraping et FBref via importation.
2. Nettoyage des données et harmonisation des variables.
3. Identification et rapprochement des joueurs présents dans les deux sources.
4. Fusion des bases de données.
5. Vérification de la qualité du jeu de données obtenu.
6. Construction du dataset final utilisé dans les analyses.

Le fichier final est disponible à l'emplacement suivant :

`data/processed/dataset_final.csv`

Il comprend **505 observations**, correspondant à 505 joueurs distincts.

---

## 2. Analyse descriptive

Cette étape vise à comprendre la structure de notre jeu de données et à identifier les premières relations entre la valeur marchande et les caractéristiques des joueurs.

### Analyses réalisées

Nous avons notamment étudié :

- La distribution des valeurs marchandes.
- La relation entre l'âge et la valeur marchande.
- Les différences de valeur marchande selon les postes.
- Les relations entre la valeur marchande, le temps de jeu et les performances offensives.
- Les corrélations entre les différentes variables.

### Principaux constats

L'analyse descriptive met en évidence plusieurs résultats intéressants.

Tout d'abord, la relation entre l'âge et la valeur marchande semble non linéaire : les joueurs relativement jeunes présentent généralement des valeurs marchandes plus élevées, tandis que celles-ci tendent à diminuer aux âges plus avancés.

Nous observons également des associations positives entre la valeur marchande et plusieurs indicateurs sportifs, notamment les minutes jouées, les buts et les passes décisives.

Ces premiers constats motivent notre analyse économétrique, qui vise à étudier ces relations en contrôlant simultanément plusieurs caractéristiques des joueurs.

---

## 3. Analyse économétrique

L'objectif de cette partie est de quantifier les associations entre les caractéristiques des joueurs et leur valeur marchande.

### Méthode utilisée

Nous estimons plusieurs modèles de régression linéaire par la méthode des **moindres carrés ordinaires (MCO)**.

La variable expliquée est le logarithme de la valeur marchande des joueurs.

Cette transformation permet notamment de limiter l'asymétrie de la distribution des valeurs marchandes et de faciliter l'interprétation économique de certains coefficients.

Nous utilisons des **erreurs standards robustes HC3** afin de rendre les inférences statistiques moins sensibles à une éventuelle hétéroscédasticité.

### Modèles estimés

Quatre modèles ont été construits progressivement :

| Modèle | Variables explicatives |
|---|---|
| M1 | Âge et âge au carré |
| M2 | Âge, âge au carré et minutes jouées |
| M3 | Âge, âge au carré, minutes jouées, buts et passes décisives |
| M4 | Variables de M3 et indicatrice du poste de gardien |

Cette démarche permet d'étudier l'amélioration du pouvoir explicatif lorsque de nouvelles variables sont introduites.

### Diagnostics économétriques

Différents outils sont mobilisés pour étudier la pertinence des modèles :

- Tests de Wald pour examiner certaines hypothèses sur les coefficients.
- Analyse graphique des résidus.
- Calcul des facteurs d'inflation de la variance (VIF) pour étudier la multicolinéarité.
- Comparaison des modèles à l'aide du R², du R² ajusté, de l'AIC et du BIC.

### Modèle retenu

À l'issue de cette comparaison, nous retenons le **modèle M3**, qui présente un bon compromis entre pouvoir explicatif, simplicité et interprétabilité.

Sa spécification est la suivante :

$$
\log(V_i)=\beta_0+\beta_1 Age_i+\beta_2 Age_i^2+\beta_3 Minutes_i+\beta_4 Buts_i+\beta_5 Passes_i+\varepsilon_i
$$

Ce modèle présente un coefficient de détermination de **0,599**.

Il explique ainsi environ **59,9 % de la variabilité du logarithme de la valeur marchande** des joueurs de notre échantillon.

L'ajout d'une indicatrice de gardien dans M4 n'apporte pas d'amélioration suffisamment importante pour justifier la complexification du modèle.

---

## 4. Principaux résultats

### Effet de l'âge

Nos estimations mettent en évidence une **relation en U inversé entre l'âge et la valeur marchande**.

Toutes choses égales par ailleurs, la valeur marchande estimée augmente dans un premier temps avec l'âge, atteint un maximum autour de **23 ans**, puis diminue.

### Effet du temps de jeu

Le nombre de minutes jouées présente une association positive et statistiquement significative avec la valeur marchande.

À caractéristiques contrôlées identiques, les joueurs accumulant davantage de temps de jeu tendent à présenter des valeurs marchandes plus élevées.

### Effet des performances offensives

Les buts et les passes décisives présentent également des associations positives et statistiquement significatives.

Ces résultats suggèrent que les performances offensives contribuent à expliquer les différences de valeur marchande entre les joueurs.

### Décomposition de Shapley

Afin d'approfondir l'interprétation de notre modèle, nous effectuons une **décomposition de Shapley du R²**.

Cette méthode permet de répartir le pouvoir explicatif du modèle entre ses différents facteurs, en tenant compte de leurs contributions marginales dans plusieurs combinaisons de variables.

L'âge et son carré sont regroupés au sein d'un même facteur.

Les résultats approximatifs sont les suivants :

| Facteur | Contribution relative au R² |
|---|---:|
| Âge et âge au carré | 55 % |
| Minutes jouées | 27 % |
| Buts | 9 % |
| Passes décisives | 9 % |

Ces résultats montrent que **l'âge et le temps de jeu représentent les contributions les plus importantes au pouvoir explicatif de notre modèle**.

Il convient toutefois de préciser que cette décomposition ne mesure pas des effets causaux et que les pourcentages sont relatifs au R² expliqué, et non à la variabilité totale de la valeur marchande.

---

## 5. Limites et perspectives

### Limites de l'étude

Notre étude présente plusieurs limites.

Premièrement, notre modèle repose sur un nombre relativement restreint de caractéristiques. Certains facteurs susceptibles d'influencer la valeur marchande ne sont pas pris en compte, notamment :

- La durée restante du contrat.
- La réputation et le potentiel du joueur.
- L'historique des blessures.
- Les performances défensives.
- Certaines caractéristiques propres au club.

Deuxièmement, notre analyse porte uniquement sur la Premier League et sur une seule saison. Les résultats ne peuvent donc pas nécessairement être généralisés à d'autres championnats ou périodes.

Troisièmement, les valeurs marchandes provenant de Transfermarkt sont des estimations et ne correspondent pas nécessairement aux montants réellement payés lors des transferts.

Ensuite, pour les joueurs ayant eu un transfert au cours de la saison et au sein de la premiere league, nous avons décider d'affecter ce joueur au club où il a eu le plus de temps de jeu. Ce choix peut etre discutable.
Enfin, les relations estimées doivent être interprétées comme **des associations statistiques, et non comme des effets causaux**.

### Perspectives d'amélioration

Plusieurs prolongements pourraient être envisagés :

- Enrichir le jeu de données avec des variables explicatives supplémentaires.
- Étendre l'analyse à plusieurs saisons ou championnats.
- Estimer des modèles distincts selon les postes des joueurs.
- Comparer les MCO à des méthodes de régression pénalisée, notamment **Ridge et Lasso**.
- Évaluer les performances prédictives des modèles sur un échantillon de test.
- Explorer des méthodes non linéaires de machine learning.

Ces extensions permettraient notamment de compléter notre approche explicative par une démarche davantage orientée vers la prédiction.

---

## Prérequis et installation

Le projet a été développé avec **Python** et repose principalement sur les bibliothèques suivantes :

| Bibliothèque | Utilisation |
|---|---|
| `pandas` | Manipulation et préparation des données |
| `numpy` | Calculs numériques |
| `matplotlib` | Visualisations |
| `seaborn` | Graphiques statistiques |
| `statsmodels` | Estimation et diagnostics économétriques |
| `pyshapley2` | Décomposition de Shapley |
| `requests` | Récupération de pages web |
| `beautifulsoup4` | Extraction de données HTML |
| `selenium` | Automatisation du navigateur |

Les dépendances du projet sont indiquées dans le fichier `requirements.txt`.

Pour les installer, exécuter depuis la racine du projet :

```bash
python -m pip install -r requirements.txt
```

L'utilisation d'un environnement virtuel Python est recommandée afin d'isoler les dépendances du projet.

---

## Guide d'utilisation

### Consultation des données

Le jeu de données utilisé dans les analyses se trouve dans :

```text
data/processed/dataset_final.csv
```

Les analyses descriptives et économétriques peuvent être reproduites directement à partir de ce fichier, sans avoir à relancer toute la collecte des données.

### Exécution des notebooks

Les notebooks sont organisés selon les différentes étapes du projet.

Pour comprendre l'ensemble de la démarche, il est recommandé de les consulter dans l'ordre suivant :

1. **Collecte des données** : récupération des informations depuis les sources.
2. **Préparation des données** : nettoyage, fusion et construction du dataset final.
3. **Analyse descriptive** : visualisations et étude des corrélations.
4. **Analyse économétrique** : estimations, tests statistiques, comparaison des modèles et décomposition de Shapley.

**Remarque :** les chemins relatifs utilisés pour lire les fichiers CSV dépendent du répertoire de travail du noyau Jupyter. Ils doivent être vérifiés si les notebooks sont exécutés depuis un autre emplacement.

### Remarque concernant le web scraping

Le notebook consacré au web scraping de Transfermarkt a été développé en fonction de la structure du site au moment de la réalisation du projet.

Il est donc possible que ce notebook ne fonctionne plus correctement à l'avenir, notamment si Transfermarkt modifie la structure HTML de ses pages ou renforce ses mécanismes de sécurité contre les requêtes automatisées.

Dans ce cas, certaines parties du code pourraient nécessiter des adaptations.

**Toutefois, les données déjà collectées et nettoyées sont conservées dans les dossiers du projet**, ce qui permet de reproduire les analyses descriptives et économétriques sans devoir exécuter à nouveau le web scraping, dès lors que les fichiers nécessaires sont disponibles.

La réutilisation et la redistribution des données provenant des sites sources restent soumises à leurs conditions respectives.

---

## Bilan

Ce projet nous a permis de mettre en œuvre une démarche complète d'analyse de données et de modélisation économétrique, depuis la collecte et la préparation des données jusqu'à l'interprétation des résultats.

Notre étude montre que **l'âge, le temps de jeu et les performances offensives sont significativement associés à la valeur marchande des joueurs de Premier League**.

Le modèle M3 explique une part importante de la variabilité observée dans notre échantillon, tout en conservant une spécification relativement simple et interprétable.

Enfin, la décomposition de Shapley permet de compléter l'analyse économétrique en mettant en évidence l'importance relative des différents facteurs dans le pouvoir explicatif du modèle.

Ce travail illustre ainsi l'intérêt de mobiliser les méthodes statistiques et économétriques pour étudier des problématiques concrètes dans le domaine du football professionnel.

## Contributeurs
- Groupe de projet : Les footix
- Développeur principal : Alaqad Zachary, Benjamin Roy et Joudy Zyne
 - Contact : alaqadzacharyassine@gmail.com, benjamin.roy.86000@gmail.com,
joudy.zyne@ensae.fr
 
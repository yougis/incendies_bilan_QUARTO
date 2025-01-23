# Rapport Automatisé des Bilans Incendie de l'OEIL

## Introduction

### Utilité de l'usage de Quarto     

Quarto facilite la génération de rapports en permettant de produire des documents de haute qualité qui combinent texte, visualisations et analyses de données. Les blocs de code, qui peuvent être multi-langages (R, Python, Julia, etc.), permettent l'automatisation des analyses et des mises à jour des données en temps réel. Il offre également des capacités de rendu multi-support, essentielles pour des rapports dynamiques et statiques.

De plus, Quarto améliore la collaboration en permettant à plusieurs utilisateurs de contribuer simultanément et de versionner les documents, assurant ainsi une production de rapports plus efficace et cohérente. Pour plus d'informations, consultez la [documentation officielle de Quarto](https://quarto.org/).

### Présentation du projet
Le projet "Rapport Automatisé des Bilans Incendie de l'OEIL" automatise la rédaction de rapports multi-annuels, réduisant ainsi le temps et l'effort nécessaires pour produire des analyses répétitives. Ces bilans quantifient les incendies et leurs impacts spatiaux et environnementaux sur le territoire de la Nouvelle-Calédonie, via des études comparatives annuelles et le croisement de multiples sources de données.

Ces bilans sont destinés à être produit sur trois niveaux de support :
- Niveau 1 : en pdf pour un rendu accessible et universel
- Niveau 2 : en html pour un rendu dynamique
- Niveau 3 : en docs pour un partage en interne

### Les limites
1. Impossibilité d'intégrer des analyses et des interprétations appronfondies automatisées : ce système n'est pas un moteur d'intelligence artificielle il nécessite une intervention humaine
2. Les erreurs d'intéropérabilités : versioning des librairies, mauvaise configuration des chemins relatifs et absolue en fonction des explorateurs

### Prérequis en termes de compétences
- Prise en main de la structure du projet
- Compréhension de ce qu'est un environnement conda
- Compréhension des bases de données PostgreSQL et des datamarts
- Connaissance de Markdown et Quarto [documentation officielle](https://quarto.org/)
- Compétences en Python et dans les librairies de datascience associées (ici on utilise : Holoviz [documentation officielle](https://holoviz.org))
- Compréhension du système des catalogues intake pour l'appel des données

## Structure du projet

Ce projet utilise la structure d'un "Book Quarto", combinant plusieurs documents (chapitres) en un seul manuscrit, disponible dans divers formats. Grâce à la numérotation des chapitres, le book HTML supporte les références croisées entre eux. Cette approche modulaire et flexible facilite la création de documents longs et complexes, tout en intégrant du code pour la datavisualisation. Pour plus de détails, consultez [cette rubrique](https://quarto.org/docs/books/) de la documentation officielle de Quarto.

- Le fichier _quarto.yml configure le projet, définissant les options de format, les métadonnées et la structure des chapitres.

- Le fichier _variables.yml contient des paramètres à mettre à jour pour le versioning des rapports.

## Schéma de la structure du projet

```
     ├── _book                  # Contient la structure du projet Book de Quarto, généré lors de la création du book en pdf
     ├── _extensions            # À ignorer (contient les extensions pour générer la première de couverure)
     ├── .quarto                # À ignorer
┌────├── catalogue              # Contient les catalogues Intake pour l'appel des données   
│    │   ├── datasets           # À ignorer
│    │   ├── carto.yaml         # Catalogue des données utiles à la réalisation des cartes
│    │   └── feux.yaml          # Catalogue des données thématiques (modification necéssaire pour le versionning des tables de fait)
│    ├── etude                  # Contient le premier code, n'est pas inclue dans la structure finale du rapport (_quarto)
│    ├── etude_                 # Contient la partie 1 & 2 de l'étude, pas de datavizualisation
├────├── etude_caracterisation  # Contient les sections et chapitres de la partie 'caracterisation' (setting à mettre à jour)
├────├── etude_impact           # Contient les sections et chapitres de la partie 'impact' (setting à mettre à jour)
├────├── guide_methodo          # Contient les différents chapitres et sections du guide méthodologique
│    ├── ressources             # Contient les éléments png/jpeg appelés dans les chapitres et sections du book
│    │   ├── guide_methodo      # Images du guide méthodologique
│    │   ├── logos              # Logos appelés dans 'partenaires.qmd'
│    │   └── cover.png          # Logo OEIL de la première de couverture
│────├── _quarto.yml            # Configuration, structuration et métadonnées du Book Quarto, notamment ses formats
│────├── _variable.yml          # Fichier de paramétrage du versionning du rapport, à mettre à jour
│    ├── .env                   # Fichier de configuration des paramètres d'environnement pour l'accès au serveur de données, à mettre à jour
│    ├── .gitignore             # Fichier indiquant les fichiers à ignorer pour GIT
│    ├── 2_partenaires.qmd              │
│    ├── 3_intro.qmd                    │
│    ├── 4_summary.qmd                  │
│    ├── 5_glossaire.qmd                │
│    ├── 6_datasources-catalogue.qmd    │
├────├── 7_etude_.qmd                   │
├────├── 8_etude_caracterisation.qmd    │  --> Chapitres et sections du rapport, structurés dans le fichier _quarto.yml, incluant les différentes sections
├────├── 9_etude_impact.qmd             │
│    ├── 10_conclusion.qmd              │
│    ├── 11_references.qmd              │
├────├── 12-1-guide-methodologique.qmd  │
└────├── 13-2-guide-methodologique.qmd  │
     ├── acknoledgments.qmd             │
     ├── custom-references.docx         │
     ├── datasources-catalogue.qmd      │
     ├── index.qmd                      │ # le premier fichiers qui structure le 'book' ne peut trier sinon quarto ne le reconnait plus
     ├── references.bib                 │ # contient les references bibliographiques
     └── README.md              # Fichier Markdown pour prendre en main le Book Quarto
```

## Composants techniques

### Langages et outils utilisés

Ce projet utilise principalement le langage Python pour la datavisualisation ainsi que le langage markdown et LaTeX pour la rédaction du rapport.

Les principales bibliothèques utilisés :

- **pandas** : manipulation et analyse des données
- **numpy** : opération mathématiques et manipulations de tableaux
- **geopandas** : opérations géospatiales sur des données vectorielles
- **holoviews**, **hvplot**, **geoviews** : librairie pour création de carte, figure et tableau par holoviz
- **datashader** : aide à la visualisation lors d'un gros flux de donnée, transformation des polygons en image
- **bokeh** : permet la visualisation des figures en png et html, "backend" d'holoviz
- **cartopy** : opération cartographique et visualisation de données géospatiales
- **pyproj** : transformation des projections et des coordonnées géographiques
- **dotenv** : gestion des variables d'environnement
- ...

### Usage du catalogue Intake pour l'importation des données

Intake est utilisé pour charger et gérer les catalogues de données, ce qui permet de centraliser et de standardiser l'accès aux différentes sources de données utilisées dans le projet. Une première structuration des données est réalisé via des requêtes SQL ce qui permet de rendre le traitement de l'importation des données moins lourde. Ceci peut également gérer le versionning du document (voir partie : Quick Start).

### Architecture schématiques des librairies

```
               +-------------------+           
               |                   |           
               | .env              |           
               | (configuration)   |
               | permet l'accès    |
               | à la base de      |
               | donnée            |
               |                   |           
               +---------+---------+           
                         |                              
                         |                              
                         v                              
               +---------+---------+           
               |                   |           
               | Intake Catalogs   |           
               | (catalogue/       |          
               | carto.yaml,       |           
               | feux.yaml)        |
               | Système pour      |
               | l'importation des |
               | données, certaine |
               | requête SQL à     |
               | modifier pour le  |
               | versionning       |   
               |                   |           
               +---------+---------+           
                         |                              
                         |                              
                         |                              
                         |                              
                         v                              
               +---------+---------+           
               |                   |           
               |Cartopy, HoloViews,|           
               |Geoviews,Datashader|          
               |(datavizualisation)|          
               |                   |           
               +-------------------+   
```

## Quick Start

1. **Cloner le dépôt** : cloner le repos sur Azure Devops, consulter le tutoriel pour plus d'informations
2. **Configurer l'environnment conda** : il est important de configurer son environnement conda afin d'avoir les bonnes versions des packages, un environnement nommé "quarto" est disponible, la commande à rentrer dans le terminal : "conda activate nom_de_votre_environnement". Pour plus d'information consulter le turoriel.
3. **Configurer les variables d'environnement** : le fichier '.env' contient ces variables, la modification du 'path' et des identifiants sont nécessaires pour accéder aux données
4. **Configurer GIT** : mettre son nom et adresse mail afin de pouvoir identifier les modifications du rapport, pour plus d'information consulter le tutoriel
5. **Gérer le versionning** : les variables "Annee" sont à modifier selon l'année d'étude en cours, dans '_variables.yml', dans 'setting.qmd' de 'etude_caracterisation' et 'etude_impact'. Il faut également modifier les requêtes SQL dans le catalogue intake pour les tables de fait

## Continuité de la réalisation du rapport

Pour finaliser ce rapport, des codes sont notifiés afin de s'y retrouver dans les tâches à finaliser. Avec Crtl+F et les codes textes suivants vous vous retrouverez pour la finalisation du dossier.

**#A FAIRE** : figure à réaliser ou partie à réaliser
**#A METTRE A JOUR** : 'crossreference' non référencé à mettre à jour
**#NOTE DE BAS DE PAGE** : il faut vérifier les notes de bas de page notifiés avec ce code texte
**#A VERIFIER** : notifier quand les parties 'informations' à vérifier

Il faut également vérifier, les parties écrites, les crossreferences, les parties informations...




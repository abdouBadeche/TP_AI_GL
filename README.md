# Rapport d'Analyse Formelle de Concepts sur les Fruits

## Introduction

Ce rapport présente l'application de l'analyse formelle de concepts (AFC) sur un ensemble de données décrivant des fruits. Initialement, l'analyse a été commencée avec le jeu de données "Mushroom" de l'UCI Machine Learning Repository. Cependant, en raison de la complexité et de la taille de ce jeu de données, il a été décidé de basculer vers un exemple plus simple pour illustrer les concepts fondamentaux de l'AFC.

## Choix du Dataset

Le jeu de données final utilisé est constitué de cinq fruits (Pomme, Banane, Cerise, Raisin, Orange) et de huit attributs (Rouge, Jaune, Vert, Petit, Moyen, Grand, Sucré, Acide). Chaque fruit est décrit par la présence (1) ou l'absence (0) de ces attributs.

### Tableau de Données

| Fruit | Rouge | Jaune | Vert | Petit | Moyen | Grand | Sucré | Acide |

|---------|-------|-------|------|-------|-------|-------|-------|-------|

| Pomme | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 |

| Banane | 0 | 1 | 0 | 0 | 1 | 1 | 1 | 0 |

| Cerise | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 1 |

| Raisin | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |

| Orange | 0 | 1 | 0 | 0 | 1 | 0 | 1 | 1 |

### Fichier CSV

```csv
Fruit,Rouge,Jaune,Vert,Petit,Moyen,Grand,Sucré,Acide
Pomme,1,0,1,0,1,0,1,1
Banane,0,1,0,0,1,1,1,0
Cerise,1,0,0,1,0,0,1,1
Raisin,1,0,1,1,0,0,1,0
Orange,0,1,0,0,1,0,1,1

```

## Méthodologie

1.  **Préparation des Données**:

- Les données ont été formatées dans un fichier CSV avec des valeurs binaires indiquant la présence ou l'absence d'attributs pour chaque fruit.

2.  **Utilisation de FCA4J**:

- Les commandes suivantes ont été exécutées pour générer les artefacts de l'AFC :

- **Treillis de Concepts**: Génération du fichier DOT et conversion en PDF.

- **Iceberg Lattice**: Génération avec un seuil de support de 50%.

- **AOCposet**: Construction de l'AOCposet.

- **Base d'Implications (DGBI)**: Calcul des implications logiques.

- **Éléments Irréductibles**: Identification des objets et attributs irrédutibles.

- **Clarification et Réduction**: Clarification et réduction du contexte formel.

### Étapes Suivies

1.  **Création des Répertoires**:

```bash

mkdir -p Fruits/Lattice

mkdir -p Fruits/Iceberg50

mkdir -p Fruits/AOCposet

mkdir -p Fruits/DGBI

mkdir -p Fruits/Reduced/Lattice

mkdir -p Fruits/Reduced/AOCposet

```

2.  **Génération du Treillis de Concepts**:

```bash

java -jar fca4j-cli-0.4.4.jar LATTICE fruits.csv -i CSV -s COMMA -g Fruits/Lattice/fruits_lattice.dot

dot -Tpdf Fruits/Lattice/fruits_lattice.dot -o Fruits/Lattice/fruits_lattice.pdf

```

3.  **Génération de l'Iceberg Lattice**:

```bash

java -jar fca4j-cli-0.4.4.jar LATTICE -a ICEBERG fruits.csv -p 50 -i CSV -s COMMA -g Fruits/Iceberg50/fruits_iceberg.dot

dot -Tpdf Fruits/Iceberg50/fruits_iceberg.dot -o Fruits/Iceberg50/fruits_iceberg.pdf

```

4.  **Génération de l'AOCposet**:

```bash

java -jar fca4j-cli-0.4.4.jar AOCPOSET fruits.csv -i CSV -s COMMA -g Fruits/AOCposet/fruits_aocposet.dot

dot -Tpdf Fruits/AOCposet/fruits_aocposet.dot -o Fruits/AOCposet/fruits_aocposet.pdf

```

5.  **Calcul de la Base d'Implications (DGBI)**:

```bash

java -jar fca4j-cli-0.4.4.jar RULEBASIS fruits.csv -i CSV -s COMMA -folder ./Fruits/DGBI/

```

6.  **Calcul des Éléments Irréductibles**:

- Objets Irréductibles:

```bash

java -jar fca4j-cli-0.4.4.jar IRREDUCIBLE fruits.csv -lobj -u -i CSV -s COMMA Fruits/irreducible_objects.txt

```

- Attributs Irréductibles:

```bash

java -jar fca4j-cli-0.4.4.jar IRREDUCIBLE fruits.csv -lattr -u -i CSV -s COMMA Fruits/irreducible_attributes.txt

```

7.  **Clarification et Réduction du Contexte**:

- Clarification:

```bash

java -jar fca4j-cli-0.4.4.jar CLARIFY fruits.csv -xa -xo -i CSV -s COMMA Fruits/fruits_clarified.csv

```

- Réduction:

```bash

java -jar fca4j-cli-0.4.4.jar REDUCE Fruits/fruits_clarified.csv -xa -xo -u -i CSV -s COMMA Fruits/fruits_clarified_reduced.csv

```

8.  **Génération du Treillis et de l'AOCposet Réduits**:

- Treillis Réduit:

```bash

java -jar fca4j-cli-0.4.4.jar LATTICE Fruits/fruits_clarified_reduced.csv -i CSV -s COMMA -g Fruits/Reduced/Lattice/fruits_reduced.dot

dot -Tpdf Fruits/Reduced/Lattice/fruits_reduced.dot -o Fruits/Reduced/Lattice/fruits_reduced.pdf

```

- AOCposet Réduit:

```bash

java -jar fca4j-cli-0.4.4.jar AOCPOSET Fruits/fruits_clarified_reduced.csv -i CSV -s COMMA -g Fruits/Reduced/AOCposet/fruits_reduced_aocposet.dot

dot -Tpdf Fruits/Reduced/AOCposet/fruits_reduced_aocposet.dot -o Fruits/Reduced/AOCposet/fruits_reduced_aocposet.pdf

```

## Résultats

1.  **Treillis de Concepts**:

- Le treillis de concepts montre les relations hiérarchiques entre les fruits et leurs attributs. Par exemple, les fruits rouges et sucrés forment un concept distinct.

2.  **Iceberg Lattice**:

- L'iceberg lattice met en évidence les concepts fréquents, filtrant ceux qui ne satisfont pas le seuil de support.

3.  **AOCposet**:

- L'AOCposet fournit une vue simplifiée des concepts, facilitant l'identification des relations essentielles.

4.  **Base d'Implications**:

- Les implications logiques identifiées révèlent des relations telles que "Si un fruit est rouge, alors il est souvent sucré".

5.  **Éléments Irréductibles**:

- Les objets et attributs irrédutibles indiquent les éléments essentiels du contexte qui ne peuvent être simplifiés sans perte d'information.

6.  **Contexte Clarifié et Réduit**:

- La clarification et la réduction du contexte ont permis d'éliminer les redondances, simplifiant ainsi l'analyse.

## Interprétation

L'analyse formelle de concepts a permis de découvrir des relations intéressantes entre les fruits et leurs attributs. Par exemple, les fruits rouges sont souvent sucrés, et les fruits moyens ou grands sont généralement jaunes. Ces insights peuvent être utilisés pour mieux comprendre les caractéristiques communes des fruits et pour des applications telles que la classification automatique.

## Conclusion

Ce rapport a démontré l'application de l'AFC sur un petit ensemble de données de fruits, illustrant comment cette méthode peut être utilisée pour extraire des connaissances significatives à partir de données structurées. Les résultats obtenus fournissent une base solide pour des analyses plus complexes et des applications pratiques.

## Fichiers Résultats

- **Treillis de Concepts**: `Fruits/Lattice/fruits_lattice.pdf`

- **Iceberg Lattice**: `Fruits/Iceberg50/fruits_iceberg.pdf`

- **AOCposet**: `Fruits/AOCposet/fruits_aocposet.pdf`

- **Base d'Implications**: Fichiers dans `Fruits/DGBI/`

- **Objets Irréductibles**: `Fruits/irreducible_objects.txt`

- **Attributs Irréductibles**: `Fruits/irreducible_attributes.txt`

- **Contexte Clarifié**: `Fruits/fruits_clarified.csv`

- **Contexte Réduit**: `Fruits/fruits_clarified_reduced.csv`

- **Treillis Réduit**: `Fruits/Reduced/Lattice/fruits_reduced.pdf`

- **AOCposet Réduit**: `Fruits/Reduced/AOCposet/fruits_reduced_aocposet.pdf`

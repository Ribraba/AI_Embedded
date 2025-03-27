# Projet de Maintenance Prédictive avec IA Embarquée

## Introduction

Ce projet s'inscrit dans le cadre de la maintenance prédictive et de l'Industrie 4.0. L'objectif est de prédire les pannes de machines industrielles en utilisant des données issues de capteurs. Pour ce faire, nous avons utilisé le dataset AI4I 2020 Predictive Maintenance Dataset, qui contient des informations détaillées sur les pannes de machines.

## Description du Dataset

Le dataset AI4I 2020 Predictive Maintenance Dataset contient des données de capteurs provenant de machines industrielles, avec des étiquettes indiquant les types de pannes. Les types de pannes incluent TWF (Tool Wear Failure), HDF (Heat Dissipation Failure), PWF (Power Failure), OSF (Overstrain Failure), et RNF (Random Failures). Cependant, les pannes sont inégalement réparties, avec une majorité de "No Specific Failure" (RNF), ce qui pose un problème de déséquilibre des classes.

## Analyse exploratoire (EDA)

L'analyse exploratoire des données a commencé par un aperçu général des données à l'aide des fonctions `head()`, `info()`, et `describe()`. Nous avons ensuite visualisé la répartition des différents types de pannes à l'aide de graphiques. Une analyse approfondie des cas où aucune panne spécifique n'est détectée a été réalisée. Enfin, nous avons identifié les variables d'entrée (features) et les cibles (labels) pour l'entraînement du modèle.

## Modélisation 1 — **Sans équilibrage**

Les données ont été divisées en ensembles d'entraînement et de test. Nous avons utilisé un modèle de type Multi-Layer Perceptron (MLP) pour l'entraînement sur les données non équilibrées. L'évaluation du modèle a révélé des courbes de perte et de précision, ainsi qu'un rapport de classification et des matrices de confusion. Cette première modélisation a mis en évidence un problème de détection des pannes rares.

## Modélisation 2 — **Avec équilibrage**

Pour résoudre le problème de déséquilibre des classes, nous avons décomposé le dataset en cas mono-label et multi-label. Nous avons appliqué des techniques d'équilibrage telles que l'undersampling pour réduire le nombre d'échantillons de la classe majoritaire et l'oversampling SMOTE pour augmenter le nombre d'échantillons des classes minoritaires. Un nouveau dataset équilibré a été créé et normalisé. Le même modèle a été réentraîné sur ce dataset équilibré, et l'évaluation a montré une amélioration des performances sur les classes minoritaires, comme en témoignent les nouvelles matrices de confusion.

## Export du modèle et données

Le modèle a été converti en format TFLite pour être utilisé avec STM32Cube.AI. Les données de test ont été sauvegardées sous forme de fichiers `.npy`, et le modèle a été exporté sous le nom `model.tflite`.

## Déploiement sur STM32L4R9 avec **STM32Cube.AI**

Le fichier `.tflite` a été converti dans STM32Cube.AI, et du code C embarqué a été généré pour la carte STM32. Le code a été compilé et flashé sur la carte via STM32CubeIDE. Une interface UART à 115200 bauds a été mise en place pour permettre la communication entre le modèle et le PC.

## Validation embarquée via UART (Python)

Le fichier `UART_y.py` a été utilisé pour valider le modèle embarqué. Le script synchronise la communication UART avec la carte STM32, envoie les données de test, et lit les prédictions renvoyées par le modèle. Les prédictions ont été comparées aux vraies classes pour calculer l'accuracy embarquée, prouvant ainsi que le modèle fonctionne correctement sur la carte.

## Synthèse des résultats

La comparaison des performances avant et après équilibrage montre une amélioration significative. Cependant, des écarts de précision entre les exécutions sur PC et STM32 subsistent. Les limites actuelles incluent la détection des pannes rares, les cas multi-label, et le bruit des capteurs. Des recommandations ont été formulées pour améliorer le système.

## Instructions pour exécuter le projet

Pour exécuter ce projet, les prérequis Python incluent `TensorFlow`, `imbalanced-learn`, `scikit-learn`, `matplotlib`, `numpy`, et `pyserial`. Les étapes à suivre sont : entraîner le modèle sur les données, exporter le modèle en `.tflite` ainsi que les fichiers `.npy`, importer le modèle dans STM32Cube.AI, déployer le modèle sur la carte STM32, et utiliser le script Python pour la communication UART.

## Structure du projet

```
Projet_IA_Embarquee
 ┣ tp_ia_embarquee.py
 ┣ UART.py
 ┣ ai4i2020.csv
 ┣ model.tflite
 ┣ X_test_ai4i.npy
 ┣ Y_test_ai4i.npy
```

# Projet-SmartRoad-Benin

## 💡 Introduction et Équipe

Nous sommes l'équipe **SMARTROAD BENIN**, composée de cinq membres :
* **Max HOUNPATIN** (Chef d'équipe)
* **Daniel TADJIGBAN**
* **ZANNOU Loïc**
* **FADONOUGBO Alexis**
* **AGBOTON Fabio**

Notre projet, **SMARTROAD BENIN**, a pour objectif de concevoir un système de gestion de feux de signalisation intelligent et adaptatif.C'est un système de feux intelligents qui adapte le vert au trafic réel (via ultrasons) pour fluidifier la circulation. Il affiche le temps restant et intègre un module GSM pour alerter les secours en cas d'urgence.


## 🛠️ Matériel Utilisé (BOM - Bill of Materials)

Cette section liste les composants matériels et outils essentiels à la conception et au fonctionnement du projet **SMARTROAD BENIN**.

| Composant | Quantité | Rôle dans le Projet |
| :--- | :---: | :--- |
| **Microcontrôleur** (Arduino UNO ou compatible) | 1 | Cœur du système, exécution du code logique. |
| **Registres à Décalage 74HC595** | 8 | **Extension de sorties** : Contrôle des 4 feux tricolores (12 LEDs) et des 8 afficheurs 7 segments. |
| **Multiplexeur CD4051** | 1 | **Extension d'entrées** : Permet de lire les données de 8 capteurs ultrasons à travers une seule broche analogique de l'Arduino. |
| **Capteurs Ultrasons HC-SR04** | 8 | Détection de la présence des véhicules sur les 4 voies (2 par voie), pour déterminer la densité du trafic. |
| **Module GSM SIM800L** | 1 | Gestion de la communication d'urgence (envoi de SMS et appel) en cas d'accident détecté. |
| **Afficheurs 7 Segments (Anode Commune)** | 8 | Affichage du temps d'attente pour chaque voie. |
| **LEDs (Rouge, Orange, Vert)** | 12 | Signalisation des feux tricolores (4 croisements * 3 couleurs). |
| **Bouton Poussoir** | 1 | Déclencheur manuel du mode Urgence (appui long). |
| **Résistances, Câbles, Alimentation** | N/A | Composants de base pour le câblage et l'alimentation. |
| **Outils** | N/A | Platine d'essai (Breadboard), Alimentation, carton, autocollant  (pour montage final). |


## 💻 Code Source

Le code source complet du projet (Programme Arduino) est disponible dans le fichier :
**[`src`](./src)**


### Aperçu des Fonctionnalités Clés

Voici un extrait des fonctions principales du système, mettant en évidence la logique dynamique :

| Fonction | Rôle |
| :--- | :--- |
| `lireDistance(int indexCapteur)` | Mesure la distance pour un capteur Ulrason sélectionné via le **multiplexeur CD4051**. |
| `lireCapteursEtAdapterTemps(int voie)` | Détermine le **temps VERT dynamique** de la voie en fonction de la densité du trafic détectée par les capteurs. |
| `calculerTempsAttente(int voie, int tempsRestantVoieActive)` | Calcule le temps d'attente précis pour les autres voies au ROUGE (temps affiché sur les digits). |
| `verifierUrgence()` | Gère le déclenchement de l'alarme (appui long) et l'envoi d'un SMS d'alerte via le **SIM800L**. |
| `afficherMultiplexe()` | Assure l'affichage clair et non clignotant des **8 digits** par multiplexage temporel rapide. |


## 🗒️ Journal de Bord : Défis et Solutions

La conception d'un système intelligent multi-voies a présenté plusieurs défis techniques, notamment les contraintes de nombre de broches (I/O) sur la carte Arduino, ainsi que la gestion du temps de projet.

| Problème Rencontré | Solution Technique Apportée | Impact/Résultat |
| :--- | :--- | :--- |
| **Contrainte d'I/O pour les Sorties (Feux et Afficheurs)** : Le nombre de LEDs (12) et les 8 afficheurs 7 segments dépassait largement les capacités de sorties numériques directes de l'Arduino. | Intégration de **8 Registres à Décalage 74HC595** en cascade. Ils permettent de contrôler l'ensemble des sorties (feux + segments des afficheurs) en n'utilisant que **trois broches** de l'Arduino (`Data`, `Clock`, `Latch`). | Réduction drastique de la consommation de broches, permettant le contrôle de 64 sorties logiques tout en libérant des pins pour les capteurs et le GSM. |
| **Contrainte d'I/O pour les Entrées (8 Capteurs Ultrasons)** : La lecture indépendante des 8 capteurs ultrasons (avec une broche `Trig` et une broche `Echo` par capteur) nécessitait trop de broches numériques et analogiques. | Utilisation du **Multiplexeur Analogique CD4051**. Ce circuit gère l'entrée `Echo` des 8 capteurs. Nous utilisons **trois broches** de l'Arduino pour sélectionner (adresser) quel capteur est lu à un instant donné. | Optimisation de l'utilisation des broches : tous les capteurs sont gérés avec seulement 4 broches (`Trig` commun + 3 broches d'adressage pour le CD4051). |
| **Contrainte de Temps** : La densité des activités académiques a réduit le temps effectif de travail sur le projet. | Application d'une méthodologie de travail agile et division claire des tâches (comme défini dans la **Répartition des Rôles**). | Malgré les contraintes, nous avons pu valider le concept (feux adaptatifs, affichage temps d'attente) et la fonctionnalité d'urgence (SIM800L). |

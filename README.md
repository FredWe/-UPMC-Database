# -UPMC-Database

Base de donnee


Avant de TP N°2, il nessicite de créer une base de donnee '*CaveUPMC*', et le shcéma '*dba01*'. Après executer le contenu dans le script fichier `scriptCAVEUPMC.sql`.

# TP N°2 : requêtes SQL

Nous souhaitons utiliser le modèle relationnel pour gérer une cave à vin. La base de données représentant cette cave possède le schéma relationnel suivant:

- ***VINS*** ( *num, cru, annee, degre* ) 
- ***PRODUCTEURS*** ( *num, nom, prenom, region* ) 
- ***RECOLTES*** ( *nprod, nvin, quantite* )

Un vin est caractérisé par un ***numéro***, un ***cru***, une ***année*** de production et un ***degré***. L’ensemble des vins est représenté par la relation VINS. La clé de la relation VINS est l’attribut ***num***.

Un producteur est caractérisé par un ***numéro***, un ***nom***, un ***prénom*** et une ***région***. L’ensemble des producteurs est représenté par la relation PRODUCTEURS. La clé de la relation PRODUCTEURS est l’attribut ***num***. Un producteur produit un ou plusieurs vins. Réciproquement, un vin est produit par un ou plusieurs producteurs (éventuellement par aucun !).

L’ensemble des productions est représenté par la relation RECOLTES. Un tuple de la relation RECOLTES représente une production particulière d’un vin de numéro ***nvin*** par un producteur de numéro ***nprod*** en une certaine quantité. La clé de la relation RECOLTES est le groupe d’attributs (***nvin***, ***nprod***).

Cette base est déjà construite, elle appartient au schéma postgres : pour l’utilisée il faut passer le nom du schéma + le nom de la table (exemple : postgres.vins).
L’objectif du TP est d’analyser cette base de données en répondant à la liste des questions suivante:

{ Ici c'est la version avec ma solution, je mets base de donnee comme '*CaveUPMC*', et tous les tables seront sous le schema nommee '*dba01*' }

1. Afficher l’ensemble des tables du schéma postgres (voir la documentation)

	S1: 
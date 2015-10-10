# -UPMC-Database

Base de donnee


Avant de TP N°2, il nessicite de créer une base de donnee '*CaveUPMC*', et le shcéma '*dba01*'. Après executer le contenu dans le script fichier `scriptCAVEUPMC.sql` pour saisir les 3 tables à opèrer.

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

	S1: `SELECT tablename FROM pg_tables WHERE schemaname = 'dba01'; `
	
	S1.rem: *Comment lister toutes les tables de ma base ?*
	
	`SELECT tablename FROM pg_tables WHERE tablename !~ '^pg_'; `
	
	Dans l'outil psql, on utilise la commande `\dt`
	
2. Afficher les informations contenues dans la relation VINS.

	S2: `SELECT * FROM dba01.vins;`
	
3. Donner la liste des producteurs qui n’ont pas de prénom.

	S3: `SELECT * FROM dba01.producteurs WHERE trim(prenom)='' OR prenom IS NULL;`

4. Donner la liste des régions de production des vins.

	S4: `SELECT DISTINCT region FROM dba01.producteurs;` ou `SELECT region FROM dba01.producteurs GROUP BY region;`
	
5. Donner la liste des vins de 1980 ordonnée par degré.

	S5: `SELECT * FROM dba01.vins WHERE annee = 1980 ORDER BY degre;`
	
6. Donner la liste par ordre alphabétique des noms et des prénoms des producteurs de vins n’appartenant pas aux régions suivantes : Corse, Beaujolais, Bourgogne et Rhône.

	S6: `SELECT * FROM dba01.producteurs WHERE lower(region) NOT IN ('corse','beaujolais','bourgogne','rhone') ORDER BY nom,prenom;`
	
7. Quelle est la liste des crus récoltés en 1979 ordonnée par numéro de producteur? Afficher le cru, le numéro du producteur et la quantité produite.

	S7: `SELECT cru,nprod,quantite FROM dba01.vins JOIN dba01.recoltes ON num = nvin WHERE annee = 1979 ORDER BY nprod;`
	
8. Quels sont les noms des producteurs du cru Etoile, leurs régions et la quantité de vins récoltés?

	S8: `SELECT nom,region,sum(quantite) AS etoile_quantite FROM dba01.vins JOIN dba01.recoltes ON vins.num = recoltes.nvin JOIN dba01.producteurs ON recoltes.nprod = producteurs.num WHERE lower(cru) = 'etoile' GROUP BY nprod,nom,region;`
	
9. Quel est le nombre de récoltes?

	S9: `SELECT count(*) FROM dba01.recoltes;`
	
10. Combien y-a-t-il de producteurs de vin dans la région Savoie et Jura?

	S10: `SELECT count(*) FROM dba01.producteurs WHERE lower(region) IN ('savoie','jura');`
	
11. Combien y-a-t-il de producteurs de vin ayant récoltés au moins un vin dans la région Savoie et Jura?

	S11: `SELECT count(*) FROM (SELECT count(nvin) AS count_nvin FROM dba01.producteurs JOIN dba01.recoltes ON num = nprod WHERE lower(trim(region)) IN ('savoie','jura') GROUP BY nprod) AS subq WHERE count_nvin >= 1;`
	
12. Quelles sont les quantités de vin produites par région. Donner la liste ordonnée par quantité décroissante.

	S12: 
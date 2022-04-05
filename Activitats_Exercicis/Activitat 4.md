## 4. INNODB part III. REALITZA ELS SEGÜENTS APARTATS (obligatòria)  

### Crea un tablespace anomenat 'ts1' situat a /discs-mysql/disc1/ i col·loca les taules actor, address i category de la BD Sakila.

***

- Crearem el tablespace ts1 indicant la ruta on el farem amb la següent comanda dins del MySQL:

*CREATE TABLESPACE `ts1`
ADD DATAFILE ‘ts1.ibd‘
ENGINE=InnoDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161842471-865dc046-8c29-4871-8b0e-df3d1ebdd7ce.png)

- Sortirem del MySQL mourem el tablespace ts1 cap al directori /discs-mysql/disk1/:

*mv ts1.ibd /discs-mysql/disk1/*

![imagen](https://user-images.githubusercontent.com/61557739/161842693-b572bc53-c341-483a-9220-804764a2fecc.png)

- A continuació col·locarem les taules actor, addres i category de la base de dades sakila al tablespace ts1 amb les següents comandes dins del MySQL:

*USE sakila;*

![imagen](https://user-images.githubusercontent.com/61557739/161842761-ca4d504f-6902-4f26-bdf0-cc736afa0c90.png)

***Actor***

*ALTER TABLE actor TABLESPACE ts1;*

![imagen](https://user-images.githubusercontent.com/61557739/161842840-810e0c2e-7b8b-4788-8847-9dbea2974091.png)

***Address***

*ALTER TABLE address TABLESPACE ts1;*

![imagen](https://user-images.githubusercontent.com/61557739/161842907-41faa9ab-a8b3-4d4f-819e-2a98b1999dc9.png)

***Category***

*ALTER TABLE category TABLESPACE ts1;*

![imagen](https://user-images.githubusercontent.com/61557739/161842967-56514f3d-9b86-48bd-8bce-54a23bb14394.png)

***

### Crea un altre tablespace anomenat 'ts2' situat a /discs-mysql/disc2/ i col·loca-hi la resta de taules.

***

- Anirem al MySQL i crearem el tablespace ts2 amb a següent comanda:

*CREATE TABLESPACE `ts2`
ADD DATAFILE ‘ts2.ibd‘
ENGINE=InnoDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161843110-f97e62a5-a159-4419-917a-b4362a2d3556.png)

- Sortirem del MySQL mourem el tablespace ts2 cap al directori /discs-mysql/disk2/:

*mv ts2.ibd /discs-mysql/disk2/*

![imagen](https://user-images.githubusercontent.com/61557739/161843250-53225a35-5901-46ee-a3c9-18f48a7ff26c.png)

- A continuació col·locarem la resta de taules de la base de dades sakila al tablespace ts2 amb les següents comandes dins del MySQL:

![imagen](https://user-images.githubusercontent.com/61557739/161843325-bf7dc967-41ed-4203-8413-e605ad541a19.png)

*ALTER TABLE city TABLESPACE ts2;
ALTER TABLE country TABLESPACE ts2;
ALTER TABLE customer TABLESPACE ts2;
ALTER TABLE film TABLESPACE ts2;
ALTER TABLE film_actor TABLESPACE ts2;
ALTER TABLE film_category TABLESPACE ts2;
ALTER TABLE film_text TABLESPACE ts2;
ALTER TABLE inventory TABLESPACE ts2;*

![imagen](https://user-images.githubusercontent.com/61557739/161843432-7e59a11e-1e8c-4248-90ec-60759288375e.png)


*ALTER TABLE language TABLESPACE ts2;
ALTER TABLE payment TABLESPACE ts2;
ALTER TABLE rental TABLESPACE ts2;
ALTER TABLE staff TABLESPACE ts2;
ALTER TABLE store TABLESPACE ts2;*

![imagen](https://user-images.githubusercontent.com/61557739/161843468-36a9bcfb-1156-4270-ac9a-9418f087fefc.png)

***

### Comprova que pots realitzar operacions DML a les taules dels dos tablespaces.

***

*USE sakila;*

![imagen](https://user-images.githubusercontent.com/61557739/161843577-da3ee28d-4686-4965-a66e-3aaa78ed88cf.png)

***Farem operacions DML de insert, select i delete en el tablespace ts1.***

- Ho farem en una taula ja que si les operacions DML funcionen en una taula funcionaran també en les demes.:

*INSERT INTO actor (first_name, last_name)
VALUES ('Daniel', 'Garcia Costa');*

![imagen](https://user-images.githubusercontent.com/61557739/161843669-2b1757d0-ee38-4714-8e12-b0f13ec95c4e.png)

*SELECT * FROM actor;*

![imagen](https://user-images.githubusercontent.com/61557739/161843699-0a878d3d-533c-4507-b49d-7a5657e1b12a.png)


*DELETE FROM actor
WHERE first_name ='Daniel';*

![imagen](https://user-images.githubusercontent.com/61557739/161843746-cc6dd7da-2b35-41a3-b8bd-f1545b277f80.png)

*SELECT * FROM actor;*

![imagen](https://user-images.githubusercontent.com/61557739/161843796-b808e595-44ca-4cbc-a940-04f000a1567c.png)


***Farem operacions DML de insert, select i delete en el tablespace ts2.***

- Ho farem en una taula ja que si les operacions DML funcionen en una taula funcionaran també en les demes.:

*INSERT INTO country (country)
VALUES ('Spain');*

![imagen](https://user-images.githubusercontent.com/61557739/161843898-0299f7d6-ad2a-4b59-a417-dee90e0faaa5.png)

*SELECT * FROM country;*

![imagen](https://user-images.githubusercontent.com/61557739/161843933-7297a7e0-87d2-493e-ba50-422aeca03d11.png)

 
*DELETE FROM country
WHERE country ='Spain';*

![imagen](https://user-images.githubusercontent.com/61557739/161843998-cf8e4db1-5c24-42c2-ad95-673555d78510.png)

*SELECT * FROM country;*

![imagen](https://user-images.githubusercontent.com/61557739/161844065-4b253207-9272-4653-9447-ac74a864df6f.png)

***

### Quines comandes i configuracions has realitzat per fer els dos apartats anteriors?

***

***Per utilitzar una base de dades:***

*USE Nom_Base_De_Dades;*

***Per crear els tablespaces:***

*CREATE TABLESPACE `Nom_Tablespace`
ADD DATAFILE ‘Nom_tablespace.ibd‘
ENGINE=Motor_Enmagatzematge;*

***Per col·locar taules en tablespaces:***

*ALTER TABLE Nom_Taula TABLESPACE Nom_Tablespace;*

***Per inserir dades a una taula:***

*INSERT INTO Nom_Taula (Nom_Columna1, Nom_Columna2...)
VALUES (Valor1, Valor2...);*

***Per mostrar totes les dades d’una taula:***

 *SELECT * FROM Nom_Taula;*


***Per eliminar els registres d’una taula:***

*DELETE FROM Nom_Taula
WHERE Nom_Columna = Valor*

***

*Ja estaria*

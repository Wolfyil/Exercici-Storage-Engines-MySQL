## 7. Storage Engine CSV


### • Documenta i posa exemple de com utilitzar ENGINE CSV.

### •	Cal documentar els passos que has hagut de realitzar per preparar l'exemple: configuracions, instruccions DML, DDL, etc....


***

***Que es el ENGINE CSV?***

- El motor d’emmagatzematge CSV enmmagatzema les dades en arxiu de text utilitzant el format de valors separats per comes.

- Quan creem una taula de tipus CSV, el servidor crea un fitxer de definició de taula en el directori de base de dades amb l’extensió *.frm*. 

- Després el motor crea un fitxer de dades amb el nom de la taula i la extensió *.CSV*. Aquest fitxer es un fitxer de text i quan s’emmagatzemen dades en la taula, el motor la guarda en el fitxer amb el format CSV.

- També crea un metaarxiu que emmagatzema el estat de la taula i el numero de files que existeixen en la taula amb la extensió *CSM*.


**A continuació faré un exemple de com utilitzar el ENGINE CSV.**

- Primer iniciarem sessió en el MySQL i utilitzarem una base de dades, en el meu cas la BDD sakila.:

*USE sakila;*

![imagen](https://user-images.githubusercontent.com/61557739/161852149-bfb090a7-2d2a-40e2-921f-247ac648b274.png)

- Després crearem una taula i posarem que el storage engine sigui CSV com ens demana l’exercici. Utilitzarem la següent comanda MySQL:

***Nota important:*** **CSV no suporta columnes de tipus AUTO_INCREMENT,  ni tampoc utilitzar PRIMARY KEY i tampoc suporta columnes buides.**

![imagen](https://user-images.githubusercontent.com/61557739/161852219-9b0beed3-f087-4b91-b96a-ebceca72d650.png)

![imagen](https://user-images.githubusercontent.com/61557739/161852226-bb87c07f-831c-4424-bf9c-c1d72d6a81eb.png)

![imagen](https://user-images.githubusercontent.com/61557739/161852240-eb84fbf8-1e03-4fc7-8c05-58f9265818d9.png)

- La forma correcta de crear una taula amb CSV es de la següent manera:

*CREATE TABLE Alumnes (
    ID INT NOT NULL,
    Nom VARCHAR(50) NOT NULL,
    Cognoms VARCHAR(50) NOT NULL
)
ENGINE = CSV;*

![imagen](https://user-images.githubusercontent.com/61557739/161852279-a7f83271-f76b-4fe2-b63f-738baccc2c19.png)


- Després de crear la taula, farem un insert per inserir valors a la taula que acabem de crear amb la següent forma:

*INSERT INTO Alumnes (ID, Nom, Cognoms)
VALUES (1, 'Daniel', 'Garcia Costa');*

![imagen](https://user-images.githubusercontent.com/61557739/161852320-9772fca1-13ef-44b6-bfdc-ed647dcad887.png)

*INSERT INTO Alumnes (ID, Nom, Cognoms)
VALUES (2, 'Nicolas', 'Perez Sanchez');*

![imagen](https://user-images.githubusercontent.com/61557739/161852375-34857aba-ed3f-4c00-958e-92e0eb60f469.png)

- Finalment farem un SELECT per comprovar que els inserts s’han inserit correctament a la taula Alumnes que hem creat:

*SELECT * FROM Alumnes;*

![imagen](https://user-images.githubusercontent.com/61557739/161852432-a73ff00a-8906-4e21-8989-676f6a5ace89.png)

- Sortirem del MySQL.


- Per últim i lo mes important es comprovar el fitxer Alumnes.CSV, que es un fitxer que es va crear automàticament quan vam crear la taula Alumnes amb el motor CSV i on esta registrat els inserts CSV que hem fet, en la base de dades sakila. 

- Aquest fitxer Alumnes.CSV es va crear en el directori */tspaces/sakila/*. 

![imagen](https://user-images.githubusercontent.com/61557739/161852558-04d9343d-c6ea-4bdc-ab22-977641edbb58.png)


- A continuació utilitzarem la següent comanda per comprovar els inserts de la taula Alumnes.CSV:

*cat Alumnes.CSV*


![imagen](https://user-images.githubusercontent.com/61557739/161852601-3247b9b2-c85d-4afc-b397-477f8460e4d4.png)

***

***Instruccions DDL que he utilitzat:***

*CREATE TABLE Alumnes (
    ID INT NOT NULL,
    Nom VARCHAR(50) NOT NULL,
    Cognoms VARCHAR(50) NOT NULL
)
ENGINE = CSV;*

***

***Instruccions DML que he utilitzat:***

*INSERT INTO Alumnes (ID, Nom, Cognoms)
VALUES (1, 'Daniel', 'Garcia Costa');*

*INSERT INTO Alumnes (ID, Nom, Cognoms)
VALUES (2, 'Nicolas', 'Perez Sanchez');*

*SELECT * FROM Alumnes;*

***

***Instrucció de comanda d’utilitat Linux que he utilitzat:***

*ls /tspaces/sakila/*

*cd /tspaces/sakila/*

*cat Alumnes.CSV*

***

*Ja estaria*









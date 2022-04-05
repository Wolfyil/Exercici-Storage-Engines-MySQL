## 8. Storage Engine MyRocks 

### Documenta i posa exemple de com utilitzar ENGINE MyRocks. 

***

***Que es el ENGINE MyRocks?***

- MyRocks proporciona un rendiment d’emmagatzematge flash millorat a traves de la seva eficiència en lectura, escriptura i emmagatzematge de dades. 
- Es optimitzat per un emmagatzematge ràpid i de baixa latència, es ideal per les aplicacions actuals amb gran volum i us intensiu d’escriptura com per exemple registre de transaccions, monitoratge de sistemes etc. 

- Requereix menys espai d’emmagatzematge SSD, mes resistència i una millor capacitat E/S

- Com a curiositat, MyRocks va ser desenvolupat per Facebook 

***


### •	Crea una Base de dades amb 2 o 3 taules i inserta-hi contingut.

### •	Cal documentar els passos que has hagut de realitzar per preparar l'exemple: configuracions, instruccions DML, DDL, etc....

***

**A continuació faré un exemple de com utilitzar el ENGINE MyRocks.**

- En el MySQL si utilitzem la següent comanda, podem comprovar que tenim el Engine MyRocks instal·lat:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161853441-3df69e89-00f0-41cc-a2a2-3c9641648144.png)

- Crearem una base de dades anomenada Exercici_MyRocks en el MySQL i la utilitzarem per després crear dues taules:

*CREATE DATABASE Exercici_MyRocks;*

*USE Exercici_MyRocks;*

![imagen](https://user-images.githubusercontent.com/61557739/161853846-4010a5db-2a20-444a-bffa-219e8e47bb34.png)

- A continuació crearem les dues taules següents en la base de dades Exercici_MyRocks.
- A diferencia del motor CSV amb el MyRocks es permet el: *AUTO_INCREMENT,  es pot utilitzar PRIMARY KEY i permet fer columnes buides*:

*CREATE TABLE Pel·licules (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE = ROCKSDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161853923-702cff39-4479-4291-a763-10c946bd55dd.png)


*CREATE TABLE Series (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE = ROCKSDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161853949-d75e25c6-af33-4633-bdb7-86b1d154b2ec.png)

- A continuació inserim dades en les següents taules amb les següents comandes:

*INSERT INTO Pel·licules (Nom)
VALUES ('Muerte en el Nilo');*

![imagen](https://user-images.githubusercontent.com/61557739/161853973-963d49b4-681b-47cd-8a99-164b06992673.png)

*INSERT INTO Series (Nom)
VALUES ('Breaking Bad');*

![imagen](https://user-images.githubusercontent.com/61557739/161854016-edb52456-e2a1-4d34-b1e1-bc5573d18076.png)

- Finalment farem un Select per comprovar en les dues taules que hem creat, per veure totes les dades:

*SELECT * FROM Pel·licules;*

![imagen](https://user-images.githubusercontent.com/61557739/161854053-892908d1-0c62-4161-8c0a-1bee8a095222.png)

*SELECT * FROM Series;*

![imagen](https://user-images.githubusercontent.com/61557739/161854087-7f4f8798-4466-4f42-a889-e3a9e9ec3f96.png)

***

***Instruccions DDL que he utilitzat:***

*CREATE TABLE Pel·licules (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE = ROCKSDB;*

*CREATE TABLE Series (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE = ROCKSDB;*

***

***Instruccions DML que he utilitzat:***

*INSERT INTO Pel·licules (Nom)
VALUES ('Muerte en el Nilo');*

*INSERT INTO Series (Nom)
VALUES ('Breaking Bad');*

*SELECT * FROM Pel·licules;*

*SELECT * FROM Series;*

***

### A quin directori es guarden els fitxers de dades? Fes un llistat de a on són els fitxers i què ocupen

***

- El directori on es guarden els fitxers de dades de la base de dades Exercici_MyRocks es troba en la ruta: */tspaces/Exercici_MyRocks/*

- Entrarem cap al directori i veurem els fitxers amb les següents comandes:

*cd /tspaces/Exercici_MyRocks/*

*ls*

![imagen](https://user-images.githubusercontent.com/61557739/161854454-304bbebd-50ae-49e1-bbdd-514f4d892ecc.png)

- Com podem observar utilitzant el motor MyRocks es guarden amb el format *.sdi*

- Comprovarem amb la següent comanda quant ocupen els fitxers. 
- Aquesta comanda el que fa es que mostra el total de quant ocupa els fitxers en el directori i ens surt la mida de quant pesen els arxius de forma individual, perquè la lectura sigui molt mes senzilla.

- Dins del directori */tspaces/Exercici_MyRocks/* executem la següent comanda per veure quant ocupen:

*ls -sh*

![imagen](https://user-images.githubusercontent.com/61557739/161854576-6e1192f6-b476-4d77-91c7-30ee6f853fb4.png)

- ***El fitxer de Pel·licules ocupa 4,0 Kilobytes.***

- ***El fitxer de Series ocupa 4,0 Kilobytes.***

***

###	Quina és la compressió per defecte que utilitza per les taules? 

***

- Per comprovar quina compressió utilitza per defecte, hem d’iniciar sessió en el MySQL i utilitzar la següent comanda:

*SELECT * FROM INFORMATION_SCHEMA.rocksdb_cf_options 
WHERE option_type LIKE '%ompression%' AND cf_name='default';*

![imagen](https://user-images.githubusercontent.com/61557739/161854729-e4692c2e-367b-4296-8d83-93949946db58.png)


**Com podem Observar utilitza la compressió de tipus LZ4**

***

### Com ho faries per canviar-lo. Per exemple utilitza Zlib o ZSTD o sense compressió

***

- ***A continuació canviarem el format que tenim nosaltres que es el LZ4 per el format ZSTD.***

- Per canviar-ho hem d’anar al fitxer */etc/my.cnf* i afegir la següent línia que això ens canviarà el format LZ4 cap al format ZSTD, guardem el fitxer i sortim:

*[mysqld]*

*rocksdb_default_cf_options=compression=kZSTD;bottommost_compression=kZSTD*

![imagen](https://user-images.githubusercontent.com/61557739/161854910-a2b44dc2-1cc3-439c-b693-bcf9b4e77b50.png)

- Finalment reiniciem el servei MySQL:

*systemctl restart mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161854959-8cb9bdf3-a3c0-4af3-b1bf-1ba1463d6b27.png)

- Després tornem a iniciar sessió en el Percona MySQL i comprovem que s’ha canviat la compressió al tipus ZSTD per defecte amb la següent comanda que vam utilitzar anteriorment:

*SELECT * FROM INFORMATION_SCHEMA.rocksdb_cf_options 
WHERE option_type LIKE '%ompression%' AND cf_name='default';*

![imagen](https://user-images.githubusercontent.com/61557739/161855079-0e15dab9-2f48-4486-8952-cdcd8bb8021a.png)

***

*Ja estaria*





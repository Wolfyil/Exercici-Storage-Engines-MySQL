## 6. Implementar BD Distribuïdes.

### Com s'ha vist a classe MySQL proporciona el motor d'emmagatzemament FEDERATED que té com a funció permetre l'accés remot a bases de dades MySQL en un servidor local sense utilitzar tècniques de replicació ni clustering.

***
### • Prepara un Servidor Percona Server amb la BD de Sakila

### • Prepara un segon servidor Percona Server a on hi hauran un conjunt de taules FEDERADES al primer servidor.

### • Per realitzar aquest link entre les dues BD podem fer-ho de dues maneres:

### •	Opció1: especificar TOTA la cadena de connexió a CONNECTION 

### •	Opció2: especificar una connexió a un server a CONNECTION que prèviament s'ha creat mitjançant CREATE SERVER

### •	Posa un exemple de 2 taules de cada opció.  Tingues en compte els permisos a nivell de BD i de SO així com temes de seguretat com firewalls, etc...

### •	Detalla quines són els passos i comandes que has hagut de realitzar en cada màquina.


***


***1er Servidor***

- Primer de tot hem de comprovar si tenim el engine FEDERATED activat, ho comprovarem amb la següent comanda en el MySQL:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161848279-b99bf2d2-205a-4f81-bd77-1601bec50034.png)

- Com podem veure esta desactivat el motor federated, per activar-ho hem d’anar al fitxer /etc/my.cnf i afegir la següent línia, guardem i sortim del fitxer:

*[mysqld]*

*federated*

![imagen](https://user-images.githubusercontent.com/61557739/161848363-5ac7fcf7-0ace-4285-bc54-813cedc6a64c.png)

- A continuació reiniciem el Percona MySQL: 

*systemctl restart mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161848418-87b3a9a7-de5d-42fa-b42f-eb7d5ce5b40c.png)

- Tornem a iniciar Sessió en el MySQL i comprovem que el FEDERATED esta habilitat amb la següent comanda:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161848466-32ca3573-f916-45c7-9e5d-55b490c5a383.png)

- Després d’activar el Federated en el primer servidor que utilitzem la BDD sakila.
- Crearem a continuació 4 taules dins d’aquesta amb les següents comandes per poder fer aquest exercici:

*CREATE TABLE Fruites (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
);*

![imagen](https://user-images.githubusercontent.com/61557739/161848583-788f0545-59fa-4adf-a82b-f84e7f49e678.png)

*CREATE TABLE Verdures (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
);*

![imagen](https://user-images.githubusercontent.com/61557739/161848626-11ffd902-f84b-4a3f-b4df-3e57a29efa83.png)

*CREATE TABLE Videojocs (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
);*

![imagen](https://user-images.githubusercontent.com/61557739/161848671-b20a4e0b-64cb-4c9d-a3d7-484e2bd05163.png)

*CREATE TABLE Supermercats (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
);*

![imagen](https://user-images.githubusercontent.com/61557739/161848734-8cec9568-e1c0-4063-92aa-2c751b1d4a2b.png)


- A continuació inserim dades a les taules que hem creat:

*INSERT INTO Fruites (Nom)
VALUES ('Pera');*

![imagen](https://user-images.githubusercontent.com/61557739/161848779-9620a8d4-3fc3-4a2f-97de-e12e81cb1e0c.png)


*INSERT INTO Verdures (Nom)
VALUES ('Broquil');
*

![imagen](https://user-images.githubusercontent.com/61557739/161848819-1fad3a68-0060-4af0-beb9-8861d67ad550.png)

*INSERT INTO Videojocs (Nom)
VALUES ('Star Wars Jedi: Fallen Order');
*

![imagen](https://user-images.githubusercontent.com/61557739/161848867-abb08e7b-1d1b-41b9-998a-c4929b60d15d.png)

*INSERT INTO Supermercats (Nom)
VALUES ('Mercadona');
*

![imagen](https://user-images.githubusercontent.com/61557739/161848906-c93c1211-d53e-4ba7-a99b-1ea343226f42.png)

- Comprovació amb el Select de les taules que hem creat:

*SELECT * FROM Fruites;*

![imagen](https://user-images.githubusercontent.com/61557739/161848995-24fd24b4-1349-4bdd-8b28-3a5512435277.png)

*SELECT * FROM Verdures;*

![imagen](https://user-images.githubusercontent.com/61557739/161849050-084af986-3694-4b9f-bea1-d3297e4d3aa8.png)

*SELECT * FROM Videojocs;*

![imagen](https://user-images.githubusercontent.com/61557739/161849093-3d8eec08-b4b8-46b7-a519-5e251d33c71d.png)

*SELECT * FROM Supermercats;*

![imagen](https://user-images.githubusercontent.com/61557739/161849128-e608c68f-660b-4dac-bf02-2012c5f3f393.png)

***

- Ara una vegada tot llest en el primer servidor. **Hem de preparar un segon servidor Percona for MySQL, en el meu cas agafaré una maquina neta i instal·laré el Percona Server for MySQL.**

***Es molt important que hem de mantenir la sessió i connexió dels dos Servidors sinó no funcionarà, en el meu cas utilitzo Putty i tinc dos finestres a l’hora, un servidor diferent en cada finestra.***

**Comprovació dels dos servidors encesos:**

![imagen](https://user-images.githubusercontent.com/61557739/161849261-2de5a547-78b5-4b6c-971c-0abb5b8f38ae.png)

***

***2n Servidor***

- Primer de tot hem de comprovar si tenim el engine FEDERATED activat en el segon servidor, ho comprovarem amb la següent comanda en el MySQL:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161849534-1e61b18a-67bf-4ce2-b525-2dc023a5ed2c.png)

- Com podem veure esta desactivat el motor federated, per activar-ho hem d’anar al fitxer /etc/my.cnf i afegir la següent línia, guardem i sortim del fitxer:

*[mysqld]*

*federated*

![imagen](https://user-images.githubusercontent.com/61557739/161849606-5d59c939-2a7f-4529-91e8-cd2bbf786810.png)

- A continuació reiniciem el Percona MySQL:

*systemctl restart mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161849659-9734811b-ff5a-4a36-8d77-708b29221a78.png)


- Tornem a iniciar Sessió en el MySQL i comprovem que el FEDERATED esta habilitat amb la següent comanda:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161849697-d02b6a4e-9ce4-4bd3-bb0a-1be27c248445.png)


- En aquest segon servidor crearem una base de dades anomenada Exercici_Federated, on farem les connexions Federated cap al primer Servidor:

*CREATE DATABASE Exercici_Federated;*

![imagen](https://user-images.githubusercontent.com/61557739/161849762-37d85431-53d2-4553-a642-0ec9588d0d6b.png)


- Utilitzarem la base de dades que hem creat en el segon servidor:

*USE Exercici_Federated*

![imagen](https://user-images.githubusercontent.com/61557739/161849812-8f9a4af7-0a4e-4e98-a535-e89ea79643bf.png)

***

### Opcio 1

***

- Primer desactivarem el Firewall en el Servidor 1 i Servidor 2:

*systemctl disable firewalld*

![imagen](https://user-images.githubusercontent.com/61557739/161849923-d42bc569-668e-4064-9e0d-1c10f70f7d85.png)

- Crearem 2 taules en el segon servidor amb la nomenclatura *_Federated*, posarem que utilitzi el motor Engine FEDERATED i que facin una connexió a la BDD sakila de la IP del Primer Servidor.
- Això lo que fara es obtenir les dades de les taules del primer Servidor i ens la passarà a les taules del segon servidor. De la següent forma:

*CREATE TABLE Fruites_Federated (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE=FEDERATED
CONNECTION='mysql://root:patata@192.168.56.37:3306/sakila/Fruites';*


![imagen](https://user-images.githubusercontent.com/61557739/161850008-a040646f-48a9-4c42-85a9-813db6671a9d.png)


*CREATE TABLE Verdures_Federated (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE=FEDERATED
CONNECTION='mysql://root:patata@192.168.56.37:3306/sakila/Verdures';*

![imagen](https://user-images.githubusercontent.com/61557739/161850060-af48281a-4a6b-4408-a149-49e35def3657.png)

- Finalment farem un select de les dues taules per veure si hem obtingut les dades del primer servidor fent la connexió:

*SELECT * FROM Fruites_Federated;*

***Hem dona error, he intentat donar-li permisos i desactivar els firewalls però no hem funciona***

![imagen](https://user-images.githubusercontent.com/61557739/161850195-208ad774-e87c-4941-b8eb-2b308c96a09e.png)

***

### Opcio 2

***


- Primer crearem el CREATE SERVER en el Servidor 2 amb la següent comanda, posarem la IP del primer servidor:

*CREATE SERVER Connexio_Servidor1
FOREIGN DATA WRAPPER mysql
OPTIONS (USER 'root', HOST '192.168.56.37', PORT 3306, DATABASE 'sakila');*


![imagen](https://user-images.githubusercontent.com/61557739/161850300-03601f2d-a9a2-4876-afb7-575b1617cd62.png)


- Després crearem les 2 ultimes taules taules en el segon servidor amb la nomenclatura *_Federated*, posarem que utilitzi el motor Engine FEDERATED i farem un CONNECTION d’una forma mes curta gracies al CREATE SERVER:

*CREATE TABLE Videojocs_Federated (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE=FEDERATED
CONNECTION='Connexio_Servidor1/Videojocs';*

![imagen](https://user-images.githubusercontent.com/61557739/161850379-35cc70d2-1884-4281-a385-1c25cee14f09.png)

*CREATE TABLE Supermercats_Federated (
    ID INT NOT NULL AUTO_INCREMENT,
    Nom VARCHAR(50) NOT NULL,
    PRIMARY KEY (ID)
)
ENGINE=FEDERATED
CONNECTION='Connexio_Servidor1/Supermercats';*

![imagen](https://user-images.githubusercontent.com/61557739/161850417-8659bc86-8d58-4f2c-93ad-9307ab13c543.png)

- Finalment farem un select de les dues taules per veure si hem obtingut les dades del primer servidor fent la connexió:

*SELECT * FROM Videojocs_Federated;*

***També aquí hem dona error:***

![imagen](https://user-images.githubusercontent.com/61557739/161850625-4ec64393-6491-49a8-a933-4196b69d7bd5.png)


***

*Ja estaria*

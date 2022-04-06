## 1. REALITZA I/O RESPON ELS SEGÜENTS APARTATS (obligatòria) 


### Indica quins són els motors d’emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d’aquesta.

***
- Hem de iniciar sessió en el MySQL i utilitzarem la següent comanda SQL per veure els motors d’emmagatzematge:

*SHOW ENGINES*

![imagen](https://user-images.githubusercontent.com/61557739/161742486-441526e8-6ef6-4f5a-a734-4fc0bb533883.png)


Els motors que estan actius son:

- PERFORMANCE_SCHEMA
- InnoDB
- MEMORY
- MyISAM
- MRG_MYISAM
- BLACKHOLE
- CSV
- ARCHIVE

El motor que no esta actiu es el: 
-	FEDERATED

***

### Com puc saber quin és el motor d’emmagatzematge per defecte? 
### Mostra com canviar aquest paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor MyISAM?

***

- Com podem observar en l’imatge. El motor d’emmagatzematge per defecte es el InnoDB, ja que ve en DEFAULT:

*SHOW ENGINES*

![imagen](https://user-images.githubusercontent.com/61557739/161743385-dcc32fde-da11-4dfa-8ca1-9df85a34c202.png)

- Per poder canviar que el motor d’emmagatzematge per defecte sigui el motor MyISAM en comptes del InnoDB hi ha 2 maneres possibles:

1. La primera manera es modificar el arxiu my.cnf. Que la tenim en la ruta /etc

![imagen](https://user-images.githubusercontent.com/61557739/161743591-4acac80a-be21-4bbe-9e76-9d952d71a3f3.png)

- Dins del arxiu my.cnf afegirem la següent línia de codi al fitxer:

*default-storage-engine = MyISAM*

![imagen](https://user-images.githubusercontent.com/61557739/161743732-72b34d7a-9e38-44e3-b2a0-4ea6acd60439.png)

- Guardem els canvis i reiniciem el servei mysql:

*systemctl restart mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161743818-a17d941d-5840-4c13-8007-bb26df16d73d.png)

- Després tornem a iniciar sessió en el Percona Server MySQL i comprovem que MyISAM sigui el motor d’emmagatzematge per defecte:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161743869-fb10eea9-9153-4fc5-930f-290895605a52.png)

***

2. La segona forma es iniciant sessió en el MySQL i utilitzar la següent comanda MySQL, per fer que MyISAM sigui el motor d’emmagatzematge per defecte:

*SET default_storage_engine=MyISAM;*

![imagen](https://user-images.githubusercontent.com/61557739/161744097-7db3a256-a2ed-41e8-8da1-0ebe957027d8.png)

***

### Explica els passos per instal·lar i activar l'ENGINE MyRocks. MyRocks és un motor d'emmagatzematge per MySQL basat en RocksDB (SGBD incrustat de tipus clau-valor). 
### Aquest tipus d’emmagatzematge està optimitzat per ser molt eficient en les escriptures amb lectures acceptables.

***

- Per instal·lar el MyRocks en el Percona Server 8x utilitzarem la següent comanda:

*sudo yum install percona-server-rocksdb*

![imagen](https://user-images.githubusercontent.com/61557739/161744321-97883797-78fd-409c-9eb8-2d35d3cbcc2a.png)

- A continuació utilitzarem la següent comanda per donar-li al usuari root MySQL els credencials per poder activar el ENGINE RocksDB (MyRocks):

*ps-admin --enable-rocksdb -uroot -ppatata*

![imagen](https://user-images.githubusercontent.com/61557739/161744407-7a22e669-2aad-4a87-bf57-dba095c56d28.png)

- Després iniciarem sesio en el Percona MySQL i comprovarem amb la següent comanda per veure si el Percona MyRocks esta activat:

*SHOW ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161744488-a3aa0545-8c0f-4903-b12d-7503df336aa5.png)


***

###	Importa la BD Sakila com a taules MyISAM. 
### Fes els canvis necessaris per importar la BD Sakila perquè totes les taules siguin de tipus MyISAM. 

***

- Primer de tot iniciarem sessió en el MySQL i importarem la BD sakila:

*SOURCE /root/sakila-schema.sql*

![imagen](https://user-images.githubusercontent.com/61557739/161744759-0291eb49-f65a-4e08-88fd-7325334cf2f3.png)

- Comprovació de la creació:

*SHOW DATABASES;*

![imagen](https://user-images.githubusercontent.com/61557739/161744834-1fa69daf-9540-424d-b9e4-76f11e88a1c5.png)

- Ara farem una comprovació per veure quin motor utilitza les taules de la BD Sakila amb la següent comanda:

*SELECT TABLE_NAME, ENGINE FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'sakila';*

![imagen](https://user-images.githubusercontent.com/61557739/161744905-593d8e9e-b62a-490a-93f7-9a4b8f3d7bb6.png)

![imagen](https://user-images.githubusercontent.com/61557739/161744932-e3bb1d6f-a9ac-45c4-811a-5e0529784b69.png)

- Com podem comprovar utilitzen InnoDB. A continuació canviarem aquestes taules Sakila de InnoDB a MyISAM utilitzant la següent comanda:

*SELECT CONCAT('ALTER TABLE ',TABLE_NAME, ' ENGINE=MyISAM;')  AS ComandaSQL
FROM Information_schema.TABLES WHERE TABLE_SCHEMA = 'sakila' AND ENGINE = 'InnoDB' AND TABLE_TYPE = 'BASE TABLE' ORDER BY TABLE_NAME ASC;*

![imagen](https://user-images.githubusercontent.com/61557739/161745034-9c72db0d-995d-404a-8699-74155b4b39b0.png)

- També es pot canviar les taules a MyISAM utilitzant la següent comanda, però dona error per les FK:

*ALTER TABLE actor ENGINE = MyISAM;*

![imagen](https://user-images.githubusercontent.com/61557739/161745094-97bc5a53-fa7b-4cba-929d-e555562c43cf.png)

***

### Mira quins són els fitxers físics que ha creat, quan ocupen i quines són les seves extensions. 
### Mostra'n una captura de pantalla i indica què conté cada fitxer.

***

- La ruta dels fitxers de taules de sakila es en:

*/var/lib/mysql/sakila/*

- Però no hem surt els fitxers MyISAM nomes de InnoDB. Ja que el MyISAM no funciona per el error de les FK.

![imagen](https://user-images.githubusercontent.com/61557739/161745225-ac1ab4f3-1c71-4db0-9f5b-1cbccc9a6d1b.png)

***

### Un cop fet això torna a deixar el motor InnoDB per defecte.

***

- Utilitzarem la següent comanda per posar el motor InnoDB per defecte, hem d’iniciar sessió en el MySQL:

*SET default_storage_engine=InnoDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161745376-ff542467-652e-4950-b354-2641a4a76367.png)

- Comprovació:

*SHOW STORAGES ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161745428-3761cbf9-e3cd-4a27-bdf7-e052d4ed4348.png)

- També l’altre manera es anar al fitxer my,cnf i posarem el InnoDB amb la següent línia de codi, guardem i reiniciem el percona MySQL:

***[mysqld]***

*default-storage-engine = InnoDB*

![imagen](https://user-images.githubusercontent.com/61557739/161745469-0ea21524-89ee-4045-a940-5bb1ebbff7fe.png)

*systemctl restart mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161745523-90899c0c-5cb9-4182-a1b7-06a2f6a15cc0.png)

- Comprovació:

*SHOW STORAGES ENGINES;*

![imagen](https://user-images.githubusercontent.com/61557739/161745592-adddbbed-b073-4303-b71b-a1a5a9157b83.png)

***

### A partir de MySQL apareixen els schemas de metadades i informació guardats amb InnoDB. 
### Busca informació d'aquests schemas. Indica quin és l'objectiu de cadascun d'ells i posa'n un exemple d'ús.

***

- Iniciarem sessió en el MySQL i utilitzarem la següent comanda per mostrar els schemas de metadades del InnoDB. 
- El schemas importants estan marcats en la següent imatge:

*SHOW TABLES FROM INFORMATION_SCHEMA LIKE 'INNODB%';*

![imagen](https://user-images.githubusercontent.com/61557739/161745747-ff108e27-803c-4dfc-af64-8034f4a6a148.png)


- ***INNODB_TABLES:*** Proporciona metadates a les taules InnoDB, es equivalent a la informació en el SYS_TABLES

- Exemple d’us del INNODB_TABLES en el MySQL:

*SELECT * FROM information_schema.INNODB_TABLES LIMIT 2\G*

![imagen](https://user-images.githubusercontent.com/61557739/161745874-844effe9-3dee-42a7-a66f-9ef65b323a2b.png)


- ***INNODB_COLUMNS:*** Proporciona metadates a les columnes de la taula InnoDB, es equivalent a la informació en el SYS_COLUMNS

- Exemple d’us del INNODB_COLUMNS en el MySQL:

*SELECT * FROM information_schema.INNODB_COLUMNS LIMIT 4\G*


![imagen](https://user-images.githubusercontent.com/61557739/161745954-5b5f1eef-9204-453f-961f-65c5e3462828.png)

- ***INNODB_INDEXES:*** Proporciona metadates al indexs del InnoDB, es equivalent a la informació en el SYS_INDEXES

- Exemple d’us del INNODB_INDEXES en el MySQL:

*SELECT * FROM information_schema.INNODB_INDEXES \G*

![imagen](https://user-images.githubusercontent.com/61557739/161746149-20520ce9-3a78-4d74-a98c-c152a6deb1c2.png)


- ***INNODB_FIELDS:*** Proporciona metadates en les columnes clau dels índexs de InnoDB, es equivalent a la informació en el SYS_FIELDS.

- Exemple d’us del INNODB_FIELDS en el MySQL:

*SELECT * FROM information_schema.INNODB_FIELDS \G*

![imagen](https://user-images.githubusercontent.com/61557739/161746262-145eb77d-ef70-47e8-b234-e0c782365e95.png)


- ***INNODB_TABLESTATS:*** Dona una vista de la informació del estat de baix nivell a les taules de InnoDB, son taules que deriven d’estructures de dades en memòria.

- Exemple d’us del INNODB_TABLESTATS en el MySQL:

No he trobat comanda, però un exemple d’us seria que es pot utilitzar per desenvolupar noves extensions relacionades amb el rendiment o monitoritzar el rendiment d’alt nivell.


***



- ***INNODB_DATAFILES:*** Proporciona informació a la ruta del arxiu de dades per els arxius InnoDB per taula i tablespaces generals, es equivalent a la informacio del SYS_DATAFILES.

- Exemple d’us del INNODB_DATAFILES en el MySQL:

No hem funciona a mi la comanda, però la comanda seria la següent:

*SELECT * FROM INNODB_DATAFILES;*

***
- ***INNODB_TABLESPACES:*** Proporciona metadades als arxius per taula i tablespaces del InnoDB, es equivalent a la informació del SYS_TABLESPACES

- Exemple d’us del INNODB_TABLESPACES en el MySQL:

*DESC information_schema.innodb_tablespaces;*

![imagen](https://user-images.githubusercontent.com/61557739/161746617-2bc3bd9c-8212-4564-a5b0-470248200e21.png)


*SELECT * FROM information_schema.INNODB_TABLESPACES \G*

![imagen](https://user-images.githubusercontent.com/61557739/161746677-d118cb69-30d6-4d10-80af-603019ba368e.png)


- ***INNODB_FOREIGN:*** Proporciona metadades de claus foraneas definides en les taules InnoDB, es equivalent a la informació del SYS_FOREIGN.

- Exemple d’us del INNODB_FOREIGN en el MySQL:

No hem funciona a mi la comanda, però la comanda seria la següent:

*SELECT * FROM INNODB_FOREIGN \G*

***
- ***INNODB_FOREIGN_COLS:*** Proporciona metadades de les columnes de claus foraneas definides en les taules InnoDB, es equivalent a la informació del SYS_FOREIGN_COLS.

No he trobat comanda per aquest


***

### Posa un exemple que produeix un DEADLOCK i mostra-ho al professor.

***

***MOLT IMPORTANT:***
- Perquè el DEADLOCK funcioni correctament hem de tindre tots els usuaris MySQL connectats a l’hora.
- Jo en el meu cas he utilitzat Putty per poder fer-ho, tenint un Putty amb el usuari root MySQL i un altre Putty amb l’usuari pastanaga MySQL. 
- En total amb 2 sessions connectades a l’hora amb el PuTTY.

***

- Imatge de comprovació de les 2 finestres Putty amb els usuaris SQL connectats a l’hora per poder fer el exercici:

![imagen](https://user-images.githubusercontent.com/61557739/161748630-844c51a9-713f-4ed6-ab8a-0dbeb6bcbe72.png)

***

- Primer iniciarem sessió en el MySQL amb l’usuari root i crearem una base de dades per fer aquest exercici:

*CREATE DATABASE Exercici_Deadlock;*

![imagen](https://user-images.githubusercontent.com/61557739/161747162-468b9863-f251-46ff-99ab-869417160420.png)


- Utilitzarem la base de dades Exercici_Deadlock:

*USE Exercici_Deadlock*

![imagen](https://user-images.githubusercontent.com/61557739/161747260-5a92eadf-3a28-4de4-ba26-aadfacf5e3e2.png)

- Després crearem una petita taula per fer aquest exercici:

*CREATE TABLE Alumnes (
    Nom INT
)
ENGINE = INNODB;*

![imagen](https://user-images.githubusercontent.com/61557739/161747403-9cce81e1-808b-405b-8d79-b7cf39335a5a.png)


- Després l’inserim un valor a la taula:

*INSERT INTO Alumnes (Nom)
VALUES (1);*

![imagen](https://user-images.githubusercontent.com/61557739/161747492-92b5873d-ba4d-4eb7-9b26-fc11714747e0.png)

- A continuació utilitzarem la següent comanda per començar la Transacció:

*START TRANSACTION;*

![imagen](https://user-images.githubusercontent.com/61557739/161747547-050f4e84-2ba7-4cf5-b5bd-75ffb6552075.png)


- Després utilizarem la següent comanda per fer un select i veure el contingut en lock in share mode, per el deadlock:

*SELECT * FROM Alumnes WHERE Nom = 1 LOCK IN SHARE MODE;*


![imagen](https://user-images.githubusercontent.com/61557739/161747580-619081a7-e61d-439d-9331-de486d252d48.png)

- Després crearem 1 usuari, per poder fer l’exercici. 
- Utilitzarem 2 usuaris diferents, 1 que el crearem anomenat Pastanaga i l’altre l’usuari root per veure que funciona el DEADLOCK. 
- Crearem l’usuari pastanaga amb la següent comanda:

*CREATE USER 'Pastanaga'@'localhost' IDENTIFIED BY 'patata';*

![imagen](https://user-images.githubusercontent.com/61557739/161747676-aa007637-bf08-4b5a-aa7f-c0ea012fb70e.png)


- Després li donarem tots els permisos al usuari Pastanaga amb la següent comanda:

GRANT ALL PRIVILEGES ON `*.*` TO 'Pastanaga'@'localhost';

![imagen](https://user-images.githubusercontent.com/61557739/161747716-95096bfc-1e45-49ad-a525-36866e917956.png)


***Important deixarem la connexió del usuari root MySQL oberta, obrirem una altre finestra de Putty per fer la connexió del usuari Pastanaga.***

- Iniciarem a continuació sessió amb el usuari Pastanaga:

*mysql -uPastanaga -ppatata*

![imagen](https://user-images.githubusercontent.com/61557739/161747814-1c25b264-04f3-4284-8e79-a07c039a777f.png)


- Utilitzarem la base de dades Exercici_Deadlock;

![imagen](https://user-images.githubusercontent.com/61557739/161747848-0518c9df-2082-4e27-8b83-6e1f7e39b24e.png)

- A continuació utilitzarem la següent comanda amb el usuari Pastanaga, per començar la transacció i eliminar les dades de la taula:

*START TRANSACTION;*

*DELETE FROM Alumnes WHERE Nom = 1;*

![imagen](https://user-images.githubusercontent.com/61557739/161747904-da402387-57bd-48d1-b816-9107c1756c26.png)

- Després anirem al ususari root que tenim obert en l’altre finestra del Putty
- Recordem que tenim que mantenir les 2 sessions MySQL obertes perquè funcioni l’exercici.

- Utilitzarem la següent comanda amb l’usuari root, per eliminar les dades de la taula:

*DELETE FROM Alumnes WHERE Nom = 1;*


![imagen](https://user-images.githubusercontent.com/61557739/161748042-282477f0-89d8-4da8-9d16-c3074c8f30d0.png)


- Com podem observar en l’usuari Pastanaga no pot eliminar perquè el Deadlock lo impedeix:

![imagen](https://user-images.githubusercontent.com/61557739/161748083-cacfa6f6-f9a2-4e58-9706-ecb08714f9f1.png)


***

***Comprovació veient les 2 finestres:***

![imagen](https://user-images.githubusercontent.com/61557739/161749020-3f877f0d-0e8f-4e22-b6f1-3b4a34f27245.png)

***

*Ja estaria*

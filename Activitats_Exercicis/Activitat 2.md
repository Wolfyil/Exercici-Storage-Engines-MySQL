## 2. INNODB part I. REALITZA ELS SEGÜENTS APARTATS (obligatòria)  

### Desactiva l’opció que ve per defecte de innodb_file_per_table

***

- Utilitzarem la següent comanda per comprovar si esta activada l’opció de innodb_file_per_table. 
- Hem d’iniciar sessió en el MySQL:

*SHOW VARIABLES LIKE "innodb_file_per_table";*

![imagen](https://user-images.githubusercontent.com/61557739/161803668-56aa81d5-70a7-4366-bcea-dde43ab1e840.png)

- Ara desactivem l’opció per comanda MySQL que ve per defecte de innodb_file_per_table:

*SET GLOBAL innodb_file_per_table=OFF;*

![imagen](https://user-images.githubusercontent.com/61557739/161803821-a8855902-b3ec-482d-b2db-28fc05c1e3ab.png)

- També es pot anar al fitxer /etc/my.cnf i afegir la línia innodb_file_per_table=OFF per desactivar l’opció:

*innodb_file_per_table=OFF*

![imagen](https://user-images.githubusercontent.com/61557739/161803935-71c6d39d-aa94-4f53-a5bf-20d679ed6ae5.png)

- Comprovació que esta desactivada l’opció:

*SHOW VARIABLES LIKE "innodb_file_per_table";*

![imagen](https://user-images.githubusercontent.com/61557739/161804013-a1c2ea98-d5e5-4a2b-990c-653865275c94.png)

***

### Importa la BD Sakila com a taules InnoDB. 

***

- Utilitzarem la següent comanda:

***Però no funciona perquè el MyISAM no funciona i dona problemes de foreign key.***
***també per defecte a l’hora d’importar la BD Sakila el storage engine es InnoDB i les taules son de tipus InnoDB.***

*SELECT CONCAT('ALTER TABLE ',TABLE_NAME, ' ENGINE=InnoDB;')  AS ComandaSQL
FROM Information_schema.TABLES WHERE TABLE_SCHEMA = 'sakila' AND ENGINE = 'MyISAM' AND TABLE_TYPE = 'BASE TABLE' ORDER BY TABLE_NAME ASC;*


- Eliminarem la base de dades Sakila:

*DROP DATBASE sakila;*

![imagen](https://user-images.githubusercontent.com/61557739/161804478-f9e4f085-8c6e-47f1-8715-a6c4cc0e5513.png)

- Després la tornarem a importar perquè s’apliquin els canvis de innodb_file_per_table=OFF

![imagen](https://user-images.githubusercontent.com/61557739/161804543-beb7bae9-f1bf-40b4-8385-a98322d60b91.png)

- Com podem veure no ens surt fitxers innodb_file_per_table, perquè els vam posar en OFF:

*SOURCE /root/sakila-schema.sql*

![imagen](https://user-images.githubusercontent.com/61557739/161804600-631e031c-d767-474d-9bb2-f29e6e052c86.png)

- Després per fer el canvi del storage engine de MyISAM a InnoDB hem de fer la següent comanda per totes les taules.
- ***Però igualment les taules ja venen amb el InnoDB engine. Ja que el MyISAM no funciona i per defecte les taules ja tenen InnoDB:***

*ALTER TABLE actor ENGINE = InnoDB;*

![imagen](https://user-images.githubusercontent.com/61557739/161805093-e6a0e184-bcd1-48ac-bd37-10bf4a634c6b.png)

- Comprovació de Storage Engine de la BD sakila importada, en el MySQL:

*SELECT TABLE_NAME, ENGINE FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'sakila';*

![imagen](https://user-images.githubusercontent.com/61557739/161805263-d35b4d16-2f75-4761-be39-7e602a8d7f69.png)


***

### Quin/quins són els fitxers de dades? A on es troben i quin és la seva mida? 

***

- Utilitzarem la següent comanda per veure els fitxers de dades i on es troben:

*SELECT * FROM INFORMATION_SCHEMA.INNODB_DATAFILES;*

![imagen](https://user-images.githubusercontent.com/61557739/161805547-d56aa387-0956-4f72-b051-8a2ca4d45707.png)

*SELECT NAME, FILE_SIZE FROM INFORMATION_SCHEMA.INNODB_TABLESPACES;*

![imagen](https://user-images.githubusercontent.com/61557739/161805606-2d6483a4-fd2a-4cd6-baa5-06ea973e1020.png)

- Els fitxers on es troben es en la ruta /var/lib/mysql/:

![imagen](https://user-images.githubusercontent.com/61557739/161805736-41c01bee-0182-49cc-a04c-b37b2b008069.png)

***

### Canvia la configuració del MySQL per:
### Canviar la localització dels fitxers del tablespace de sistema per defecte a /discs-mysql/


***

- Primer crearem el directori /discs-mysql:

*mkdir /discs-mysql*

*chown -R mysql:mysql /discs-mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161806106-ba445f4a-2a85-42cd-aaa2-333bbb00aa73.png)

![imagen](https://user-images.githubusercontent.com/61557739/161806124-a129d520-fb31-47d4-9d51-643425c45185.png)

- Comprovem iniciant sessió en el MySQL quina es la localització dels fit-xers del tablespace per defecte amb la següent comanda:

*SELECT @@datadir;*

![imagen](https://user-images.githubusercontent.com/61557739/161806302-41d9e208-b2e6-4b3e-85d4-77a5531f7da2.png)


- Després sortirem del MySQL i pararem el servei Percona Mysql per evitar informació corrupta i per evitar possibles problemes:

*systemctl stop mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161806486-07859810-10d0-4fbf-8472-affb3cb227a4.png)

- Després utilitzarem la següent comanda per copiar recursivament els con-tinguts del /var/lib/mysql a /discs-mysql conservant els permisos origi-nals.:

cp -R -p /var/lib/mysql/* /discs-mysql

![imagen](https://user-images.githubusercontent.com/61557739/161806688-5279c49a-1474-4bd6-aa33-0253ddb35693.png)

- Després de fer les anterior comandes, anirem al fitxer /etc/my.cnf i afegirem les següents línies en la ruta del datadir i el socket.
- Posant la ruta de la carpeta que vam crear /discs-mysql. Després d’afegir les següents línies guardem i sortim del fitxer.:

***[mysqld]***

*datadir=/discs-mysql*

*socket=/discs-mysql/mysql-sock*

***[client]***

*port=3306*

*socket=/discs-mysql/mysql.sock*


![imagen](https://user-images.githubusercontent.com/61557739/161806928-31bd6a32-d3fa-4ac5-a591-fd6391043da0.png)

- Instal·larem el semanage per el SELinux amb les següents comandes:

yum provides *bin/semanage

![imagen](https://user-images.githubusercontent.com/61557739/161807053-3ce7dd0b-35ca-443e-b8f6-43b77396ff3a.png)

*yum -y install policycoreutils-python*

![imagen](https://user-images.githubusercontent.com/61557739/161807262-1c689e47-5bf7-48e5-80c9-2d2eb283c4bc.png)

- Finalment utilitzarem les següents comandes per afegir la seguretat SELinux a /discs-mysql abans de reiniciar el servei Percona MySQL. 
***Es important fer això perquè sinó donara errors per el SELinux.:***

*semanage fcontext -a -t mysqld_db_t "/discs-mysql(/.*)?"*

*restorecon -R /discs-mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161807650-902ed5e2-cb47-4704-97f0-9e3a6ae1141d.png)

- Després tornem a iniciar el MySQL per veure si funciona els nostres canvis amb la següent comanda:

*service mysqld start*

![imagen](https://user-images.githubusercontent.com/61557739/161807785-83d48dd2-11ad-4f2f-9b0d-ebc1ff74cb99.png)

- Iniciem sessió en el MySQL per comprovar que hem canviat la ruta dels tablespaces amb la següent comanda:

*SELECT @@datadir;*

![imagen](https://user-images.githubusercontent.com/61557739/161807986-78c1166d-d405-4657-aeea-094ee7bd4584.png)


***

### Tinguem dos fitxers corresponents al tablespace de sistema.
### Tots dos han de tenir la mateixa mida inicial (10MB) 
### El tablespace ha de créixer de 5MB en 5MB.


***

***MOLT IMPORTANT: Jo estic utilitzant un Percona Server for MySQL amb la versió 8.x, els exercicis de crear fitxers ibdata no hem funciona.*** 
***Ja que en la versió 8.x no es regenera el fitxer ibdata quan el eliminem, però en la versió 5.x si funciona.***
***Jo he fet els exercicis com si hem funcionessin utilitzant comandes per tal de que se demostrar com es realitza els exercicis amb els fitxers ibdata***

- Aquí deixo una captura de pantalla de justificació de perquè no funciona amb la versió 8.x i també deixaré la URL:

*https://knowledge.broadcom.com/external/article/209019/mysql-8-cannot-startup-after-remove-ib-f.html*

![imagen](https://user-images.githubusercontent.com/61557739/161808861-eb4f7099-2921-4009-8b73-2b68033e82b1.png)

***

- Primer de tot parem el servei Percona MySQL amb la següent comanda:

*systemctl stop mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161809000-9408425b-4732-4873-8f7a-0cb532d69815.png)

- Despres eliminem tots els fitxer ib:

*rm /discs-mysql/ibdata1*

*rm /discs-mysql/ib_logfile**

![imagen](https://user-images.githubusercontent.com/61557739/161809101-956d7e36-7da1-44fd-972c-a7bf9b30fe64.png)


- A continuació afegirem les següents dos línies de codi innodb per augmentar de 5 en 5MB i que tinguin una mida inicial de 10MB. 
- Això ho farem en el fitxer /etc/my.cnf. Nosaltres per defecte tenim el fitxer ibdata1: 

- Anirem al fitxer /etc/my.cnf per fer la configuracio i posar l’altre fitxer ibdata2, posarem les següents linees:

***[mysqld]***

*innodb_autoextend_increment=5M*

*innodb_data_file_path=ibdata1:10M;ibdata2:10M:autoextend*

![imagen](https://user-images.githubusercontent.com/61557739/161809253-eff2172d-5752-4824-be39-c1fb6d9dd193.png)

- Després tornem a iniciar el Servei Percona MySQL amb la següent comanda:

*service mysqld start*

![imagen](https://user-images.githubusercontent.com/61557739/161809339-6a8de0dd-f991-4735-b681-d0b6ec5dc856.png)

***

### Situa aquests fitxers (de manera relativa a la localització per defecte) en una nova localització simulant el següent:
### /discs-mysql/disk1/primer fitxer de dades → simularà un disc dur.
### /discs-mysql/disk2/segon fitxer de dades → simularà un segon disc dur.

***

- Crearem els dos directoris /discs-mysql/disk1 i el /discs-mysql/disk2. També farem que el mysql sigui propietari d’aquests directoris:

*mkdir /discs-mysql/disk1*

*mkdir /discs-mysql/disk2*

![imagen](https://user-images.githubusercontent.com/61557739/161809610-c22fb75c-b917-4326-bdde-48ea047530d2.png)

*chown -R mysql:mysql /discs-mysql/disk1*

*chown -R mysql:mysql /discs-mysql/disk2*

![imagen](https://user-images.githubusercontent.com/61557739/161809656-9f208a71-41bf-4bb9-9013-369bb672ae4d.png)

- Després sortirem del MySQL i pararem el servei Percona Mysql per evitar possibles problemes:

*systemctl stop mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161809704-820510f6-8834-458b-9498-3212fe53bfca.png)

- Despres eliminem tots els fitxer ib:

*rm /discs-mysql/ibdata1*

*rm /discs-mysql/ibdata2*

*rm /discs-mysql/ib_logfile**


- Després anirem al fitxer /etc/my.cnf i afegirem les següents línies:

***[mysqld]***

*innodb_autoextend_increment=5M*

*innodb_data_home_dir =*

*innodb_data_file_path=/disk1/ibdata1:10M;/disk2/ibdata2:10M:autoextend*

![imagen](https://user-images.githubusercontent.com/61557739/161809822-0f9b9bd2-d298-4db9-9166-f302b76e0488.png)

- Després tornem a iniciar el MySQL amb la següent comanda:

*service mysqld start*

![imagen](https://user-images.githubusercontent.com/61557739/161809946-0869de51-5e75-4d79-abff-8a72b78c00f0.png)

***

*Ja estaria*

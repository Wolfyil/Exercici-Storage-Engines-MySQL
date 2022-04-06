## 3. INNODB part II. REALITZA ELS SEGÜENTS APARTATS (obligatòria)  

### Partint de l'esquema anterior configura el Percona Server perquè cada taula generi el seu propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vulgueu).
### Indica quins són els canvis de configuració que has realitzat.

***

- Iniciem sessió en el mysql per comprovar que el innodb_file_per_table esta desactivat per l’exercici 2:

*SHOW VARIABLES LIKE "innodb_file_per_table";*

![imagen](https://user-images.githubusercontent.com/61557739/161814497-736068ff-ea61-49a1-a223-e73cc2bf0d1e.png)

***El nostre objectiu es activar aquesta funció i que les taules d’una base de dades tinguin els seus propis tablespaces***

- Primer crearem el directori /tspaces:

*mkdir /tspaces*

*chown -R mysql:mysql /tspaces*

![imagen](https://user-images.githubusercontent.com/61557739/161814600-04678db8-1838-4c54-8e63-1abfa0718a8b.png)

![imagen](https://user-images.githubusercontent.com/61557739/161814613-a578cdda-a09e-4a9c-b68a-248d12f19b0f.png)

- Després sortirem del MySQL i pararem el servei Percona Mysql per evitar informació corrupta i per evitar possibles problemes:

*systemctl stop mysql*

![imagen](https://user-images.githubusercontent.com/61557739/161814668-b312a70b-ca61-420e-93f5-e3c5e3e2387d.png)

- Després utilitzarem la següent comanda per copiar recursivament els con-tinguts del /var/lib/mysql a /discs-mysql conservant els permisos origi-nals.:

cp -R -p /var/lib/mysql/* /tspaces

![imagen](https://user-images.githubusercontent.com/61557739/161814814-7e490ca5-f8e8-4b3f-82aa-a1a8c84b0075.png)

- Després de fer les anterior comandes, anirem al fitxer /etc/my.cnf i afegirem les següents línies en la ruta del datadir i el socket.
- Posant la ruta de la carpeta que vam crear /tspaces. Després d’afegir les següents línies guardem i sortim del fitxer.:

- També editarem la següent línia en el mateix fitxer perquè cada taula tingui el seu fitxer .ibd:

***[mysqld]***

*innodb_file_per_table=ON*

**- Comentarem les línies dels fitxers ibdata dels exercicis anteriors:**

***[mysqld]***

*datadir=/tspaces*

*socket=/tspaces/mysql.sock*

*innodb_file_per_table=ON*

***[client]***

*port=3306*

*socket=/tspaces/mysql.sock*

![imagen](https://user-images.githubusercontent.com/61557739/161815054-5549f45e-cf5f-4b68-b689-db1bbab6109d.png)

- Finalment utilitzarem les següents comandes per afegir la seguretat SELinux a /discs-mysql abans de reiniciar el servei Percona MySQL. 
- Es important fer això perquè sinó donara errors per el SELinux.:

*semanage fcontext -a -t mysqld_db_t "/tspaces(/.*)?"*

*restorecon -R /tspaces*

![imagen](https://user-images.githubusercontent.com/61557739/161840745-53fe1215-5c88-491b-91c2-2fd2ba9188ea.png)

- Després tornem a iniciar el MySQL per veure si funciona els nostres canvis amb la següent comanda:

*service mysqld start*

![imagen](https://user-images.githubusercontent.com/61557739/161840863-bf719bb9-9917-41fa-922d-6bb99d27e6ed.png)

- Iniciem sessió en el MySQL per comprovar que hem canviat la ruta dels tablespaces amb la següent comanda:

*SELECT @@datadir;*

![imagen](https://user-images.githubusercontent.com/61557739/161840910-63d27b2b-4904-4bf3-9a71-b33b2e0b2f29.png)


- Comprovació de que esta activat

*SHOW VARIABLES LIKE "innodb_file_per_table";*

![imagen](https://user-images.githubusercontent.com/61557739/161841023-6b83bf30-aba7-4d1c-af3f-e8b2ed7a8081.png)

- Recordem que teníem el innodb_file_per_table en OFF i en el fitxer my.cnf el vam activar a ON. 
***Mostraré la taula sakila amb l’opcio OFF:***

![imagen](https://user-images.githubusercontent.com/61557739/161841064-c431b119-9ddb-42c3-8b72-36f63bd3c4d8.png)


***Com podem veure no hi ha res.***

- Perquè hi hagi taules amb el seu propi tablespace el que hem de fer es iniciar sessió en el MySQL i eliminar la base de dades sakila:

*DROP DATABASE sakila;*

![imagen](https://user-images.githubusercontent.com/61557739/161841198-09319db7-02e0-4e16-bdc4-ed5a9d3ac77c.png)


- Després hem de tornar a importar la base de dades sakila perquè es pugui crear els tablespaces per cada taula.
- ja que l’única manera perquè faci el canvi es eliminant i tornant a importar:

*SOURCE /root/sakila-schema.sql*

![imagen](https://user-images.githubusercontent.com/61557739/161841298-9a2a0f47-0cba-4c30-b113-5a1b8a964a1c.png)

- Ara si tornem a /tspaces/ i fem un ls, podem veure que cada taula de la base de dades sakila te el seu propi tablespace:

![imagen](https://user-images.githubusercontent.com/61557739/161841417-798e5e31-bfbe-45af-b30a-80c41284bb59.png)

***

*Ja estaria*

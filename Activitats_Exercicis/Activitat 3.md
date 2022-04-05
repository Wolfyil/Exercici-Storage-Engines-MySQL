## 3. INNODB part II. REALITZA ELS SEGÜENTS APARTATS (obligatòria)  

### Partint de l'esquema anterior configura el Percona Server perquè cada taula generi el seu propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vulgueu).
### Indica quins són els canvis de configuració que has realitzat.

***

- Iniciem sessió en el mysql per comprovar que el innodb_file_per_table esta desactivat per l’exercici 2:

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

*innodb_file_per_table=ON*

- Comentarem les línies dels fitxers ibdata dels exercicis anteriors:



![imagen](https://user-images.githubusercontent.com/61557739/161815054-5549f45e-cf5f-4b68-b689-db1bbab6109d.png)




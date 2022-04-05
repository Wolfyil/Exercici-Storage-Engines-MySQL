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


*Ja estaria*

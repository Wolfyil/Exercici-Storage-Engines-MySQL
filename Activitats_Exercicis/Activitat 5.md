## REDOLOG. REALITZA ELS SEGÜENTS APARTATS.

### Com podem comprovar (Innodb Log Checkpointing):

***

- Podem fer les comprovacions amb la següent comanda una vegada iniciat sessió en el MySQL:

*SHOW ENGINE INNODB STATUS;*

**Una vegada executada la comanda anterior ens sortirà diferents informacions com podem veure en pantalla:**

![imagen](https://user-images.githubusercontent.com/61557739/161845943-c6821a74-80e1-411e-bf51-7b6c66f868a2.png)


- El Innodb Log Checkpointing es troba mes avall com es mostra en la següent imatge, en el apartat de Log:

![imagen](https://user-images.githubusercontent.com/61557739/161846000-4155ae56-e355-4e51-bae3-34b2efbb9694.png)


***

### LSN (Log Sequence Number)

***

- El LSN es troba en el mateix apartat del Log:

![imagen](https://user-images.githubusercontent.com/61557739/161846096-d65fb98e-9fd3-4ffa-9fba-92a09fd60aa4.png)

***

### L'últim LSN actualitzat a disc

***

![imagen](https://user-images.githubusercontent.com/61557739/161846146-3cddb63e-44a9-45a0-ae90-72ec734fccd5.png)

***

### Quin és l'últim LSN que se li ha fet Checkpoint

***

![imagen](https://user-images.githubusercontent.com/61557739/161846233-25e6157e-7bfa-4dfe-9b24-50b23e57d6d0.png)


***

### Proposa un exemple a on es vegi l'ús del redolog

***

- El redolog son fitxers que registren canvis en les bases de dades com resultat de transaccions o accions internes. 
- Serveixen per protegir les bases de dades de les pèrdues d’integritat en casos de falles causats per exemple, per subministrament elèctric o errors de discs durs.

- Aquests fitxers treballen de manera cíclica, si un arxiu redo log s’emplena passarà al següent arxiu en el que fa un checkpoint, aquesta informació s’enmagatzema en el arxiu de control.

**Els seus exemple d’us son:**

- ***Millorar el rendiment d’emmagatzematge de dades:*** Els logs es converteixen en escriptures seqüencials i nomes guarden els canvis per tenir una mida mes petita. 
Així la base de dades es mes ràpid i eficient guardant els canvis de dades de forma seqüencial en un *iblog:_file*.

- ***La recuperació d’accidents:*** Els fitxers redo log al tornar arrencar el servidor InnoDB detecta que hem tingut una caiguda i farà un crash recovery per tal de recuperar la informació 


***

### Com podem mirar el número de pàgines modificades (dirty pages)? I el número total de pàgines?

***

- Utilitzant la comanda anterior:

*SHOW ENGINE INNODB STATUS;*

- Si anem cap avall hi ha un apartat anomenat *Buffer Pool and Memory*, aquí podem trobar el numero de pagines modificades i el numero total de pagines:

**Numero de pagines modificades:**

![imagen](https://user-images.githubusercontent.com/61557739/161846912-1fd28993-6440-426a-a7a2-1feb0e1d85fa.png)

**Numero total de pagines:**

![imagen](https://user-images.githubusercontent.com/61557739/161846977-9a4aa9bd-e8d5-4caf-bc5f-fb0d471d0ef3.png)

***

*Ja estaria*

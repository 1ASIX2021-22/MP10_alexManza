<div id="top"></div>

<!-- PROJECT LOGO -->
<br />
<div align="center">
 
  <h3 align="center">M10 - IESEBRE | ASIX DAM 1r</h3>

  <p align="center">
    Alejandro Manzanera Ramon
  </p>
</div>

<!-- ABOUT THE PROJECT -->
## Tasca

L'objectiu de la tasca és instal·lar el sistema gestor de base de dades MySQL i ajustar-lo per tal que funcioni amb una aplicació/plantilla que us proporciona el professor. Cal documentar tot el procediment en un fitxer Markdown (extensió .md) anomenat MYSQL.md que posareu a l'arrel del vostre repositori Github.

### Instal·lació de mysql-server

Per començar, destaco que estic treballant en una màquina d'Ubuntu neta amb php7.4, composer i nodejs / npm acabats d'instal·lar.

El primer pas que realitzarem serà instal·lar a la màquina mysql-server per tindre el servidor mysql funcionant sobre aquesta.

```
Instal·lem mysql-server amb el gestor de paquets de ubuntu (apt).
```

<img src="https://user-images.githubusercontent.com/91521595/173458283-6824b4b4-d3c5-4b18-a9d6-c8727312dcea.PNG">

En instal·lar mysql-server, també ens instal·larà el client. Un cop ha acabat utilitzem la comanda mysql al terminal i revisem el resultat.

<img src="https://user-images.githubusercontent.com/91521595/173458461-a5efd3f9-09bf-434a-b8e6-99477e3d281e.PNG" >

Podem veure que ens retorna un missatge que ens notifica que se'ns denega l'accés per a l'usuari ismasg sobre localhost.

Això és perquè a l'executar aquesta comanda sense cap paràmetre, intenta accedir al servidor mysql (localhost) amb l'usuari sobre el qual estem connectats al bash. Ens denega l'accés perquè no hi ha cap usuari al servidor mysql amb aquest nom.

Hi han diverses formes de verificar que el servei està funcionant i sobre quin port. De sèrie ja sabem que mysql funciona normalment sobre el port 3306, ho verificarem amb nmap.

Instal·lem nmap a la màquina i posteriorment realitzem un escaneig sense cap paràmetre sobre aquesta.

<img src="https://user-images.githubusercontent.com/91521595/173458504-d7b1e835-caf4-45e5-a7ad-bfda6fc55aff.PNG" >

Podem veure com efectivament tenim el servei mysql corrent sobre el port 3306.

Ara que hem vist que tenim el servei funcionant, volem accedir amb el client al servidor. Per fer-ho accedirem amb l'usuari per defecte que es genera durant la instal·lació.

Podem veure l'arxiu de configuració amb les credencials utilitzant la següent comanda:

```
cat /etc/mysql/debian.cnf 
```

<img src="https://user-images.githubusercontent.com/91521595/173458672-29a2acf5-56d6-4f34-a6d6-74e33811905d.PNG">

Copiem les credencials i ara sí que sí que procedim a iniciar sessió al servidor MySQL.

Des de el terminal utilitzarem la comanda mysql, amb el paràmetre -p perquè ens demani contrasenya (si no intentarà accedir sense password) i -u amb el nom d'usuari que podem veure a l'arxiu mencionat.

```
mysql -p -u debian-sys-maint
```

Un cop hem ficat la contrasenya, podem veure com s'ens ha obert el prompt del client de mysql.

<img src="https://user-images.githubusercontent.com/91521595/173458694-6965aecf-cbb3-4db9-bfa4-135cf82645b0.PNG">

### MySQL Client

Procedirem a fer proves sobre el servidor de MySQL que hem instal·lat des de el client al qual hem accedit.

Podem veure les bases de dades existents amb la següent comanda:

```
show databases;
```

<img src="https://user-images.githubusercontent.com/91521595/173459506-97833525-0a8f-426b-8aa7-f4a1987419fc.PNG" >

Per a la tasca, crearem una base de dades anomenada 'tasks'. Per fer-ho, utilitzem la següent comanda:

```
CREATE DATABASE tasks;
```

Veurem que ens retorna un missatge informant de que la query s'ha executat correctament. Podem llistar posteriorment les bases de dades un altre cop i veurem que apareix la que acabem de crear.

<img src="https://user-images.githubusercontent.com/91521595/173459523-f7aca7e8-80aa-4da3-839b-33cf1423468a.PNG" >

Accedirem a la base de dades i posteriorment crearem una taula.

Per accedir a la base de dades utilitzarem la següent comanda:

```
use tasks;
```

Ens retorna un missatge informant que la base de dades seleccionada ha canviat.

Crearem una taula amb un camp de tipus text i un altre de boolean amb nom tasks. No detallaré la estructura, ja que en aquesta pràctica no ens centrem en la base de dades sinó com ferla funcionar.

Crearem la taula amb la següent comanda:

```
CREATE TABLE tasks (descripion text, completed boolean);
```

Ens retornarà el missatge que ja hem vist abans informant que la query s'ha executat correctament.

Podem veure les taules existents a la base de dades amb la següent comanda:

```
SHOW tables;
```

<img src="https://user-images.githubusercontent.com/91521595/173459565-2cdae940-73e7-442b-b42f-44c8a2cb6770.PNG" >

### MySQL Workbench

Ara procedirem a instal·lar MySQL Workbench.

Per començar, accedim a la pàgina oficial de MySQL i a l'apartat de descarregues del Workbench, seleccionem el sistema operatiu i la versió.

Ens descarreguem aquest (arxiu amb extensió .deb)

<img src="https://user-images.githubusercontent.com/91521595/173459586-66bcd4de-ef1f-45fa-9f98-9ba596510c29.PNG" >

Obrim el terminal a la carpeta de descarregues i procedim a instal·lar aquesta eina amb dpkg.

```
dpkg -i "nom del arxiu.deb"
```

<img src="https://user-images.githubusercontent.com/91521595/173459611-1ec9721b-6bf3-48dd-a7c8-c1067ad4747d.PNG" >

El motiu per el qual no ens deixa instal·lar MySQL Workbench és perquè ens falten dependències que requereix l'eina, ho solucionarem instal·lant amb apt el paquet de mysql-workbench-community.

Ens torna a donar errors perquè aquest paquet necessita altres dependències. Ho podem solucionar utilitzant la següent comanda:

```
apt --fix-broken install
```

<img src="https://user-images.githubusercontent.com/91521595/173459633-6feb97d4-85ef-455c-a2e5-994b74970df3.PNG" >

Un cop fet això, si busquem als programes de ubuntu "Workbench" veurem que tenim aquest instal·lat i el podem executar des de allí mateix.

<img src="https://user-images.githubusercontent.com/91521595/173459919-b6fc5c72-6d36-4d50-9b68-dd6d72099330.PNG" >

### Preparem l'entorn (MySQL Workbench)

Un cop hem obert MySQL Workbench, veurem que ens surt una instància per poder accedir a localhost amb usuari root. El problema és que les credencials no són correctes i, per tant, no podrem accedir.

<img src="https://user-images.githubusercontent.com/91521595/173459741-22c555d9-5c4e-429b-9678-3733bdaf7711.png" >

Per solucionar aquest error, podem generar les credencials o bé preparar una instància amb les credencials que actualment sí que són les correctes.

Optarem per aquesta segona opció.

Modificarem les credencials de la instància ja existent. Per fer-ho, simplement fem click dret a la instància i veurem el botó amb l'opció de "Edit connection".

<img src="https://user-images.githubusercontent.com/91521595/173460078-8edc8bfa-af47-4371-9db2-4124c52b3241.png" >

S'ens obrirà una pestanya amb la informació de la instància.

Modifiquem les dades de l'usuari amb les credencials que hem vist que ens venien per defecte a l'instal·lar mysql-server al principi de l'activitat.

<img src="https://user-images.githubusercontent.com/91521595/173460119-2115a419-7ba4-4622-85b3-9f8bfdcbc616.png" >

Tanquem la pestanya que hem obert per editar la instància i ara fem doble-click esquerre a aquesta.

<img src="https://user-images.githubusercontent.com/91521595/173460185-c3be58c2-907b-4aae-b739-ee022b71671d.png" >

Podrem veure que hem accedit a la base de dades desde mysql-workbench.

Entre els diferents panells visibles, al central podem escriure querys que s'executaran al servidor mysql. Executaré un query de prova:

<img src="https://user-images.githubusercontent.com/91521595/173460222-6c4df930-33e6-4795-bb9e-d7627c85c478.png" >

### phpStorm

El IDE que utilitzarem per a aquesta activitat és el phpStorm. No explicaré en detall com instal·lar aquest.

Executo phpStorm a la màquina i creo un projecte anomenat MP10.

<img src="https://user-images.githubusercontent.com/91521595/173460278-a9921484-4f49-42b2-80c8-4605e93a0310.png">

Ara, configurarem MySQL sobre el projecte que just hem creat.

Per fer-ho, obrim el panell de bases de dades i afegim una base de dades fent click al símbol "+" que apareix al que se'ns ha obert.

Al desplegable fem click a "MySQL".

<img src="https://user-images.githubusercontent.com/91521595/173460330-8d86c37a-3320-4064-9ba8-5ba3da9b4b55.png" >

S'ens obrirà la pestanya per poder configurar una base de dades de tipus MySQL al projecte.

Veurem que ens apareix un warning a la part inferior del modal que se'ns ha obert. El motiu és que ens falten controladors per poder utilitzar MySQL amb phpStorm.

<img src="https://user-images.githubusercontent.com/91521595/173460369-e2b44418-6100-4899-8e1f-ec22644b5719.png" >

Podem instal·lar els controladors necessaris executant la següent comanda des de el terminal:

```
apt-get install php7.4-mysql
```

Configurar la connexió no té molt misteri, simplement afegirem les credencials que hem estat utilitzant a la pestanya de configuració de la base de dades dintre del phpStorm.

Especifiquem que la base de dades que utilitzarem és "tasks" (la que hem creat després d'instal·lar mysql-server i accedir per terminal).

<img src="https://user-images.githubusercontent.com/91521595/173460472-9345d58e-6cbc-4247-90d0-f8e17f3ef076.png" >

Podem comprovar la connexió fent click a "Test connection".

<img src="https://user-images.githubusercontent.com/91521595/173460544-bee61a28-e3ab-4465-97c1-a28b67eee6fb.png" >


### Proves finals


Un cop s'ens ha obert phpStorm sobre el directori, creem un arxiu anomenat index.php

Dintre, afegim un echo amb un hola món escrit per a php.

<img src="https://user-images.githubusercontent.com/91521595/173460801-334cb5b6-e8e1-4c33-b2a3-f83185315531.png" >

Tornem al terminal i executem index.php desde aquest amb la seguent comanda:

```
php index.php
```

També, podem utilitzar la comanda php per iniciar un servidor i veure-ho des de un navegador amb la següent comanda:

```
php -S IP:PORT index.php
```

<img src="https://user-images.githubusercontent.com/91521595/173460864-6f00c841-e581-457c-92d9-4fdacd4c5b70.png" >





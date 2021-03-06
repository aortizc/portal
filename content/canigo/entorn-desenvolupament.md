+++
date        = "2015-11-18"
title       = "Entorn de desenvolupament"
description = "Màquina virtual amb l'ecosistema d'eines Canigó per a començar a desenvolupar"
sections    = "Canigó"
weight 		= 99
+++

### Objectius

* Facilitar la posada en marxa de l'entorn de desenvolupament, aprovisionant una màquina virtual amb tot el necessari per a començar el desenvolupament d'una aplicació Canigó.
* Simular els entorns de desplegament als CPD Generalitat, facilitant contenidors amb les mateixes versions i configuracions dels PaaS que ens trobarem als clouds.

### Pre requisits

* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://www.vagrantup.com/downloads.html)
* [Vagranfile](https://github.com/cs-canigo/dev-environment/releases/tag/1.0.1) amb la configuració de l'entorn Canigó

La creació de la VM ha estat certificada amb Vagrant 1.8. Es recomana l'ús d'aquesta versió o superior.

### Com començar?

* Descarregar i descomprimir el [zip](https://github.com/cs-canigo/dev-environment/archive/1.0.1.zip) a la carpeta que desitgem (p.e. c:/vms o /home/user/vms)

* Anar per línia de comanda a la carpeta on estigui el Vagrantfile i executem:

		vagrant up

	Amb aquesta instrucció, vagrant aixecarà una màquina virtual a Virtualbox i executarà les comandes que inclogui el Vagrantfile. El temps d'instal·lació serà llarg degut a que instal·la tot el software necessari per a desenvolupar i desplegar en els entorns de proves (es pot veure què instal·la reseguint el shell script).

* En el moment que a la màquina virtual aixecada es vegi l'escriptori, el procés ja haurà finalitzat. Podem tancar la màquina i engegar-la i aturar-la a través de VirtualBox.


### Setup inicial

* Usuari i password: canigo/canigo

* Configurar el teclat amb la configuració correcta: Menú inici > Preferències > Keyboard Input Methods > Input method > Add
* Anara a IBUS Preferences -> Input Method -> Sel·leccionar l'idioma desitjat

### Programari instal·lat

* IDE: [Spring Tool Suite] (https://spring.io/tools) (basat en Eclipse Mars) amb JDK 7 (Oracle) i els següents plugins:

	- M2Eclipse per integració amb [Apache Maven](https://maven.apache.org/)
	- CTTI Canigó per creació aplicacions Canigó 3.1 basades en arquitectura REST+HTML5/JS o JSF
	- Spring Tool Suite per facilitar el desenvolupament d'aplicacions basades en [Spring](http://spring.io/projects)
	- Subclipse per integració amb [Subversion] (https://subversion.apache.org/)
	- SonarQube per integració amb [SonarQube] (http://www.sonarqube.org/) (antic Sonar)

* Altres

	- Engine Docker i Docker Compose Tool per l'execució de contenidors Docker
	- Navegador Google Chrome
	- Client VPNC per accés a XCAT

### Demo Equipaments

Des del CS Canigó es proporciona un exemple d'aplicació Canigó 3 per ser desplegada en contenidors Docker. El stack és el següent:

* MySQL 5.6: [mysql:5.6](https://hub.docker.com/_/mysql/)

* Tomcat 7 + Oracle JDK 7: [cscanigo/tomcat7-java7](https://hub.docker.com/r/cscanigo/tomcat7-java7/)

* Apache 2.4: [cscanigo/httpd:2.4] (https://hub.docker.com/r/cscanigo/httpd/)

A continuació es descriuen les passes a seguir per la instal·lació i execució de l'aplicació demo Equipaments:

* Configurar al fitxer /etc/hosts la següent entrada:

		127.0.0.1	demos.canigo.ctti.gencat.cat

	Aquesta modificació es pot realitzar executant el editor de text "leafpad":

		$ sudo leafpad /etc/hosts

* Des de l'Eclipse descarregar el projecte demo "equipaments":

	File -> Import -> Projects from Git -> Clone URI https://github.com/cs-canigo/equipaments.git (introduïr unes credencials vàlides de Github) -> Sel·lecccionar branch "master" -> Sel·leccionar directori "/home/canigo/Documents/workspace-sts-3.7.1.RELEASE" com a Destinations -> Sel·leccionar l'opció "Import Existing Eclipse projects" -> Finish

* Construïr l'aplicació:

	Run as... -> Maven build -> Configurar Base Directory "${workspace_loc:/equipaments}", Goals "clean package"-> Executar

* Adaptar els paths "home/canigo/..." del fitxer "``<workspace_eclipse>``/``<demo_equipaments>/``src/main/docker/stacks/dev/apache-tomcat-mysql/docker-compose.yml" als paths locals en cas de no haver fer servit els noms per defecte:

	- ``<workspace_eclipse>``=/home/canigo/Documents/workspace-sts-3.7.1.RELEASE
	- ``<demo_equipaments>``=equipaments

* Executar la demo des de linia de comandes:

		$ cd <workspace_eclipse>/<demo_equipaments>/src/main/docker/stacks/dev/apache-tomcat-mysql/
		$ docker-compose up

	O bé des d'Eclipse:

	Run -> External tools -> External Tools Configurations... -> Program - New launch configuration -> Name "equipaments" -> Location: "/usr/local/bin/docker-compose", Working Directory: "${workspace_loc:/``<demo_equipaments>``/src/main/docker/stacks/dev/apache-tomcat-mysql}", Arguments: "up" -> Executar

* Accedir amb el Chrome a la URL http://demos.canigo.ctti.gencat.cat/demo-equipaments

* La forma més fàcil d'eliminar els contenidors en execució, i per tant aturar l'aplicació, és executar la següent comanda:

		$ docker rm -f $(docker ps -qa)


### Oracle VirtualBox

Si es volen afegir carpetes compartides entre la màquina host i la guest s'han de seguir les següents passes:

* Afegir el grup vboxsf a l'usuari canigo (cal ser root o fer sudo):

		$ sudo usermod -a -G vboxsf canigo

* Reiniciar la màquina o tornar logar-se.
* Afegir les carpetes desitjades a través de Dispositius -> Carpetes compartides -> Preferències de carpetes compartides... Es pot fer en "calent" i apareixen al directori "/media" dins la màquina virtual.

### Versions

#### 1.0.0 - Ubuntu 15.04 (16/11/2015)

RELEASE NOTES

* Versió inicial

#### 1.0.1 - Ubuntu 15.10 (16/3/2016)

RELEASE NOTES

* Actualització a Ubuntu 15.10

UPGRADE

En cas de voler actualitzar a la v1.0.1 des de la v1.0.0 de l'entorn de desenvolupament cal...

* Fer l'upgrade a la versió 15.10 d'Ubuntu
* Executar les següents comandes per a que segueixi apareixent el fons de pantalla corporatiu CTTI:

		$ sudo cp /usr/share/lubuntu/wallpapers/1510-lubuntu-default-wallpaper.png /usr/share/lubuntu/wallpapers/1510-lubuntu-default-wallpaper_bck.png
		$ sudo wget http://canigo.ctti.gencat.cat/devenv/fonspantalla_1280.png -O /usr/share/lubuntu/wallpapers/1510-lubuntu-default-wallpaper.png

* Revisar la configuració de teclat a Preferències -> IBus Preferences -> Input method (en cas de no estar activat l'IBus cal activar-lo a Preferències -> Language Support -> Keyboard input method system)

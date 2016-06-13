# HOL - Azure & Docker

## Introduction

Le but de ce lab est de vous permettre de déployer une application ASP.NET Core dans un conteneur Docker, sur une machine Linux hébergée sur la plateforme cloud Microsoft Azure. 
Il est entièrement réalisable depuis un post sous Linux, MacOS ou Windows.

ASP.NET Core 1.0 est une technologie qui permet de développer des applications web à l'aide du langage C# et qui a été pensé pour s'exécuter sur n'importe quelle plateforme Linux, Mac OS ou Windows.
La documentation est disponible sur la page : https://docs.asp.net/en/latest/

Pour ce lab, vous n'avez pas besoin de connaître cette technologie puisque nous vous fournissons une application d'exemple que vous allez déployer à l'aide des outils Docker sur la plateforme Microsoft Azure.

## Préparation de votre environnement de travail

Dans cette section, vous allez installer les outils nécessaire à la réalisation du lab:

- Docker et Docker Machine, qui vous permettront respectivement de créer des images de conteneurs, de déployer des conteneurs, de les gérer et aussi de créer des machines virtuelles capables d'héberger ces conteneurs
- Visual Studio Code, un outil de développement cross-platform adapté au développement d'application web
- Git, pour vous permettre de récupérer le projet d'exemple GitHub

### Mac OS

Pour la connexion à GitHub, vous pouvez utiliser les outils Git pour Mac OS (https://git-scm.com/download/mac) ou l'outil GitHub Desktop, disponible sur cette page : https://desktop.github.com/ 

Les outils Docker sont disponibles au travers de la Docker Toolbox qui est disponible en téléchargement sur cette page : https://www.docker.com/products/docker-toolbox.
Toutes les étapes à effectuer pour installer cet outil sont décrites sur cette page : https://docs.docker.com/mac/step_one/

Une fois les outils Docker installés, téléchargez la version Mac OS de Visual Studio Code depuis https://go.microsoft.com/fwlink/?LinkID=534106.
Dézippez l'archive puis faites un "Drag & Drop" de Visual Studio Code.app dans votre dossier application et éventuellement, ajoutez-le à votre Docker pour le retrouver facilement.

Lancez Visual Studio Code pour valider que tout est OK.
Vous êtes prêt pour démarrer le lab !


### Windows

Pour la connexion à GitHub, vous pouvez utiliser les outils Git pour Mac (https://git-scm.com/download/windows) pour Mac OS ou l'outil GitHub Desktop, disponible sur cette page : https://desktop.github.com/ 

Les outils Docker sont disponibles au travers de la Docker Toolbox qui est disponible en téléchargement sur cette page : https://www.docker.com/products/docker-toolbox.
Toutes les étapes à effectuer pour installer cet outil sont décrites sur cette page : https://docs.docker.com/windows/step_one/ 

Une fois les outils Docker installés, téléchargez la version Mac OS de Visual Studio Code depuis https://go.microsoft.com/fwlink/?LinkID=534107. Exécutez alors l'installeur VSCodeSetup.exe et laissez-vous guider.

Lancez Visual Studio Code pour valider que tout est OK.
Vous êtes prêt pour démarrer le lab !

### Linux

Sous Linux, vous pouvez utiliser la ligne de commande Git. Vous trouverez les instructions d'installation pour la plupart des distributions sur cette page : https://git-scm.com/download/linux

Docker tourne nativement sous Linux, il n'y a pas de ToolBox comme sous Windows et Mac OS. Il est donc nécessaire d'installer séparement :

- Docker : un guide d'installation est disponible ici : https://docs.docker.com/linux/step_one/
- Docker Machine : un guide d'installation est disponible ici : https://docs.docker.com/machine/install-machine/

Visual Studio Code est disponible pour sous la forme d'un .deb (http://go.microsoft.com/fwlink/?LinkID=760868) pour les distributions basées sur Debian ou d'un .rpm (http://go.microsoft.com/fwlink/?LinkID=760867) pour les distributions basées sur Red-Hat.
Une fois le bon package téléchargé, l'installation se fait de la manière suivante:

    # For .deb
    sudo dpkg -i <file>.deb

    # For .rpm (Fedora 21 and below)
    sudo yum install <file>.rpm

    # For .rpm (Fedora 22 and above)
    sudo dnf install <file>.rpm

Il vous suffit ensuite de lancer la commande "code" pour démarrer Visual Studio Code !
Vous êtes prêt pour démarrer le lab !

## Création d'une machine hôte Docker dans Microsoft Azure

### Récupérartion de votre ID d'abonnement

Rendez-vous sur http://portal.azure.com et authentifiez vous avec votre compte Microsoft Azure. Une fois connecté, dans le menu de gauche, recherchez le lien vers vos abonnements et cliquez dessus pour afficher la liste :

![Liste des abonnements Microsoft Azure](https://github.com/DXFrance/DockerLab/blob/master/medias/id_subscription.png)

Copiez alors l'ID de votre abonnement, vous en aurez besoin pour la suite.

### Création de la machine virtuelle avec Docker Machine

Ouvrez un terminal / shell et tapez :

    export AZURE_SUBSCRIPTION_ID=<votre id d'abonnement>

En remplaçant <votre id d'abonnement> par l'id que vous avez récupéré sur le portail Microsoft Azure à l'étape précédente

Tapez ensuite la commande :

    docker-machine create -d azure <nom vm>

En remplaçant <nom vm> par le nom que vous souhaitez donner à votre machine virtuelle.

Cette commande va déclencher la création de toutes les ressources nécessaires au déploiement d'une machine virtuelle Linux avec Docker d'installé dans le Cloud Microsoft Azure.
Lors de la première connexion, vous devez vous authentifier avec votre compte Microsoft Azure. La commande attend d'ailleurs que vous vous connectiez à l'adresse http://aka.ms/devicelogin en utilisant le code fourni :

![Authentification depuis Docker Machine](https://github.com/DXFrance/DockerLab/blob/master/medias/device_login.png)

Accédez donc à l'adresse http://aka.ms/devicelogin depuis votre navigateur et entrez le code qui vous a été fourni par la ligne de commande :

![Authentification depuis Docker Machine - Etape 2](https://github.com/DXFrance/DockerLab/blob/master/medias/device_login_2.png)

Une fois authentifié depuis votre navigateur, la ligne de commande reprend et la création de la machine virtuelle commence !

![Création de la machine virtuelle...](https://github.com/DXFrance/DockerLab/blob/master/medias/cmd_execution.png)

Patientez quelques instants le temps que la machine soit créée... Une fois l'opération terminée, vous pourrez vous connecter à la machine en SSH.
Pour cela, tapez la commande :

    docker-machine ssh <nom vm>

En remplaçant <nom vm> par le nom que vous avez donné à votre machine virtuelle. Et voilà, vous êtes connecté à la machine virtuelle.

Fermez la connexion ssh et tapez la commande :

    docker-machine ls

Cette commande vous permet de lister les machines hôtes Docker créées et gérées avec Docker Machine. Vous devez voir apparaître la machine créée à l'instant.

### Configuration de votre environnement

Afin de pouvoir travailler avec votre nouvelle machine Linux hébergée dans Azure, il est nécessaire de définir quelques variables d'environnement. Cela vous évitera de vous connecter en SSH à chaque fois. Tapez la commande :

    docker-machine env <nom vm>

Exécutez alors les différentes commandes qui vous sont retournées par la commande.

Votre environnement est prêt ! Pour valider que tout est bon, vous pouvez taper la commande suivante :

    docker info

Cette commande doit vous afficher les informations propres au deamon Docker qui s'exécute sur la machine virtuelle dans Microsoft Azure.

## Création de l'image Docker pour l'application Asp.Net Core 1.0

Pour commencez, faites un clone du repository GitHub sur votre machine pour récupérer les sources de l'application : https://github.com/DXFrance/DockerLab.git

Une fois ceci fait, naviguez vers le dossier **src**. Celui-ci contient un fichier **Dockerfile** qui défini comment l'image Docker qui sera utilisée pour exécuter l'application Asp.Net Core doit être créée:

    # Utilisation de l'image de base fournie par Microsoft pour exécuter des applications .NET Core sous Linux
    FROM microsoft/dotnet:1.0.0-preview1

    # Définition de la personne qui maintient le projet
    MAINTAINER Julien Corioland, Microsoft France, @jcorioland

    # Définition du port sur lequel l'application va s'exécuter à l'intérieur du conteneur
    EXPOSE 5000

    # Définition du point d'entrée de l'application (exécuté au démarrage du conteneur)
    ENTRYPOINT ["dotnet", "run"]

    # Copie du contenu du répertoire HelloAspNet local à l'intérieur du conteneur dans le répertoire /app
    ADD ./HelloAspNet/ ./app

    # Définition du répertoire courant
    WORKDIR /app

    # Restauration des dépendences du projet et compilation de l'application
    RUN dotnet restore
    RUN dotnet build 

Comme vous pouvez le constater, ce fichier décrit les différentes actions à réaliser pour créer l'image.

* La directive *FROM* permet d'indiquer quelle est l'image qui servira de base. En l'occurence, nous utilisons ici l'image officielle de Dotnet Core sous Linux fournie par Microsoft.
* La directive *MAINTAINER* permet de définir l'auteur de l'image
* La directive *EXPOSE* permet d'exposer un port à l'extérieur du conteneur (celui sur lequel s'exécutera l'application, ici 5000)
* La directive *ENTRYPOINT* permet de définir la commande qui sera exécutée au démarrage du conteneur, ici : *dotnet run*
* La directive *ADD* permet d'envoyer le contenu du répertoire **HelloAspNet** de votre poste client au dossier **/app** à l'intérieur du conteneur
* La directive *WORKDIR* permet de définir le répertoire de travail courant
* La directive *RUN* permet d'exécuter une commande à l'intérieur du répertoire. Ici, nous en exécutant deux : *dotnet restore* qui permet de récupérer les dépendences de l'application et *dotnet build* qui permet de compiler l'application.

Pour obtenir une image à partir d'un fichier Dockerfile, il suffit de taper la commande suivante :

    docker build -t <nom_de_votre_image> .

En remplaçant <nom_de_votre_image> par le nom que vous souhaitez donner à votre image.

Il est important de comprendre que ce qu'il se passe en réalité, c'est que tout le contexte qui est sur votre poste (le Dockerfile ainsi que les fichiers du dossier HelloAspNet) vont être envoyé au deamon Docker qui tourne sur la machine virtuelle distante, dans Microsoft Azure. Et c'est ensuite sur cette machine distante que l'image va être créée !

Patientez pendant la création de l'image, cela peut prendre quelques minutes. Vous pouvez valider que votre image a bien été créée, vous pouvez taper la commande :

    docker images

Cette commande affiche toutes les images qui sont disponibles sur la machine Docker.

## Exécution de l'application dans un conteneur Docker

Maintenant que l'image a été créée, il est possible d'instancier un conteneur à partir de celle-ci, à l'aide de la commande :

    docker run -d -p 80:5000 --name <nom_conteneur> <nom_de_votre_image>

En remplaçant <nom_conteneur> par le nom que vous souhaitez donner à votre conteneur et <nom_de_votre_image> par le nom que vous avez donné à l'image lors de sa création.

L'option *-d* permet d'indiquer que le conteneur doit tourner en tâche de fond et l'option *-p* qui permet d'indiquer que le port 5000 exposé par le conteneur doit être mappé sur le port 80 de la machine virtuelle hôte.

Une fois le conteneur lancé vous pouvez valider qu'il est en cours d'exécution en tapant la commande :

    docker ps -a

## Configuration du Firewall et connexion à l'application

Et bien évidemment vous pouvez vous connecter à l'application. Pour se faire, rendez-vous sur le portail Microsoft Azure (https://portal.azure.com) et authentifiez vous avec votre compte. A l'aide du menu de gauche, parcourez les groupes de ressources. Vous devez voir un groupe de ressource nommé *docker-machine* qui contient toutes les ressources qui ont été créée pour votre machine virtuelle (compte de stockage, carte réseau, firewall...) et notamment une IP publique :

Vous pourrez alors récupérer l'IP publique de votre machine virtuelle via cette interface, et vous y connecter :

![Récupération de l'IP publique](https://github.com/DXFrance/DockerLab/blob/master/medias/public_ip.png)

Une autre solution est d'utiliser Docker Machine et la commande : 

    docker-machine ip <nom_de_votre_vm>

Si vous tentez de vous connecter à la machine en http en utilisant l'IP que vous venez de récupérer, vous constaterez que la page web ne s'affiche pas, tout simplement car vous n'avez pas autorisé le trafic entrant sur le port 80 au niveau du firewall de la machine virtuelle.
Retournez sur le portail Microsoft Azure, et cliquez cette fois sur le firewall, puis sur *Règles de sécurité de trafic entrant* dans le menu de droite. Dans l'interface qui s'ouvre, cliquez sur le bouton **+** et ajoutez une règle pour autoriser le port 80 :

![Récupération de l'IP publique](https://github.com/DXFrance/DockerLab/blob/master/medias/add_trafic_inbound.png)

Validez en cliquant sur OK et patientez quelques instants le temps que la règle s'applique. Vous pouvez alors vous tenter de parcourir l'application web à nouveau, à partir de l'IP de la machine.
Cette fois, tout devrait fonctionner !
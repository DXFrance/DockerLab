# HOL - Azure & Docker

## Introduction

Le but de ce lab est de vous permettre de déployer une application ASP.NET Core dans un conteneur Docker, sur une machine Linux hébergée sur la plateforme cloud Microsoft Azure. 
Il est entièrement réalisable depuis un post sous Linux, MacOS ou Windows.

ASP.NET Core 1.0 est une technologie qui permet de déployer des applications web à l'aide du langage C# et qui a été pensé pour s'exécuter sur n'importe quelle plateforme Linux, Mac OS ou Windows.
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

## Clone du repository d'exemple


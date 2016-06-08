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
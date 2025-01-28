### **Cours Ludique sur Docker : Initiation, Fonctionnement, Syntaxe et Fonctions**

---

#### **1. Tableau/Diagramme Explicatif du Fonctionnement de Docker**

| **Élément**               | **Description**                                                                 | **Analogie**                                                                 |
|---------------------------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| **Docker Engine**          | Le cœur de Docker, qui gère les conteneurs, les images, les réseaux, etc.     | Comme un moteur de voiture qui fait fonctionner tout le système.            |
| **Images Docker**          | Des modèles pré-configurés pour créer des conteneurs.                         | Comme un moule à gâteau qui permet de créer plusieurs gâteaux identiques.   |
| **Conteneurs**             | Des instances d’images Docker en cours d’exécution.                          | Comme une boîte contenant tout ce dont une application a besoin pour tourner.|
| **Dockerfile**             | Un fichier texte contenant les instructions pour créer une image Docker.      | Comme une recette de cuisine pour préparer un plat.                        |
| **Docker Hub**             | Un registre public où l’on peut trouver et partager des images Docker.        | Comme une bibliothèque où on emprunte des livres (images).                 |
| **Volumes Docker**         | Des espaces de stockage persistants pour les données des conteneurs.          | Comme une clé USB pour sauvegarder des fichiers.                           |
| **Réseaux Docker**         | Permet aux conteneurs de communiquer entre eux et avec l’extérieur.           | Comme un réseau téléphonique pour connecter des appels.                    |
| **Docker Compose**         | Un outil pour gérer plusieurs conteneurs dans une application multi-services. | Comme un chef d’orchestre qui coordonne plusieurs musiciens (conteneurs).  |

---

#### **2. Bibliothèque du Langage Initial des Projets Holberton Concernant Docker**

| **Concept**                | **Description**                                                                 | **Exemple**                                                                 |
|---------------------------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| **Dockerfile**             | Fichier de configuration pour créer une image Docker.                         | `FROM ubuntu`, `RUN apt-get update`, `COPY . /app`, `CMD ["python3", "app.py"]` |
| **Images**                 | Modèles pour créer des conteneurs.                                            | `docker build -t mon_image .`                                              |
| **Conteneurs**             | Instances d’images en cours d’exécution.                                      | `docker run -d --name mon_conteneur mon_image`                             |
| **Volumes**                | Stockage persistant pour les données.                                         | `docker volume create mon_volume`                                          |
| **Réseaux**                | Communication entre conteneurs.                                               | `docker network create mon_reseau`                                         |
| **Docker Compose**         | Gestion de plusieurs conteneurs.                                              | `docker-compose up -d`                                                     |
| **Docker Hub**             | Registre public d’images Docker.                                              | `docker pull ubuntu`, `docker push mon_image`                              |

---

#### **3. Tableau des Syntaxes et Fonctions de Docker**

| **Commande**               | **Description**                                                                 | **Exemple**                                                                 |
|---------------------------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| **docker build**           | Crée une image à partir d’un Dockerfile.                                      | `docker build -t mon_image .`                                              |
| **docker run**             | Lance un conteneur à partir d’une image.                                      | `docker run -d --name mon_conteneur mon_image`                             |
| **docker ps**              | Liste les conteneurs en cours d’exécution.                                    | `docker ps -a` (affiche tous les conteneurs)                               |
| **docker stop**            | Arrête un conteneur.                                                          | `docker stop mon_conteneur`                                                |
| **docker rm**              | Supprime un conteneur.                                                        | `docker rm mon_conteneur`                                                  |
| **docker images**          | Liste les images disponibles localement.                                      | `docker images`                                                            |
| **docker rmi**             | Supprime une image.                                                           | `docker rmi mon_image`                                                     |
| **docker pull**            | Télécharge une image depuis Docker Hub.                                       | `docker pull ubuntu`                                                       |
| **docker push**            | Envoie une image vers Docker Hub.                                             | `docker push mon_image`                                                    |
| **docker exec**            | Exécute une commande dans un conteneur en cours d’exécution.                  | `docker exec -it mon_conteneur bash`                                       |
| **docker logs**            | Affiche les logs d’un conteneur.                                              | `docker logs mon_conteneur`                                                |
| **docker network**         | Gère les réseaux Docker.                                                      | `docker network create mon_reseau`                                         |
| **docker volume**          | Gère les volumes Docker.                                                      | `docker volume create mon_volume`                                          |
| **docker-compose up**      | Lance les services définis dans un fichier `docker-compose.yml`.              | `docker-compose up -d`                                                     |
| **docker-compose down**    | Arrête et supprime les services définis dans un fichier `docker-compose.yml`. | `docker-compose down`                                                      |

---

#### **4. Exemple de Dockerfile**

```Dockerfile
# Utiliser une image de base
FROM ubuntu:latest

# Mettre à jour le système et installer des packages
RUN apt-get update && apt-get install -y python3

# Copier les fichiers de l'application dans le conteneur
COPY . /app

# Définir le répertoire de travail
WORKDIR /app

# Commande à exécuter au lancement du conteneur
CMD ["python3", "app.py"]
```

---

#### **5. Exemple de docker-compose.yml**

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  api:
    image: my_api_image
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```

---

#### **6. Cours Ludique : Initiation à Docker**

**Étape 1 : Installer Docker**  
- Téléchargez Docker sur votre machine (Windows, macOS, Linux).  
- Vérifiez l’installation avec `docker --version`.

**Étape 2 : Créer une Image**  
- Écrivez un `Dockerfile` pour définir votre environnement.  
- Construisez l’image avec `docker build -t mon_image .`.

**Étape 3 : Lancer un Conteneur**  
- Exécutez un conteneur avec `docker run -d --name mon_conteneur mon_image`.  
- Vérifiez qu’il fonctionne avec `docker ps`.

**Étape 4 : Explorer Docker Compose**  
- Créez un fichier `docker-compose.yml` pour gérer plusieurs services.  
- Lancez les services avec `docker-compose up -d`.

**Étape 5 : Partager votre Image**  
- Envoyez votre image sur Docker Hub avec `docker push mon_image`.  
- Téléchargez une image avec `docker pull mon_image`.

---

#### **7. Tableau des Bonnes Pratiques**

| **Conseil**                | **Description**                                                                 |
|---------------------------|-------------------------------------------------------------------------------|
| **Utiliser des images officielles** | Toujours privilégier les images officielles pour la sécurité et la stabilité. |
| **Minimiser les couches**  | Réduire le nombre de lignes dans le Dockerfile pour optimiser la taille de l’image. |
| **Utiliser .dockerignore** | Ignorer les fichiers inutiles lors de la construction de l’image.             |
| **Limiter les privilèges** | Ne pas exécuter les conteneurs en mode root pour des raisons de sécurité.     |
| **Utiliser des volumes**   | Stocker les données persistantes dans des volumes pour éviter de les perdre.  |

---

Avec ce cours ludique, vous êtes maintenant prêt à explorer Docker et à l’utiliser dans vos projets Holberton ! 🚀🐳
# 0. Create Your First Docker Image

### **Tâche 0 : Créez votre première image Docker**

Pour créer une image Docker, vous devrez utiliser un **Dockerfile**. Créez un Dockerfile qui :

1. Est basé sur la dernière version d'**Ubuntu**.
2. Met à jour **APT** en utilisant `apt-get update`.
3. Met à niveau les logiciels déjà installés via **APT** en utilisant `apt-get upgrade -y`.

Une fois l'image construite, vous pouvez exécuter l'image Docker dans un conteneur, et elle affichera **"Hello, World!"** dans le terminal.

---

### **Exemple de sortie**
Voici un exemple de ce à quoi votre sortie pourrait ressembler (cela peut varier en fonction de votre environnement local et de la présence de données en cache) :

```bash
Dereks-MacBook-Pro:docker-project derekwebb$ docker build -f ./Dockerfile -t softy-pinko:task0 .
[+] Building 0.7s (7/7) FINISHED                                                                  
 => [internal] load build definition from Dockerfile                                         0.0s
 => => transferring dockerfile: 37B                                                          0.0s
 => [internal] load .dockerignore                                                            0.0s
 => => transferring context: 2B                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                             0.6s
 => [1/3] FROM docker.io/library/ubuntu:latest@sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d  0.0s
 => CACHED [2/3] RUN apt-get update                                                          0.0s
 => CACHED [3/3] RUN apt-get upgrade -y                                                      0.0s
 => exporting to image                                                                       0.0s
 => => exporting layers                                                                      0.0s
 => => writing image sha256:45d461b2a1471047589659e82af46202206be08b5b725d941a0a659b843a402  0.0s
 => => naming to docker.io/library/softy-pinko:task0                                         0.0s
Dereks-MacBook-Pro:docker-project derekwebb$ docker run -it --rm --name softy-pinko-task0 softy-pinko:task0
Hello, World!
Dereks-MacBook-Pro:docker-project derekwebb$
```

---

### **Dépôt GitHub**
- **Dépôt :** [holbertonschool-softy-pinko-docker](https://github.com/holbertonschool/holbertonschool-softy-pinko-docker)  
- **Répertoire :** `task0`  

---

### **Tutoriel détaillé : Créer votre première image Docker**

#### **Étape 1 : Créer un Dockerfile**
Un **Dockerfile** est un fichier texte qui contient toutes les commandes nécessaires pour assembler une image Docker. Voici comment créer un Dockerfile pour cette tâche :

1. **Base de l'image :**  
   - Nous utilisons la dernière version d'Ubuntu comme base.  
   - Ajoutez cette ligne au Dockerfile :  
     ```Dockerfile
     FROM ubuntu:latest
     ```

2. **Mettre à jour APT :**  
   - APT est le gestionnaire de paquets d'Ubuntu. Nous devons le mettre à jour pour obtenir les dernières informations sur les paquets disponibles.  
   - Ajoutez cette ligne :  
     ```Dockerfile
     RUN apt-get update
     ```

3. **Mettre à niveau les logiciels installés :**  
   - Nous mettons à niveau tous les logiciels déjà installés pour nous assurer que nous utilisons les dernières versions.  
   - Ajoutez cette ligne :  
     ```Dockerfile
     RUN apt-get upgrade -y
     ```

4. **Afficher "Hello, World!" :**  
   - Enfin, nous ajoutons une commande pour afficher "Hello, World!" lorsque le conteneur est exécuté.  
   - Ajoutez cette ligne :  
     ```Dockerfile
     CMD echo "Hello, World!"
     ```

#### **Dockerfile complet**
Voici à quoi ressemble le Dockerfile complet :

```Dockerfile
# Utiliser la dernière version d'Ubuntu comme base
FROM ubuntu:latest

# Mettre à jour APT
RUN apt-get update

# Mettre à niveau les logiciels installés
RUN apt-get upgrade -y

# Afficher "Hello, World!" lors de l'exécution du conteneur
CMD echo "Hello, World!"
```

---

#### **Étape 2 : Construire l'image Docker**
Une fois le Dockerfile créé, vous pouvez construire l'image Docker en utilisant la commande suivante :

```bash
docker build -f ./Dockerfile -t softy-pinko:task0 .
```

- **`docker build`** : Commande pour construire une image Docker.
- **`-f ./Dockerfile`** : Spécifie le fichier Dockerfile à utiliser.
- **`-t softy-pinko:task0`** : Donne un nom (`softy-pinko`) et un tag (`task0`) à l'image.
- **`.`** : Indique que le contexte de construction est le répertoire courant.

---

#### **Étape 3 : Exécuter le conteneur**
Une fois l'image construite, vous pouvez exécuter un conteneur à partir de cette image en utilisant la commande suivante :

```bash
docker run -it --rm --name softy-pinko-task0 softy-pinko:task0
```

- **`docker run`** : Commande pour exécuter un conteneur.
- **`-it`** : Exécute le conteneur en mode interactif avec un terminal.
- **`--rm`** : Supprime automatiquement le conteneur après son arrêt.
- **`--name softy-pinko-task0`** : Donne un nom au conteneur.
- **`softy-pinko:task0`** : Spécifie l'image à utiliser.

---

#### **Explication des commandes Docker**

1. **`FROM ubuntu:latest`** :  
   - Cette commande indique que notre image Docker sera basée sur la dernière version d'Ubuntu.  
   - **`ubuntu:latest`** est une image officielle disponible sur Docker Hub.

2. **`RUN apt-get update`** :  
   - Cette commande met à jour la liste des paquets disponibles dans APT.  
   - Elle est essentielle pour s'assurer que nous pouvons installer les dernières versions des logiciels.

3. **`RUN apt-get upgrade -y`** :  
   - Cette commande met à niveau tous les logiciels déjà installés.  
   - L'option **`-y`** répond automatiquement "oui" à toutes les questions pendant la mise à niveau.

4. **`CMD echo "Hello, World!"`** :  
   - Cette commande définit la commande par défaut à exécuter lorsque le conteneur démarre.  
   - Ici, elle affiche simplement "Hello, World!" dans le terminal.

---

#### **Résultat attendu**
Lorsque vous exécutez le conteneur, vous devriez voir la sortie suivante dans votre terminal :

```bash
Hello, World!
```

Cela signifie que votre conteneur fonctionne correctement et exécute la commande définie dans le Dockerfile.

---

### **Conclusion**
Ce tutoriel vous a guidé à travers la création de votre première image Docker. Vous avez appris à :

1. Créer un **Dockerfile** pour définir les étapes de construction de l'image.
2. Utiliser des commandes de base comme **`FROM`**, **`RUN`**, et **`CMD`**.
3. Construire une image Docker avec **`docker build`**.
4. Exécuter un conteneur avec **`docker run`**.

Ces compétences de base sont essentielles pour travailler avec Docker et créer des environnements de développement et de déploiement portables et reproductibles.
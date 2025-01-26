# 1. Back-end

#### **Tâche 1 : Back-end**

Pour cette tâche, commencez par faire une copie de votre répertoire `task0` et nommez-le `task1`. Ensuite, nous allons modifier le Dockerfile pour installer **Python3**, **pip3** et **Flask**. Vous n'avez peut-être pas encore utilisé Flask, mais ne vous inquiétez pas ; pour ce projet, nous vous fournirons tout le code Flask nécessaire pour démarrer. Nous validerons que tout a été installé correctement en exécutant un serveur Flask avec un point de terminaison qui, lorsqu'il est appelé, retourne **"Hello, World!"**.

---

### **Instructions**

1. **Installer Python3, pip3 et Flask :**
   - Assurez-vous d'installer automatiquement et de sauter les entrées utilisateur avec l'option **`-y`** pour `apt-get`.
   - **Remarque :** Flask doit être installé avec **pip3**, et non via `apt-get`.
   - Si vous obtenez une erreur **"This environment is externally managed"** lors de l'installation des packages Python, ajoutez la ligne suivante avant d'appeler `pip` dans votre Dockerfile :
     ```Dockerfile
     RUN rm /usr/lib/python*/EXTERNALLY-MANAGED
     ```

2. **Créer un fichier Python local :**
   - Créez un fichier Python nommé `api.py` et collez le script suivant. Ce script utilise Flask pour créer un point de terminaison qui retourne **"Hello, World!"** lorsqu'il est appelé.
   - Hébergez cette application Flask sur **0.0.0.0** au lieu de **127.0.0.1** pour qu'elle soit accessible en dehors de la machine actuelle (la machine actuelle étant un conteneur Docker qui fonctionne sur votre ordinateur portable/bureau).
   - Hébergez cette application Flask sur le port **5252**.

   **`api.py` :**
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/api/hello')
   def hello_world():
       return 'Hello, World!'

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5252)
   ```

3. **Modifier le Dockerfile :**
   - Utilisez **`/app`** comme répertoire de travail dans votre Dockerfile.
   - Copiez le fichier Python (`api.py`) dans votre image Docker.

4. **Exécuter l'image Docker :**
   - Lorsque vous exécutez votre image Docker, votre serveur Flask doit démarrer et accepter les requêtes.
   - Assurez-vous de rediriger le port **5252** du conteneur Docker vers le port **5252** de la machine hôte lors de l'exécution de votre image dans un conteneur.

---

### **Exemple de sortie**
Voici un exemple de ce à quoi votre sortie pourrait ressembler (cela peut varier en fonction de votre environnement local et de la présence de données en cache) :

```bash
Dereks-MacBook-Pro:task1 derekwebb$ docker build -f ./Dockerfile -t softy-pinko:task1 .
[+] Building 0.9s (12/12) FINISHED                                                                
 => [internal] load build definition from Dockerfile                                         0.0s
 => => transferring dockerfile: 37B                                                          0.0s
 => [internal] load .dockerignore                                                            0.0s
 => => transferring context: 2B                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                             0.8s
 => [internal] load build context                                                            0.0s
 => => transferring context: 28B                                                             0.0s
 => [1/7] FROM docker.io/library/ubuntu:latest@sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd  0.0s
 => CACHED [2/7] RUN apt-get update                                                          0.0s
 => CACHED [3/7] RUN apt-get upgrade -y                                                      0.0s
 => CACHED [4/7] RUN apt-get install -y python3 python3-pip                                  0.0s
 => CACHED [5/7] RUN pip3 install flask                                                      0.0s
 => CACHED [6/7] WORKDIR /app                                                                0.0s
 => CACHED [7/7] COPY ./api.py /app/api.py                                                   0.0s
 => exporting to image                                                                       0.0s
 => => exporting layers                                                                      0.0s
 => => writing image sha256:58f5eb04ef4a3ac604fcc74adc799c09e09b2697675d9ec552d45c3a9e7d572  0.0s
 => => naming to docker.io/library/softy-pinko:task1                                         0.0s
Dereks-MacBook-Pro:task1 derekwebb$ docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5252
 * Running on http://172.17.0.2:5252
Press CTRL+C to quit
```

---

### **Dépôt GitHub**
- **Dépôt :** [holbertonschool-softy-pinko-docker](https://github.com/holbertonschool/holbertonschool-softy-pinko-docker)  
- **Répertoire :** `task1`  

---

### **Tutoriel détaillé : Créer un back-end Flask avec Docker**

#### **Étape 1 : Créer un Dockerfile**
Le Dockerfile est utilisé pour définir les étapes de construction de l'image Docker. Voici comment le créer pour cette tâche :

1. **Base de l'image :**  
   - Nous utilisons la dernière version d'Ubuntu comme base.  
   - Ajoutez cette ligne au Dockerfile :  
     ```Dockerfile
     FROM ubuntu:latest
     ```

2. **Mettre à jour APT et installer Python3 et pip3 :**  
   - Nous devons mettre à jour APT, puis installer Python3 et pip3.  
   - Ajoutez ces lignes :  
     ```Dockerfile
     RUN apt-get update
     RUN apt-get upgrade -y
     RUN apt-get install -y python3 python3-pip
     ```

3. **Installer Flask avec pip3 :**  
   - Flask doit être installé avec pip3.  
   - Ajoutez cette ligne :  
     ```Dockerfile
     RUN pip3 install flask
     ```

4. **Définir le répertoire de travail et copier le fichier Python :**  
   - Nous utilisons **`/app`** comme répertoire de travail et copions le fichier `api.py` dans l'image Docker.  
   - Ajoutez ces lignes :  
     ```Dockerfile
     WORKDIR /app
     COPY ./api.py /app/api.py
     ```

5. **Démarrer le serveur Flask :**  
   - Nous définissons la commande par défaut pour démarrer le serveur Flask.  
   - Ajoutez cette ligne :  
     ```Dockerfile
     CMD ["python3", "api.py"]
     ```

#### **Dockerfile complet**
Voici à quoi ressemble le Dockerfile complet :

```Dockerfile
# Utiliser la dernière version d'Ubuntu comme base
FROM ubuntu:latest

# Mettre à jour APT et installer Python3 et pip3
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y python3 python3-pip

# Installer Flask avec pip3
RUN pip3 install flask

# Définir le répertoire de travail
WORKDIR /app

# Copier le fichier Python dans l'image Docker
COPY ./api.py /app/api.py

# Démarrer le serveur Flask
CMD ["python3", "api.py"]
```

---

#### **Étape 2 : Construire l'image Docker**
Une fois le Dockerfile créé, vous pouvez construire l'image Docker en utilisant la commande suivante :

```bash
docker build -f ./Dockerfile -t softy-pinko:task1 .
```

- **`docker build`** : Commande pour construire une image Docker.
- **`-f ./Dockerfile`** : Spécifie le fichier Dockerfile à utiliser.
- **`-t softy-pinko:task1`** : Donne un nom (`softy-pinko`) et un tag (`task1`) à l'image.
- **`.`** : Indique que le contexte de construction est le répertoire courant.

---

#### **Étape 3 : Exécuter le conteneur**
Une fois l'image construite, vous pouvez exécuter un conteneur à partir de cette image en utilisant la commande suivante :

```bash
docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
```

- **`docker run`** : Commande pour exécuter un conteneur.
- **`-p 5252:5252`** : Redirige le port 5252 du conteneur vers le port 5252 de la machine hôte.
- **`-it`** : Exécute le conteneur en mode interactif avec un terminal.
- **`--rm`** : Supprime automatiquement le conteneur après son arrêt.
- **`--name softy-pinko-task1`** : Donne un nom au conteneur.
- **`softy-pinko:task1`** : Spécifie l'image à utiliser.

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

4. **`RUN apt-get install -y python3 python3-pip`** :  
   - Cette commande installe Python3 et pip3.  
   - **`python3`** est le langage de programmation, et **`pip3`** est le gestionnaire de paquets pour Python.

5. **`RUN pip3 install flask`** :  
   - Cette commande installe Flask, un framework web pour Python.  
   - Flask est utilisé pour créer des applications web légères et modulaires.

6. **`WORKDIR /app`** :  
   - Cette commande définit **`/app`** comme répertoire de travail dans le conteneur.  
   - Toutes les commandes suivantes seront exécutées dans ce répertoire.

7. **`COPY ./api.py /app/api.py`** :  
   - Cette commande copie le fichier `api.py` de votre machine locale vers le répertoire **`/app`** dans l'image Docker.

8. **`CMD ["python3", "api.py"]`** :  
   - Cette commande définit la commande par défaut à exécuter lorsque le conteneur démarre.  
   - Ici, elle démarre le serveur Flask en exécutant le fichier `api.py`.

---

#### **Résultat attendu**
Lorsque vous exécutez le conteneur, vous devriez voir la sortie suivante dans votre terminal :

```bash
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5252
 * Running on http://172.17.0.2:5252
Press CTRL+C to quit
```

Cela signifie que votre serveur Flask fonctionne correctement et est accessible à l'adresse `http://127.0.0.1:5252/api/hello`.

---

### **Conclusion**
Ce tutoriel vous a guidé à travers la création d'un back-end Flask avec Docker. Vous avez appris à :

1. Créer un **Dockerfile** pour installer Python3, pip3 et Flask.
2. Créer un fichier Python (`api.py`) pour démarrer un serveur Flask.
3. Construire une image Docker avec **`docker build`**.
4. Exécuter un conteneur avec **`docker run`** et rediriger les ports.

Ces compétences de base sont essentielles pour travailler avec Docker et créer des applications web modernes et portables.
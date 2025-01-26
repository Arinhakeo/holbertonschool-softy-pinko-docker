# 2. Front-end

#### **Tâche 2 : Front-end**

Pour cette tâche, commencez par faire une copie de votre répertoire `task1` et nommez-le `task2`. Nous avons créé un serveur API très simple avec une route qui retourne **"Hello, World!"**, et maintenant nous voulons créer une page web pour afficher le contenu de notre serveur API dans le contexte d'un front-end plus complet. Avant de créer notre front-end, réorganisons un peu ce projet dans le répertoire `task2`.

---

### **Instructions**

1. **Réorganiser le projet :**
   - Créez un nouveau répertoire nommé **`back-end`** à l'intérieur de votre répertoire `task2`.
   - Déplacez tous les fichiers actuellement dans `task2` à l'intérieur du nouveau répertoire **`back-end`**.
   - Vous devriez maintenant avoir **`api.py`** et **`Dockerfile`** dans le répertoire **`task2/back-end`**.

2. **Créer un répertoire front-end :**
   - Créez un nouveau répertoire **`task2/front-end`**.
   - À l'intérieur de ce répertoire, clonez ce dépôt :  
     [https://github.com/atlas-school/softy-pinko-front-end](https://github.com/atlas-school/softy-pinko-front-end).

3. **Créer un Dockerfile pour le front-end :**
   - Dans le répertoire **`task2/front-end`**, créez un nouveau fichier **`Dockerfile`**.
   - Au lieu d'utiliser la dernière version d'Ubuntu, utilisez la dernière version de **Nginx**.
   - Les fichiers de **`softy-pinko-front-end`** doivent être copiés dans **`/var/www/html/softy-pinko-front-end`** sur l'image Docker.

4. **Configurer Nginx :**
   - Créez un fichier de configuration Nginx nommé **`softy-pinko-front-end.conf`** dans le répertoire **`task2/front-end`**.
   - Ce fichier doit être copié dans l'image Docker à l'emplacement **`/etc/nginx/conf.d/default.conf`**.
   - Le fichier de configuration doit inclure tous les paramètres nécessaires pour que votre site s'affiche lorsque vous visitez l'URL.
   - Lors de la recherche sur les fichiers de configuration Nginx, la seule section dont vous aurez besoin dans **`softy-pinko-front-end.conf`** est la section **`server`**. Faites attention à la syntaxe utilisée pour configurer un port à écouter (recommandation : port **9000**), le nom du serveur, l'emplacement et le fichier d'index à utiliser.

   **Exemple de `softy-pinko-front-end.conf` :**
   ```nginx
   server {
       listen 9000;
       server_name localhost;

       location / {
           root /var/www/html/softy-pinko-front-end;
           index index.html;
       }
   }
   ```

5. **Construire et exécuter l'image Docker :**
   - Construisez l'image Docker pour le front-end en utilisant la commande suivante :
     ```bash
     docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task2 ./front-end
     ```
   - Exécutez le conteneur en redirigeant le port **9000** du conteneur vers le port **9000** de la machine hôte :
     ```bash
     docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task2 softy-pinko-front-end:task2
     ```

---

### **Exemple de sortie**
Voici un exemple de ce à quoi votre sortie pourrait ressembler (cela peut varier en fonction de votre environnement local et de la présence de données en cache) :

```bash
Dereks-MacBook-Pro:task2 derekwebb$ docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task2 ./front-end
[+] Building 0.6s (8/8) FINISHED                                                                                                          
 => [internal] load build definition from Dockerfile                                                                                 0.0s
 => => transferring dockerfile: 37B                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                    0.0s
 => => transferring context: 2B                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                      0.5s
 => [internal] load build context                                                                                                    0.0s
 => => transferring context: 34.13kB                                                                                                 0.0s
 => [1/3] FROM docker.io/library/nginx:latest@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305                0.0s
 => CACHED [2/3] COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end                                                    0.0s
 => CACHED [3/3] COPY ./softy-pinko-front-end.conf  /etc/nginx/conf.d/default.conf                                                   0.0s
 => exporting to image                                                                                                               0.0s
 => => exporting layers                                                                                                              0.0s
 => => writing image sha256:5aeebcbf58006d92aa190103a44fa395f2e51a42cc7452a3561ce42af3b2aa46                                         0.0s
 => => naming to docker.io/library/softy-pinko-front-end:task2                                                                       0.0s
Dereks-MacBook-Pro:task2 derekwebb$ docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task2 softy-pinko-front-end:task2
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/06/12 17:00:32 [notice] 1#1: using the "epoll" event method
2023/06/12 17:00:32 [notice] 1#1: nginx/1.25.0
2023/06/12 17:00:32 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/06/12 17:00:32 [notice] 1#1: OS: Linux 5.15.49-linuxkit
2023/06/12 17:00:32 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/06/12 17:00:32 [notice] 1#1: start worker processes
2023/06/12 17:00:32 [notice] 1#1: start worker process 28
2023/06/12 17:00:32 [notice] 1#1: start worker process 29
2023/06/12 17:00:32 [notice] 1#1: start worker process 30
2023/06/12 17:00:32 [notice] 1#1: start worker process 31
2023/06/12 17:00:32 [notice] 1#1: start worker process 32
2023/06/12 17:00:32 [notice] 1#1: start worker process 33
2023/06/12 17:00:32 [notice] 1#1: start worker process 34
2023/06/12 17:00:32 [notice] 1#1: start worker process 35
```

---

### **Dépôt GitHub**
- **Dépôt :** [holbertonschool-softy-pinko-docker](https://github.com/holbertonschool/holbertonschool-softy-pinko-docker)  
- **Répertoire :** `task2`  

---

### **Tutoriel détaillé : Créer un front-end avec Nginx et Docker**

#### **Étape 1 : Réorganiser le projet**
1. **Créer un répertoire back-end :**
   - Dans le répertoire `task2`, créez un sous-répertoire nommé **`back-end`**.
   - Déplacez **`api.py`** et **`Dockerfile`** dans ce répertoire.

2. **Créer un répertoire front-end :**
   - Dans le répertoire `task2`, créez un sous-répertoire nommé **`front-end`**.
   - Clonez le dépôt **`softy-pinko-front-end`** dans ce répertoire :
     ```bash
     git clone https://github.com/atlas-school/softy-pinko-front-end.git
     ```

---

#### **Étape 2 : Créer un Dockerfile pour le front-end**
Dans le répertoire **`task2/front-end`**, créez un fichier **`Dockerfile`** avec le contenu suivant :

```Dockerfile
# Utiliser la dernière version de Nginx comme base
FROM nginx:latest

# Copier les fichiers du front-end dans le répertoire Nginx
COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end

# Copier le fichier de configuration Nginx
COPY ./softy-pinko-front-end.conf /etc/nginx/conf.d/default.conf

# Exposer le port 9000
EXPOSE 9000
```

---

#### **Étape 3 : Configurer Nginx**
Dans le répertoire **`task2/front-end`**, créez un fichier **`softy-pinko-front-end.conf`** avec le contenu suivant :

```nginx
server {
    listen 9000;
    server_name localhost;

    location / {
        root /var/www/html/softy-pinko-front-end;
        index index.html;
    }
}
```

---

#### **Étape 4 : Construire et exécuter l'image Docker**
1. **Construire l'image Docker :**
   - Utilisez la commande suivante pour construire l'image Docker :
     ```bash
     docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task2 ./front-end
     ```

2. **Exécuter le conteneur :**
   - Utilisez la commande suivante pour exécuter le conteneur :
     ```bash
     docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task2 softy-pinko-front-end:task2
     ```

---

#### **Résultat attendu**
Lorsque vous exécutez le conteneur, vous devriez voir la sortie suivante dans votre terminal :

```bash
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:9000
Press CTRL+C to quit
```

Cela signifie que votre serveur Nginx fonctionne correctement et est accessible à l'adresse `http://127.0.0.1:9000`.

---

### **Conclusion**
Ce tutoriel vous a guidé à travers la création d'un front-end avec Nginx et Docker. Vous avez appris à :

1. Réorganiser un projet pour séparer le back-end et le front-end.
2. Créer un Dockerfile pour un front-end utilisant Nginx.
3. Configurer Nginx pour servir des fichiers statiques.
4. Construire et exécuter une image Docker pour le front-end.

Ces compétences sont essentielles pour créer des applications web modernes et portables.
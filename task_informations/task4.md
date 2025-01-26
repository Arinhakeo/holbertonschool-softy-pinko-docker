# 4. Making it Simpler with Docker Compose

#### **Tâche 4 : Simplifier avec Docker Compose**
 
Avoir des images/conteneurs Docker distincts pour chaque composant de votre application peut réduire la complexité de vos applications web. Cependant, avoir plus d'une seule image Docker à démarrer dans des conteneurs peut présenter des défis. Que faire si vous devez démarrer 3 images Docker distinctes, ou 7, ou 50 ? Les démarrer une par une deviendrait rapidement un problème. C'est là que **Docker Compose** entre en jeu. Avec un fichier **`docker-compose.yml`**, nous pouvons spécifier les différents composants de votre application, configurer quelques paramètres de base, et laisser Docker gérer le démarrage de toute l'application en une seule fois, quel que soit le nombre de conteneurs.

Avant d'aller plus loin, assurez-vous de copier le répertoire `task3` et de le nommer `task4`.

Dans le répertoire `task4`, créez un fichier **`docker-compose.yml`**.

Une fois que vous avez configuré votre fichier **`docker-compose.yml`** avec les services corrects, vous devriez pouvoir les construire en utilisant **`docker-compose build`** et les exécuter avec **`docker-compose up`** comme ceci :

---

### **Exemple de sortie**

#### **Terminal**
```bash
Dereks-MacBook-Pro:task4 derekwebb$ docker-compose build
[+] Building 1.3s (13/13) FINISHED                                                                                              
 => [internal] load build definition from Dockerfile                                                                       0.0s
 => => transferring dockerfile: 32B                                                                                        0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                           1.2s
 => [1/8] FROM docker.io/library/ubuntu:latest@sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd032e2246d30a722fa348fd799a5     0.0s
 => [internal] load build context                                                                                          0.0s
 => => transferring context: 28B                                                                                           0.0s
 => CACHED [2/8] RUN apt-get update                                                                                        0.0s
 => CACHED [3/8] RUN apt-get upgrade -y                                                                                    0.0s
 => CACHED [4/8] RUN apt-get install -y python3 python3-pip                                                                0.0s
 => CACHED [5/8] RUN pip3 install flask                                                                                    0.0s
 => CACHED [6/8] RUN pip3 install flask-cors                                                                               0.0s
 => CACHED [7/8] WORKDIR /app                                                                                              0.0s
 => CACHED [8/8] COPY ./api.py /app/api.py                                                                                 0.0s
 => exporting to image                                                                                                     0.0s
 => => exporting layers                                                                                                    0.0s
 => => writing image sha256:26379ebe4741c491978aa03c623ebbd768c964fefba64055e3013460fe7c5a1d                               0.0s
 => => naming to docker.io/library/softy-pinko-back-end:task4                                                              0.0s
[+] Building 1.0s (8/8) FINISHED                                                                                                
 => [internal] load build definition from Dockerfile                                                                       0.0s
 => => transferring dockerfile: 32B                                                                                        0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                            0.8s
 => [internal] load build context                                                                                          0.1s
 => => transferring context: 3.46kB                                                                                        0.0s
 => [1/3] FROM docker.io/library/nginx:latest@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305      0.0s
 => CACHED [2/3] COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end                                          0.0s
 => CACHED [3/3] COPY ./softy-pinko-front-end.conf  /etc/nginx/conf.d/default.conf                                         0.0s
 => exporting to image                                                                                                     0.1s
 => => exporting layers                                                                                                    0.0s
 => => writing image sha256:479f9d992a6eae6a56dff4b0cf7f9754446fa195ef273afba5705f460fee8da4                               0.0s
 => => naming to docker.io/library/softy-pinko-front-end:task4                                                             0.0s
Dereks-MacBook-Pro:task4 derekwebb$ 
Dereks-MacBook-Pro:task4 derekwebb$ docker-compose up
[+] Running 0/2
 ⠿ front-end Warning                                                                                                                                                                                                                               2.1s
 ⠿ back-end Warning                                                                                                                                                                                                                                2.1s
[+] Building 1.6s (20/20) FINISHED                                                                                                                                                                                                                      
 => [softy-pinko-front-end:task4 internal] load build definition from Dockerfile                                                                                                                                                                   0.0s
 => => transferring dockerfile: 188B                                                                                                                                                                                                               0.0s
 => [softy-pinko-back-end:task4 internal] load build definition from Dockerfile                                                                                                                                                                    0.0s
 => => transferring dockerfile: 262B                                                                                                                                                                                                               0.0s
 => [softy-pinko-front-end:task4 internal] load .dockerignore                                                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                                                                                    0.0s
 => [softy-pinko-back-end:task4 internal] load .dockerignore                                                                                                                                                                                       0.0s
 => => transferring context: 2B                                                                                                                                                                                                                    0.0s
 => [softy-pinko-front-end:task4 internal] load metadata for docker.io/library/nginx:latest                                                                                                                                                        1.4s
 => [softy-pinko-back-end:task4 internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                                                        1.4s
 => [softy-pinko-back-end:task4 internal] load build context                                                                                                                                                                                       0.0s
 => => transferring context: 250B                                                                                                                                                                                                                  0.0s
 => [softy-pinko-back-end:task4 1/8] FROM docker.io/library/ubuntu:latest@sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd032e2246d30a722fa348fd799a5                                                                                                  0.0s
 => [softy-pinko-front-end:task4 internal] load build context                                                                                                                                                                                      0.1s
 => => transferring context: 2.68MB                                                                                                                                                                                                                0.0s
 => [softy-pinko-front-end:task4 1/3] FROM docker.io/library/nginx:latest@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305                                                                                                  0.0s
 => CACHED [softy-pinko-back-end:task4 2/8] RUN apt-get update                                                                                                                                                                                     0.0s
 => CACHED [softy-pinko-back-end:task4 3/8] RUN apt-get upgrade -y                                                                                                                                                                                 0.0s
 => CACHED [softy-pinko-back-end:task4 4/8] RUN apt-get install -y python3 python3-pip                                                                                                                                                             0.0s
 => CACHED [softy-pinko-back-end:task4 5/8] RUN pip3 install flask                                                                                                                                                                                 0.0s
 => CACHED [softy-pinko-back-end:task4 6/8] RUN pip3 install flask-cors                                                                                                                                                                            0.0s
 => CACHED [softy-pinko-back-end:task4 7/8] WORKDIR /app                                                                                                                                                                                           0.0s
 => CACHED [softy-pinko-back-end:task4 8/8] COPY ./api.py /app/api.py                                                                                                                                                                              0.0s
 => [softy-pinko-front-end:task4] exporting to image                                                                                                                                                                                               0.0s
 => => exporting layers                                                                                                                                                                                                                            0.0s
 => => writing image sha256:26379ebe4741c491978aa03c623ebbd768c964fefba64055e3013460fe7c5a1d                                                                                                                                                       0.0s
 => => naming to docker.io/library/softy-pinko-back-end:task4                                                                                                                                                                                      0.0s
 => => writing image sha256:479f9d992a6eae6a56dff4b0cf7f9754446fa195ef273afba5705f460fee8da4                                                                                                                                                       0.0s
 => => naming to docker.io/library/softy-pinko-front-end:task4                                                                                                                                                                                     0.0s
 => CACHED [softy-pinko-front-end:task4 2/3] COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end                                                                                                                                      0.0s
 => CACHED [softy-pinko-front-end:task4 3/3] COPY ./softy-pinko-front-end.conf  /etc/nginx/conf.d/default.conf                                                                                                                                     0.0s
[+] Running 3/3
 ⠿ Network task4_default        Created                                                                                                                                                                                                            0.0s
 ⠿ Container task4-back-end-1   Created                                                                                                                                                                                                            0.1s
 ⠿ Container task4-front-end-1  Created                                                                                                                                                                                                            0.1s
Attaching to task4-back-end-1, task4-front-end-1
task4-back-end-1   |  * Serving Flask app 'api'
task4-back-end-1   |  * Debug mode: off
task4-back-end-1   | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
task4-back-end-1   |  * Running on all addresses (0.0.0.0)
task4-back-end-1   |  * Running on http://127.0.0.1:5252
task4-back-end-1   |  * Running on http://172.18.0.2:5252
task4-back-end-1   | Press CTRL+C to quit
task4-front-end-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
task4-front-end-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
task4-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
task4-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
task4-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
task4-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
task4-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
task4-front-end-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: using the "epoll" event method
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: nginx/1.25.0
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: OS: Linux 5.15.49-linuxkit
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker processes
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 28
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 29
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 30
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 31
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 32
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 33
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 34
task4-front-end-1  | 2023/06/12 19:27:43 [notice] 1#1: start worker process 35
```

---

### **Dépôt GitHub**
- **Dépôt :** [holbertonschool-softy-pinko-docker](https://github.com/holbertonschool/holbertonschool-softy-pinko-docker)  
- **Répertoire :** `task4`  

---

### **Tutoriel détaillé : Utiliser Docker Compose**

#### **Étape 1 : Créer un fichier `docker-compose.yml`**
Le fichier **`docker-compose.yml`** permet de définir les services (conteneurs) de votre application et de les démarrer ensemble. Voici comment le créer pour cette tâche :

1. **Définir les services :**
   - Nous avons deux services : **`back-end`** et **`front-end`**.
   - Chaque service est configuré avec un **`build`** (pour construire l'image) et des **`ports`** (pour exposer les ports).

2. **Exemple de `docker-compose.yml` :**
   ```yaml
   version: '3.8'

   services:
     back-end:
       build:
         context: ./back-end
         dockerfile: Dockerfile
       image: softy-pinko-back-end:task4
       ports:
         - "5252:5252"
       networks:
         - softy-pinko-network

     front-end:
       build:
         context: ./front-end
         dockerfile: Dockerfile
       image: softy-pinko-front-end:task4
       ports:
         - "9000:9000"
       depends_on:
         - back-end
       networks:
         - softy-pinko-network

   networks:
     softy-pinko-network:
       driver: bridge
   ```

#### **Explication des sections clés :**
- **`services`** : Liste des services (conteneurs) à démarrer.
- **`build`** : Spécifie le contexte (répertoire) et le fichier Dockerfile pour construire l'image.
- **`image`** : Nom de l'image Docker générée.
- **`ports`** : Redirige les ports du conteneur vers la machine hôte (format `HOSTPORT:CONTAINERPORT`).
- **`depends_on`** : Indique que le service **`front-end`** dépend du service **`back-end**.
- **`networks`** : Crée un réseau Docker pour permettre la communication entre les conteneurs.

---

#### **Étape 2 : Construire et exécuter les services**
1. **Construire les images :**
   - Utilisez la commande suivante pour construire les images Docker :
     ```bash
     docker-compose build
     ```

2. **Démarrer les conteneurs :**
   - Utilisez la commande suivante pour démarrer tous les services :
     ```bash
     docker-compose up
     ```

---

#### **Résultat attendu**
Lorsque vous accédez à l'application front-end à l'adresse `http://127.0.0.1:9000`, vous devriez voir le texte **"Hello, World!"** affiché dans l'élément **`dynamic-content`**.

---

### **Conclusion**
Ce tutoriel vous a guidé à travers l'utilisation de **Docker Compose** pour simplifier la gestion de plusieurs conteneurs Docker. Vous avez appris à :

1. Créer un fichier **`docker-compose.yml`** pour définir les services.
2. Configurer les services pour qu'ils communiquent entre eux.
3. Construire et exécuter plusieurs conteneurs en une seule commande.

Ces compétences sont essentielles pour gérer des applications complexes avec plusieurs composants.
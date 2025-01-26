# 6. Scale Horizontally

#### **Tâche 6 : Scale Horizontally**

Félicitations ! Vous avez un serveur proxy qui redirige les requêtes vers un serveur de contenu statique ou vers un serveur API pour les données dynamiques. Cela fonctionne bien pour l'instant... mais que se passerait-il si le trafic vers votre site augmentait considérablement au point où votre serveur API serait submergé de requêtes et ne pourrait plus répondre rapidement ? Dans ce cas, nous aimerions ajouter un deuxième serveur API et répartir la charge entre ces deux serveurs. Heureusement, avec Nginx et Docker, cela se fait simplement en ajoutant un drapeau lors de l'exécution de `docker-compose up` !

Pour cette tâche, faites une copie du répertoire `task5` et nommez-le `task6`. Votre objectif est de lancer vos conteneurs Docker de manière à avoir 2 (ou plus) serveurs API. Nginx agira comme un répartiteur de charge entre eux, de sorte que chaque requête soit envoyée au serveur API suivant en utilisant l'algorithme de répartition de charge Round-Robin.

Créez un nouveau fichier dans le répertoire `task6` nommé `2-api-servers.txt` et insérez-y la commande exacte `docker-compose` utilisée pour démarrer deux serveurs API au lieu d'un seul. Remarque : pour cette tâche, le vérificateur supposera que vous avez nommé votre serveur back-end `back-end`, et votre fichier doit se terminer par une nouvelle ligne. Votre sortie devrait ressembler à ceci :

**Terminal**

```bash
[+] Running 4/0
 ⠿ Container task6-back-end-2   Created                                                                                    0.0s
 ⠿ Container task6-back-end-1   Created                                                                                    0.0s
 ⠿ Container task6-front-end-1  Created                                                                                    0.0s
 ⠿ Container task6-proxy-1      Created                                                                                    0.0s
Attaching to task6-back-end-1, task6-back-end-2, task6-front-end-1, task6-proxy-1
task6-back-end-2   |  * Serving Flask app 'api'
task6-back-end-2   |  * Debug mode: off
task6-back-end-2   | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
task6-back-end-2   |  * Running on all addresses (0.0.0.0)
task6-back-end-2   |  * Running on http://127.0.0.1:5252
task6-back-end-2   |  * Running on http://172.20.0.2:5252
task6-back-end-2   | Press CTRL+C to quit
task6-back-end-1   |  * Serving Flask app 'api'
task6-back-end-1   |  * Debug mode: off
task6-back-end-1   | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
task6-back-end-1   |  * Running on all addresses (0.0.0.0)
task6-back-end-1   |  * Running on http://127.0.0.1:5252
task6-back-end-1   |  * Running on http://172.20.0.3:5252
task6-back-end-1   | Press CTRL+C to quit
task6-front-end-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
task6-front-end-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
task6-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
task6-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
task6-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
task6-front-end-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
task6-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
task6-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
task6-front-end-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: using the "epoll" event method
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: nginx/1.25.1
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: OS: Linux 5.15.49-linuxkit
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker processes
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 27
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 28
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 29
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 30
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 31
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 32
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 33
task6-front-end-1  | 2023/06/15 19:52:22 [notice] 1#1: start worker process 34
task6-proxy-1      | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
task6-proxy-1      | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
task6-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
task6-proxy-1      | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
task6-proxy-1      | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
task6-proxy-1      | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
task6-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
task6-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
task6-proxy-1      | /docker-entrypoint.sh: Configuration complete; ready for start up
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: using the "epoll" event method
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: nginx/1.25.1
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: OS: Linux 5.15.49-linuxkit
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker processes
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 28
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 29
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 30
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 31
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 32
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 33
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 34
task6-proxy-1      | 2023/06/15 19:52:23 [notice] 1#1: start worker process 35
```

Si vous allez sur la page web et que vous la rechargez plusieurs fois, votre sortie de terminal devrait ressembler à ceci :

**Terminal**

```bash
task6-back-end-1   | 172.20.0.5 - - [15/Jun/2023 19:53:55] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.5 - - [15/Jun/2023 19:53:57] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.5 - - [15/Jun/2023 19:53:58] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.5 - - [15/Jun/2023 19:53:58] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.5 - - [15/Jun/2023 19:53:59] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.5 - - [15/Jun/2023 19:54:00] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.5 - - [15/Jun/2023 19:54:00] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.5 - - [15/Jun/2023 19:54:01] "GET /api/hello HTTP/1.0" 200 -
```

Remarquez comment le serveur back-end auquel le répartiteur de charge Nginx a redirigé la requête alterne entre les deux serveurs back-end différents. Voici un exemple avec 5 serveurs back-end :

**Terminal**

```bash
task6-back-end-2   | 172.20.0.8 - - [15/Jun/2023 19:55:21] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-5   | 172.20.0.8 - - [15/Jun/2023 19:55:22] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.8 - - [15/Jun/2023 19:55:23] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-4   | 172.20.0.8 - - [15/Jun/2023 19:55:23] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-3   | 172.20.0.8 - - [15/Jun/2023 19:55:24] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.8 - - [15/Jun/2023 19:55:24] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-5   | 172.20.0.8 - - [15/Jun/2023 19:55:24] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.8 - - [15/Jun/2023 19:55:25] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-4   | 172.20.0.8 - - [15/Jun/2023 19:55:25] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-3   | 172.20.0.8 - - [15/Jun/2023 19:55:26] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-2   | 172.20.0.8 - - [15/Jun/2023 19:55:26] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-5   | 172.20.0.8 - - [15/Jun/2023 19:55:27] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-1   | 172.20.0.8 - - [15/Jun/2023 19:55:27] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-4   | 172.20.0.8 - - [15/Jun/2023 19:55:27] "GET /api/hello HTTP/1.0" 200 -
task6-back-end-3   | 172.20.0.8 - - [15/Jun/2023 19:55:28] "GET /api/hello HTTP/1.0" 200 -
```

**Repo :**

- GitHub repository : holbertonschool-softy-pinko-docker
- Directory : task6

---

### Explication détaillée

1. **Répartition de charge** : La répartition de charge (load balancing) est une technique utilisée pour distribuer les requêtes entre plusieurs serveurs afin d'éviter la surcharge d'un seul serveur. Ici, Nginx est utilisé comme répartiteur de charge.

2. **Round-Robin** : L'algorithme Round-Robin est une méthode simple de répartition de charge où chaque nouvelle requête est envoyée au serveur suivant dans une liste circulaire. Cela garantit une distribution équitable des requêtes.

3. **Docker Compose** : Docker Compose permet de gérer plusieurs instances d'un service en utilisant la commande `docker-compose up --scale`. Par exemple, pour démarrer deux instances du service `back-end`, vous pouvez utiliser la commande suivante :

   ```bash
   docker-compose up --scale back-end=2
   ```

4. **DNS interne de Docker** : Docker utilise un DNS interne pour résoudre les noms de services en adresses IP. Cela permet à Nginx de rediriger les requêtes vers les différentes instances du service `back-end` sans avoir à connaître leurs adresses IP exactes.

5. **Sécurité et isolation** : En ne mappant pas les ports des services `back-end` sur la machine hôte, vous limitez l'accès direct à ces services, ce qui renforce la sécurité. Toutes les requêtes doivent passer par le serveur proxy.

6. **Scalabilité horizontale** : Cette approche permet de facilement augmenter le nombre d'instances de votre service `back-end` pour gérer une augmentation du trafic. Vous pouvez ajouter autant d'instances que nécessaire en ajustant simplement la commande `docker-compose up --scale`.

En suivant ces étapes, vous avez créé un environnement scalable où les requêtes sont réparties entre plusieurs serveurs API, ce qui améliore la performance et la disponibilité de votre application.
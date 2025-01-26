# 5. Proxy Server

#### **Tâche 5 : Proxy Server**

Jusqu'à présent, nous avons créé un serveur statique et un serveur API, chacun ayant ses propres images Docker et fonctionnant dans des conteneurs Docker distincts. Cela fonctionne, mais cela signifie que nous devons gérer deux emplacements différents. Si nous changeons le port utilisé par le serveur API, nous devrons modifier le client pour qu'il se connecte à ce nouveau port. Dans ce cas, il s'agit de ports sur notre ordinateur, mais ce seraient des adresses IP si ces serveurs étaient externes. Ce n'est pas idéal, surtout si vous avez de nombreux clients différents à mettre à jour et à déployer.

Au lieu de cela, plaçons un serveur devant notre serveur statique et notre serveur API, qui agira comme un proxy entre le client et notre application complète. Les clients (dans ce cas, le navigateur web) n'auront besoin de connaître que l'emplacement du serveur proxy, et notre front-end pourra également simplement se connecter au même serveur proxy, plutôt que d'accéder directement à l'API, pour être ensuite redirigé vers le serveur API afin d'obtenir des données dynamiques.

Avant d'aller plus loin, copiez le répertoire `task4` et nommez-le `task5`. Dans le répertoire `task5`, créez un nouveau dossier nommé `proxy`.

Comme pour notre serveur de contenu statique, nous allons utiliser Nginx pour ce serveur proxy. Dans votre répertoire `proxy`, créez un fichier `Dockerfile` qui utilise la dernière version de Nginx. Créez également un fichier nommé `proxy.conf`, qui contiendra toute la configuration de votre serveur proxy. Dans le `Dockerfile`, assurez-vous de copier le fichier `proxy.conf` à cet emplacement spécifique dans votre image Docker pour le serveur proxy : `/etc/nginx/conf.d/default.conf`.

Pour le fichier `proxy.conf`, la seule section dont vous aurez besoin est la section "server". Vous voulez que ce serveur proxy écoute sur le port 80. Configurez une section `location` pour `/` et utilisez `proxy_pass` pour rediriger toutes les requêtes entrantes sur cette route vers `http://INSERT-YOUR-FRONT-END-DOCKER-COMPOSE-SERVICE-NAME-HERE:9000`. Ensuite, configurez une section `location` pour `/api` et utilisez `proxy_pass` pour rediriger toutes les requêtes entrantes sur cette route vers `http://INSERT-YOUR-BACK-END-DOCKER-COMPOSE-SERVICE-NAME-HERE:5252`.

**proxy.conf**

```nginx
server {
    // Remplacez par votre configuration de serveur Nginx
}
```

Notez que vous utilisez le même nom de service que celui que vous avez utilisé dans votre fichier `docker-compose.yml` pour les services front-end et back-end. Cela est dû au fait que Docker dispose d'un DNS interne qui configurera les routes vers les adresses IP internes pour ces noms. C'est un peu de "magie" que Docker fait, mais cela rend très pratique de ne pas avoir à connaître les adresses IP exactes de ces services.

Une autre chose qui doit être modifiée est le JavaScript utilisé. Auparavant, il appelait directement le serveur back-end ; maintenant, nous voulons que le JavaScript fasse également un appel API via le serveur proxy. Remplacez votre JavaScript en bas du fichier `index.html` par le suivant :

**JavaScript en bas de index.html**

```javascript
<script>
    // Charger des données dynamiques du back-end sur le port 5252
    $(function() {
        $.ajax({
            type: "GET",
            url: "/api/hello",
            success: function (data) {
                console.log(data);
                $('#dynamic-content').text(data);
            }
        });
    });
</script>
```

Ensuite, nous devons mettre à jour le fichier `docker-compose.yml` pour ajouter un nouveau service nommé `proxy`. Assurez-vous d'inclure les mêmes sections que précédemment : 
- `build`, `context`, `dockerfile`
- `image`
- `ports` (mappez le port 80 du conteneur au port 80 de la machine hôte)
- Note : Vous devez supprimer le mappage des ports pour les services front-end et back-end vers les ports de la machine hôte. Vous voudrez garder les ports 5252 et 9000 ouverts en interne sur les conteneurs afin que d'autres conteneurs Docker puissent les atteindre, mais vous ne voulez pas que des clients externes accèdent directement au front-end ou au back-end ; tout doit passer par le serveur proxy. Si vous ne mappez aucun port, vous ne pourrez pas passer le pare-feu que Docker a mis en place. Docker utilise quelque chose de similaire au pare-feu Linux `ufw` (Uncomplicated Firewall) ; vous ne travaillez pas directement avec ce pare-feu, mais vous mappez les ports autorisés dans les fichiers Dockerfiles ou docker-compose pour autoriser/interdire l'accès à certains ports.
- `depends_on`

Votre sortie devrait ressembler à ceci :

**Terminal**

```bash
Dereks-MacBook-Pro:task5 derekwebb$ docker-compose build
[+] Building 1.5s (13/13) FINISHED                                                                                              
 => [internal] load build definition from Dockerfile                                                                       0.0s
 => => transferring dockerfile: 262B                                                                                       0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                           1.4s
 => [1/8] FROM docker.io/library/ubuntu:latest@sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd032e2246d30a722fa348fd799a5     0.0s
 => [internal] load build context                                                                                          0.0s
 => => transferring context: 259B                                                                                          0.0s
 => CACHED [2/8] RUN apt-get update                                                                                        0.0s
 => CACHED [3/8] RUN apt-get upgrade -y                                                                                    0.0s
 => CACHED [4/8] RUN apt-get install -y python3 python3-pip                                                                0.0s
 => CACHED [5/8] RUN pip3 install flask                                                                                    0.0s
 => CACHED [6/8] RUN pip3 install flask-cors                                                                               0.0s
 => CACHED [7/8] WORKDIR /app                                                                                              0.0s
 => CACHED [8/8] COPY ./api.py /app/api.py                                                                                 0.0s
 => exporting to image                                                                                                     0.0s
 => => exporting layers                                                                                                    0.0s
 => => writing image sha256:839f2b832b76117f25a85adc73133026d69ab9008eb6c01459922b47cd61a2b1                               0.0s
 => => naming to docker.io/library/softy-pinko-back-end:task5                                                              0.0s
[+] Building 1.1s (8/8) FINISHED                                                                                                
 => [internal] load build definition from Dockerfile                                                                       0.0s
 => => transferring dockerfile: 188B                                                                                       0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                            0.9s
 => [1/3] FROM docker.io/library/nginx:latest@sha256:593dac25b7733ffb7afe1a72649a43e574778bf025ad60514ef40f6b5d606247      0.0s
 => [internal] load build context                                                                                          0.0s
 => => transferring context: 2.68MB                                                                                        0.0s
 => CACHED [2/3] COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end                                          0.0s
 => CACHED [3/3] COPY ./softy-pinko-front-end.conf  /etc/nginx/conf.d/default.conf                                         0.0s
 => exporting to image                                                                                                     0.0s
 => => exporting layers                                                                                                    0.0s
 => => writing image sha256:7c073606697aa4578af1ddad5f2210e611f5aa0762175006d0992b637c1de081                               0.0s
 => => naming to docker.io/library/softy-pinko-front-end:task5                                                             0.0s
[+] Building 0.4s (7/7) FINISHED                                                                                                
 => [internal] load build definition from Dockerfile                                                                       0.0s
 => => transferring dockerfile: 31B                                                                                        0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                            0.3s
 => [internal] load build context                                                                                          0.0s
 => => transferring context: 32B                                                                                           0.0s
 => [1/2] FROM docker.io/library/nginx:latest@sha256:593dac25b7733ffb7afe1a72649a43e574778bf025ad60514ef40f6b5d606247      0.0s
 => CACHED [2/2] COPY ./proxy.conf /etc/nginx/conf.d/default.conf                                                          0.0s
 => exporting to image                                                                                                     0.0s
 => => exporting layers                                                                                                    0.0s
 => => writing image sha256:bc5dd1e5f2e15577273ef909c555c32b7dfd892c5c6f07f1d50a8519a4c99f32                               0.0s
 => => naming to docker.io/library/softy-pinko-proxy:task5                                                                 0.0s
Dereks-MacBook-Pro:task5 derekwebb$ docker-compose up
[+] Running 3/0
 ⠿ Container task5-back-end-1   Created                                                                                    0.0s
 ⠿ Container task5-front-end-1  Created                                                                                    0.0s
 ⠿ Container task5-proxy-1      Created                                                                                    0.0s
Attaching to task5-back-end-1, task5-front-end-1, task5-proxy-1
task5-back-end-1   |  * Serving Flask app 'api'
task5-back-end-1   |  * Debug mode: off
task5-back-end-1   | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
task5-back-end-1   |  * Running on all addresses (0.0.0.0)
task5-back-end-1   |  * Running on http://127.0.0.1:5252
task5-back-end-1   |  * Running on http://172.19.0.2:5252
task5-back-end-1   | Press CTRL+C to quit
task5-front-end-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
task5-front-end-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
task5-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
task5-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
task5-front-end-1  | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
task5-front-end-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
task5-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
task5-front-end-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
task5-front-end-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: using the "epoll" event method
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: nginx/1.25.1
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: OS: Linux 5.15.49-linuxkit
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker processes
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 28
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 29
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 30
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 31
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 32
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 33
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 34
task5-front-end-1  | 2023/06/15 19:28:38 [notice] 1#1: start worker process 35
task5-proxy-1      | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
task5-proxy-1      | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
task5-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
task5-proxy-1      | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
task5-proxy-1      | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
task5-proxy-1      | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
task5-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
task5-proxy-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
task5-proxy-1      | /docker-entrypoint.sh: Configuration complete; ready for start up
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: using the "epoll" event method
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: nginx/1.25.1
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: OS: Linux 5.15.49-linuxkit
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker processes
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 28
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 29
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 30
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 31
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 32
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 33
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 34
task5-proxy-1      | 2023/06/15 19:28:39 [notice] 1#1: start worker process 35
task5-back-end-1   | 172.19.0.4 - - [15/Jun/2023 19:28:48] "GET /api/hello HTTP/1.0" 200 -
task5-front-end-1  | 2023/06/15 19:28:49 [error] 30#30: *27 open() "/var/www/html/softy-pinko-front-end/favicon.ico" failed (2: No such file or directory), client: 172.19.0.4, server: softy-pinko-front-end, request: "GET /favicon.ico HTTP/1.0", host: "front-end:9000", referrer: "http://localhost/"
```

L'image suivante montre le navigateur accédant à `http://localhost:80`, qui est le serveur proxy.

**Navigateur**

Ensuite, remarquez que vous ne pouvez pas accéder directement aux serveurs front-end ou back-end - vous ne pouvez atteindre vos services qu'en passant par le serveur proxy.

**Navigateur**

**Repo :**

- GitHub repository : holbertonschool-softy-pinko-docker
- Directory : task5

---

### Explication détaillée

1. **Serveur Proxy** : Un serveur proxy agit comme un intermédiaire entre le client et les serveurs back-end. Il permet de centraliser les requêtes et de masquer la complexité des serveurs sous-jacents.

2. **Nginx** : Nginx est un serveur web et un proxy inverse très performant. Il est utilisé ici pour rediriger les requêtes vers les services appropriés (front-end ou back-end).

3. **Docker Compose** : Docker Compose permet de gérer plusieurs conteneurs Docker dans un seul fichier de configuration. Ici, nous ajoutons un service `proxy` pour gérer les requêtes entrantes.

4. **Redirection des requêtes** : Le fichier `proxy.conf` configure Nginx pour rediriger les requêtes vers les services appropriés. Les requêtes vers `/` sont redirigées vers le front-end, et les requêtes vers `/api` sont redirigées vers le back-end.

5. **JavaScript** : Le code JavaScript est modifié pour faire des requêtes via le serveur proxy plutôt que directement vers le back-end. Cela permet de centraliser toutes les communications via le proxy.

6. **Sécurité** : En ne mappant pas les ports front-end et back-end sur la machine hôte, nous limitons l'accès direct à ces services, ce qui renforce la sécurité.

7. **DNS interne de Docker** : Docker utilise un DNS interne pour résoudre les noms de services en adresses IP, ce qui simplifie la configuration et la gestion des services.

En suivant ces étapes, vous avez créé un environnement où toutes les requêtes passent par un serveur proxy, ce qui simplifie la gestion et améliore la sécurité.
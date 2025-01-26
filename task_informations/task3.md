# 3. Connecting the Front-end and Back-end

#### **Tâche 3 : Connecter le Front-end et le Back-end**

Pour commencer cette tâche, faites une copie de votre répertoire `task2` et nommez-le `task3`.

Cette tâche consiste à connecter votre front-end au back-end, ce qui vous permettra d'afficher des données dynamiques sur votre front-end. Cela signifie que la communication se fera entre vos deux images Docker (chacune fonctionnant dans son propre conteneur Docker). Pour faciliter cela, assurez-vous d'avoir plusieurs instances de terminal ouvertes afin de pouvoir exécuter un conteneur Docker dans chaque terminal.

---

### **Instructions**

#### **Étape 1 : Préparer le Front-end**
Nous devons ajouter quelques éléments au front-end pour qu'il puisse communiquer avec le back-end.

1. **Ajouter un nouveau HTML :**
   - Ajoutez le code HTML suivant avant le `<h1>` qui contient le texte **"We provide the best strategy to grow up your business"** dans le fichier **`index.html`** :
     ```html
     <h1 id="dynamic-content"></h1>
     ```

   **Exemple de `index.html` :**
   ```html
   <!-- . . . SOME HTML BEFORE . . . -->
   <div class="offset-xl-3 col-xl-6 offset-lg-2 col-lg-8 col-md-12 col-sm-12">
       <h1 id="dynamic-content"></h1>
       <h1>We provide the best <strong>strategy</strong><br>to grow up your<strong>business</strong></h1>
       <a href="#features" class="main-button-slider">Discover More</a>
   </div>
   <!-- . . . SOME HTML AFTER . . . -->
   ```

   - Le nouveau `<h1>` avec l'ID **`dynamic-content`** est l'endroit où nous allons insérer les données dynamiques provenant de notre serveur API.

2. **Ajouter un script JavaScript :**
   - Placez le script suivant près de la fin du HTML, juste avant la balise fermante **`</body>`** :
     ```html
     <script>
         // Charger les données dynamiques du back-end sur le port 5252
         $(function() {
             $.ajax({
                 type: "GET",
                 url: "http://localhost:5252/api/hello",
                 success: function(data) {
                     console.log(data);
                     $('#dynamic-content').text(data);
                 }
             });
         });
     </script>
     ```

   - Ce script effectue une requête AJAX vers le back-end pour récupérer les données et les afficher dans l'élément **`dynamic-content`**.

---

#### **Étape 2 : Modifier le Back-end pour accepter les requêtes CORS**
Pour permettre au front-end de communiquer avec le back-end, nous devons utiliser le plugin **CORS** pour Flask.

1. **Modifier le Dockerfile du back-end :**
   - Dans le fichier **`back-end/Dockerfile`**, installez **`flask-cors`** après avoir installé **`flask`** :
     ```Dockerfile
     RUN pip3 install flask-cors
     ```

2. **Mettre à jour le fichier `api.py` :**
   - Utilisez le script Python suivant pour votre fichier **`api.py`** :
     ```python
     from flask import Flask
     from flask_cors import CORS

     app = Flask(__name__)
     CORS(app)

     @app.route('/api/hello')
     def hello_world():
         return 'Hello, World!'

     if __name__ == '__main__':
         app.run(host='0.0.0.0', port=5252)
     ```

   - **`CORS(app)`** permet au back-end d'accepter les requêtes provenant de domaines différents (comme le front-end).

---

#### **Étape 3 : Construire et exécuter les conteneurs**
1. **Construire et exécuter le back-end :**
   - Dans un terminal, construisez et exécutez le conteneur du back-end :
     ```bash
     docker build -f ./back-end/Dockerfile -t softy-pinko-back-end:task3 ./back-end
     docker run -p 5252:5252 -it --rm --name softy-pinko-back-end-task3 softy-pinko-back-end:task3
     ```

2. **Construire et exécuter le front-end :**
   - Dans un autre terminal, construisez et exécutez le conteneur du front-end :
     ```bash
     docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task3 ./front-end
     docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task3 softy-pinko-front-end:task3
     ```

---

### **Exemple de sortie**
Voici un exemple de ce à quoi votre sortie pourrait ressembler (cela peut varier en fonction de votre environnement local et de la présence de données en cache) :

#### **Terminal 1 : Back-end**
```bash
Dereks-MacBook-Pro:task3 derekwebb$ docker build -f ./back-end/Dockerfile -t softy-pinko-back-end:task3 ./back-end
[+] Building 1.2s (13/13) FINISHED                                                                                                                                                                                                
 => [internal] load build definition from Dockerfile                                                                                                                                                                         0.0s
 => => transferring dockerfile: 37B                                                                                                                                                                                          0.0s
 => [internal] load .dockerignore                                                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                                                             1.1s
 => [1/8] FROM docker.io/library/ubuntu:latest@sha256:ac58ff7fe25edc58bdf0067ca99df00014dbd032e2246d30a722fa348fd799a5                                                                                                       0.0s
 => [internal] load build context                                                                                                                                                                                            0.0s
 => => transferring context: 28B                                                                                                                                                                                             0.0s
 => CACHED [2/8] RUN apt-get update                                                                                                                                                                                          0.0s
 => CACHED [3/8] RUN apt-get upgrade -y                                                                                                                                                                                      0.0s
 => CACHED [4/8] RUN apt-get install -y python3 python3-pip                                                                                                                                                                  0.0s
 => CACHED [5/8] RUN pip3 install flask                                                                                                                                                                                      0.0s
 => CACHED [6/8] RUN pip3 install flask-cors                                                                                                                                                                                 0.0s
 => CACHED [7/8] WORKDIR /app                                                                                                                                                                                                0.0s
 => CACHED [8/8] COPY ./api.py /app/api.py                                                                                                                                                                                   0.0s
 => exporting to image                                                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                                                      0.0s
 => => writing image sha256:72111d9280c3fb3d875aa956a9d5eb407adc55256e667ec3e30dcadd128fd073                                                                                                                                 0.0s
 => => naming to docker.io/library/softy-pinko-back-end:task3                                                                                                                                                                0.0s
Dereks-MacBook-Pro:task3 derekwebb$ docker run -p 5252:5252 -it --rm --name softy-pinko-back-end-task3 softy-pinko-back-end:task3
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5252
 * Running on http://172.17.0.2:5252
Press CTRL+C to quit
```

#### **Terminal 2 : Front-end**
```bash
Dereks-MacBook-Pro:task3 derekwebb$ docker build -f ./front-end/Dockerfile -t softy-pinko-front-end:task3 ./front-end
[+] Building 1.2s (8/8) FINISHED                                                                                                                                                                                                  
 => [internal] load build definition from Dockerfile                                                                                                                                                                         0.0s
 => => transferring dockerfile: 37B                                                                                                                                                                                          0.0s
 => [internal] load .dockerignore                                                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                                                                                                              1.0s
 => CACHED [1/3] FROM docker.io/library/nginx:latest@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305                                                                                                 0.0s
 => [internal] load build context                                                                                                                                                                                            0.0s
 => => transferring context: 34.59kB                                                                                                                                                                                         0.0s
 => [2/3] COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end                                                                                                                                                   0.0s
 => [3/3] COPY ./softy-pinko-front-end.conf  /etc/nginx/conf.d/default.conf                                                                                                                                                  0.0s
 => exporting to image                                                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                                                      0.0s
 => => writing image sha256:f981f38fb967b1a4d477ce370ed52e1fcd97e1dc4863baf1faf47e908bec4057                                                                                                                                 0.0s
 => => naming to docker.io/library/softy-pinko-front-end:task3                                                                                                                                                               0.0s
Dereks-MacBook-Pro:task3 derekwebb$ docker run -p 9000:9000 -it --rm --name softy-pinko-front-end-task3 softy-pinko-front-end:task3
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/06/12 19:10:39 [notice] 1#1: using the "epoll" event method
2023/06/12 19:10:39 [notice] 1#1: nginx/1.25.0
2023/06/12 19:10:39 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2023/06/12 19:10:39 [notice] 1#1: OS: Linux 5.15.49-linuxkit
2023/06/12 19:10:39 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/06/12 19:10:39 [notice] 1#1: start worker processes
2023/06/12 19:10:39 [notice] 1#1: start worker process 28
2023/06/12 19:10:39 [notice] 1#1: start worker process 29
2023/06/12 19:10:39 [notice] 1#1: start worker process 30
2023/06/12 19:10:39 [notice] 1#1: start worker process 31
2023/06/12 19:10:39 [notice] 1#1: start worker process 32
2023/06/12 19:10:39 [notice] 1#1: start worker process 33
2023/06/12 19:10:39 [notice] 1#1: start worker process 34
2023/06/12 19:10:39 [notice] 1#1: start worker process 35
```

---

### **Résultat attendu**
Lorsque vous accédez à l'application front-end à l'adresse `http://127.0.0.1:9000`, vous devriez voir le texte **"Hello, World!"** affiché dans l'élément **`dynamic-content`**.

---

### **Dépôt GitHub**
- **Dépôt :** [holbertonschool-softy-pinko-docker](https://github.com/holbertonschool/holbertonschool-softy-pinko-docker)  
- **Répertoire :** `task3`  

---

### **Conclusion**
Ce tutoriel vous a guidé à travers la connexion du front-end et du back-end en utilisant Docker. Vous avez appris à :

1. Ajouter des éléments HTML et JavaScript pour afficher des données dynamiques.
2. Configurer le back-end pour accepter les requêtes CORS.
3. Construire et exécuter des conteneurs Docker pour le front-end et le back-end.

Ces compétences sont essentielles pour créer des applications web modernes et interactives.
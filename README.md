# holbertonschool-softy-pinko-docker

#### **Docker**
**Une revue manuelle QA doit être effectuée (demandez-la lorsque vous avez terminé le projet)**

---

### **Description**

#### **Contexte**
Docker est une plateforme qui permet de conteneuriser vos applications, c'est-à-dire de les empaqueter dans des environnements portables et autonomes qui peuvent fonctionner n'importe où. Cela signifie que vous pouvez déplacer rapidement votre application d'un environnement à un autre, comme de votre ordinateur local à un serveur, sans vous soucier des dépendances ou des problèmes de configuration. Docker y parvient en utilisant des conteneurs, qui sont des environnements isolés contenant tout ce dont une application a besoin pour fonctionner, comme les bibliothèques, les dépendances et les configurations. Les conteneurs Docker sont légers et peuvent être démarrés et arrêtés rapidement, ce qui les rend idéaux pour le développement et le déploiement modernes de logiciels. Avec Docker, vous pouvez également rapidement mettre à l'échelle votre application en exécutant plusieurs conteneurs de la même application sur différents hôtes (ou le même hôte, comme nous le ferons dans ce projet) et les gérer à l'aide de Docker Compose ou d'autres outils d'orchestration.

En fin de compte, ce que vous allez créer dans ce projet est une infrastructure pour une application qui utilise un proxy inverse, un répartiteur de charge (load balancer), deux serveurs d'application et un serveur frontal.

Considérons le design suivant :

---

### **Design de haut niveau**

Dans ce design, il y a un seul serveur qui agit comme point d'entrée pour votre application. Ce serveur agit comme un serveur proxy inverse, qui achemine le trafic soit vers les serveurs API, soit vers le serveur de contenu statique frontal. Il agit également comme un répartiteur de charge pour équilibrer le trafic entre les deux serveurs API. Lorsque le trafic arrive, le serveur déterminera à quel service il doit être acheminé (soit le serveur de contenu statique frontal, soit le serveur API) et :

- Si la demande doit être acheminée vers le serveur de contenu statique frontal, il le fera, et tout contenu statique renvoyé par le serveur frontal au serveur proxy sera ensuite envoyé au client. Le client ne communique pas directement avec le serveur de contenu statique frontal.

- Si la demande doit être acheminée vers le serveur API, elle passera d'abord par un algorithme de répartition de charge pour déterminer à quel serveur API envoyer la demande. Nous utiliserons l'algorithme de répartition de charge Round Robin de base pour notre site. Une fois que la demande est acheminée vers un serveur API et que la réponse est renvoyée au serveur proxy, elle sera ensuite envoyée au client. Le client ne communique pas directement avec l'un des serveurs API.

---

### **Qu'est-ce que la répartition de charge Round Robin ?**

La répartition de charge Round Robin est une méthode de distribution du trafic sur plusieurs serveurs ou nœuds dans un réseau. Dans cette approche, le répartiteur de charge attribue les demandes à chaque serveur de manière rotative, ce qui signifie que chaque serveur prend son tour pour servir les demandes. Par exemple, s'il y a trois serveurs A, B et C, la première demande sera envoyée au serveur A, la deuxième au serveur B, la troisième au serveur C, la quatrième à A, la cinquième à B, et ainsi de suite, tandis que le cycle se répète. Cela garantit que chaque serveur reçoit une part égale du trafic et peut empêcher qu'un serveur ne soit submergé par les demandes. La répartition de charge Round Robin est une méthode simple et efficace pour distribuer le trafic, mais elle peut ne pas convenir à toutes les applications, en particulier celles ayant des modèles de charge de travail variables ou des exigences de ressources différentes. Pour nos besoins d'apprentissage dans ce projet, c'est un algorithme suffisant à implémenter pour nos besoins de répartition de charge.

---

### **Ce dont vous avez besoin avant de commencer**

1. **Installez Docker Desktop sur votre ordinateur local (pas sur votre sandbox)**  
   - [Site officiel de Docker](https://www.docker.com/)  
   - Pour des instructions d'installation plus détaillées, consultez :  
     - [Windows](https://docs.docker.com/desktop/install/windows-install/)  
     - [Mac](https://docs.docker.com/desktop/install/mac-install/)  
     - [Linux](https://docs.docker.com/desktop/install/linux-install/)  
     - [Chromebook](https://docs.docker.com/engine/install/) - Remarque : il s'agit du moteur Docker, mais pas de Docker Desktop. Il n'est pas clair à quel point cela fonctionnera bien sur un Chromebook. Vous pouvez préférer utiliser une machine Windows, Mac ou Linux, ou une machine sur le campus.

2. **Familiarisez-vous avec Docker**  
   - [Tutoriel Docker](https://docs.docker.com/get-started/)  

---

### **Ressources utiles**

- [Docker Cheatsheet](https://dockerlabs.collabnix.com/docker/cheatsheet/)  
- [Proxy vs Reverse Proxy (Exemples réels)](https://www.nginx.com/resources/glossary/reverse-proxy-vs-forward-proxy/)  
- [Qu'est-ce qu'un Reverse Proxy ? (vs. Forward Proxy) | Explication des serveurs proxy (Arrêtez-vous à 06:25)](https://www.youtube.com/watch?v=4NB0NDtOwIQ)  

---

### **Synthèse complète**

#### **Contexte et objectifs**
Ce projet vise à vous initier à Docker en vous permettant de créer une infrastructure pour une application moderne. L'objectif est de comprendre comment Docker peut être utilisé pour conteneuriser des applications, les rendre portables et faciliter leur déploiement dans différents environnements.

#### **Concepts clés**
1. **Conteneurisation** :  
   - Les conteneurs sont des environnements isolés qui contiennent tout ce dont une application a besoin pour fonctionner (bibliothèques, dépendances, configurations).  
   - Ils sont légers, rapides à démarrer et à arrêter, et peuvent fonctionner sur n'importe quelle machine disposant de Docker.

2. **Proxy inverse** :  
   - Un proxy inverse est un serveur qui reçoit les demandes des clients et les redirige vers les serveurs appropriés (API ou frontal).  
   - Il masque l'infrastructure interne et améliore la sécurité.

3. **Répartition de charge (Load Balancing)** :  
   - Le répartiteur de charge distribue les demandes entre plusieurs serveurs pour équilibrer la charge et éviter la surcharge d'un seul serveur.  
   - L'algorithme Round Robin est utilisé ici pour distribuer les demandes de manière équitable.

4. **Infrastructure proposée** :  
   - **1 serveur proxy inverse** : Point d'entrée unique pour l'application.  
   - **2 serveurs API** : Gèrent les demandes d'API.  
   - **1 serveur frontal** : Sert le contenu statique (HTML, CSS, JS).  

#### **Étapes du projet**
1. **Installation de Docker** :  
   - Installez Docker Desktop sur votre machine locale.  
   - Familiarisez-vous avec les commandes de base de Docker.

2. **Création des conteneurs** :  
   - Créez des conteneurs pour le proxy inverse, les serveurs API et le serveur frontal.  
   - Utilisez Docker Compose pour orchestrer les conteneurs.

3. **Configuration du proxy inverse et du load balancer** :  
   - Configurez le proxy inverse pour rediriger les demandes vers les serveurs API ou le serveur frontal.  
   - Implémentez l'algorithme Round Robin pour équilibrer la charge entre les serveurs API.

4. **Test et déploiement** :  
   - Testez l'infrastructure localement.  
   - Déployez l'application sur un serveur distant si nécessaire.

#### **Mots-clés à retenir**
| Terme               | Définition                                                                 |
|---------------------|----------------------------------------------------------------------------|
| **Conteneur**       | Environnement isolé contenant une application et ses dépendances.          |
| **Proxy inverse**   | Serveur qui redirige les demandes vers les serveurs appropriés.            |
| **Load Balancer**   | Répartiteur de charge qui équilibre le trafic entre plusieurs serveurs.    |
| **Round Robin**     | Algorithme de répartition de charge qui distribue les demandes de manière rotative. |

---

### **Résumé**
Ce projet vous permet de comprendre les bases de Docker et de créer une infrastructure moderne pour une application. Vous apprendrez à conteneuriser des services, à configurer un proxy inverse et un répartiteur de charge, et à orchestrer le tout avec Docker Compose. Ces compétences sont essentielles pour le développement et le déploiement d'applications modernes.
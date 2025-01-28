FROM ubuntu:latest

# Mettre à jour le système et installer Python et pip
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3 python3-pip python3-venv

# Créer un environnement virtuel
RUN python3 -m venv /app/venv

# Activer l'environnement virtuel et installer les packages
RUN /app/venv/bin/pip install flask flask-cors

# Copier le fichier de l'application
WORKDIR /app
COPY api.py /app/api.py

# Définir la commande pour exécuter l'application
CMD ["/app/venv/bin/python3", "api.py"]
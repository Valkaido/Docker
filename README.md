# TP1 - Application monitorée avec Docker

## Résumé du projet

## Pré-requis
* python
* venv
* flask

## Configuration

### Configuration docker-compose.yml
```
$  flaskapp:
$    build: .
$    ports:
$      - "5000:5000"
```

## Installation

### Installation manuelle de la flaskapp
Cloner le repo, créer un environnement virtuel en suivant ces étapes et installez-y flask :
```
$ python3 -m venv .venv
$ . .venv/bin/activate
$ pip install flask
```

Une fois crée, vous pouvez exécuter l'application avec la commande suivante :
```
$ flask --app app run
```

### Création de l'image Docker et exécution de la flaskapp depuis un conteneur
Pour exécuter le projet via le dockerfilem il est nécessaire de constuire l'image de cette dernière. Pour ce faire, entrer la commande suivante :
```
$ docker build --tag flaskapp .
```

Une fois l'image crée, on peut vérifier avec `docker images` que cette dernière existe bel et bien avant de démarrer notre application avec :
```
$ docker run -d -p 5000:5000 flaskapp:latest
```

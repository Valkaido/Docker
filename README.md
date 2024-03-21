# TP1 - Application monitorée avec Docker

## Résumé du projet
La société Dock In Space souhaite moderniser son application existante réalisée de manière monolithique dans son datacenter par une nouvelle application en utilisant des conteneurs Docker.

Pour ce faire, elle aimerait qu'un prototype d'application voit le jour avec les critères suivants :
- Une application conteneurisée personnelle, avec flask et python.
- Un conteneur exécutant l'application Prometheus, permettant de logger les metrics de l'application précédentes.
- Un conteneur exécutant l'application Grafana, pour créer des dashboards à partir des logs collectés

Pour la réalisation de ce projet, nous avons utilisé des éléments nécessaires à l'application pour fonctionner. Dans un but d'extension future, le fichier "requirements.txt" possède des paquets en plus, non utilisés actuellement, mais qui pourraient voir un usage éventuel à l'avenir.

Cependant, ce fichier possède notamment des paquets nécessaies, comme flask ou python, qu'il est urgent de ne pas supprimer.


## Pré-requis
Ces logiciels sont des pré-requis nécessaires à l'utilisation de notre application :
* docker
* docker compose

En plus de ces derniers, ces pré-requis sont nécessaires lors de la réalisation étapes par étapes du projet :
* python
* venv
* flask


## Configuration

### Configuration docker-compose.yml
L'extrait de code qui nous permet d'utiliser notre dockerfile et de générer notre image docker à utiliser avec l'application. Vous pouvez y changer les ports à ouvrir si besoin.

```
  flaskapp:
    build: .
    ports:
      - "5000:5000"
```

### Configuration prometheus.yml 
Au sein du répertoire "prometheus" se trouve le fichier de configuration de Prometheus. Vous pouvez y éditer les intervalles de scraping suivant votre besoin, sur quel application réaliser ce scrapping, de la gestion d'alerte afin de monitorer votre application.

### Configuration datasource.yml
Au sein du répertoire "grafana" se trouve le fichier de configuration des datasources. Comme nous n'avons que Prometheus, il n'est pas nécessaire de le modifier. Cependant, il est parfaitement possible d'étendre ce fichier en fonction de votre besoin.

### Configuration grafana.ini
Enfin, au sein du répertoire "grafana" se trouve un autre fichier de configuration, lui intéragissant avec l'application Grafana. Vous pouvez y changer les ports le domaine, l'utilisateur admin par défaut (faire attention, le mot de passe y est inscrit en clair) selon vos besoins.


## Utilisation de l'application
Pour utiliser l'application, il suffit d'exécuter la commande suivante au sein de la racine du projet (dans notre cas, `~/Docker`) :
```
$ docker compose up -d
```

L'exécution de cette commande permet à la lecture du fichier YAML d'exécuter nos applications conteneurisées. Fait intéressant, le suffixe `-d` indique à l'interpréteur de commandes de s'exécuter en mode détaché, ce qui permet d'utiliser son terminal après exécution.

Il reste néanmoins possible d'exécuter le logiciel sans ce flag, ce qui vous permettra de logger en temps réel le fonctionnement de l'application.


## Réalisation du TP étapes par étapes
Dans le cadre d'un exercice noté de cours, voici une description de la procédure suivie à la réalisation de ce projet.

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

### Création du Docker compose
Pour lier tous les éléments ensemble, il aura fallu éditer le code app.py pour ajouter les éléments qui permettent de trigger les metrics de Prometheus. 

Suite à cela, nous avons procédé à la création de deux répertoires : "Grafana" et "Prometheus" contenant des fichiers de configuration relatifs à ces outils, que nous avons par la suite liés à des volumes Docker de sorte à injecter les fichiers de configuration à l'intérieur du conteneur. Cela nous permet d'avoir deux hôtes conteneurisant ces applications.

Une partie a été ajoutée en fin de fichier pour utiliser notre propre image docker relative à notre flaskapp. Les configurations de grafana et de prometheus ont été également modifiées pour supporter les metrics de l'application.

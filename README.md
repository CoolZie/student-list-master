**Mini Projet Docker – Deploiement de l'application POZO**

Lien de l'exercice et code source :

Tâches :

1. Build et test de l'api dans le dossier sample-api
2. Infrastructure as code docker compose de deploiement de la solution globale api + front end
3. Mise en place du registry local avec interface graphique
4. Rendu des livrables

Execution du Mini Projet

1. **Build et test de l'api dans le dossier sample-api**

DockerFile de build de l'api sample-api
FROM python:3.8-buster
LABELname="Sidiki"
RUN apt update-y && apt install python-devpython3-devlibsasl2-devpython-devlibldap2-devlibssl-dev-y
RUN pip installflask==2.0.0flask\_httpauth==4.1.0 flask\_simpleldap python-dotenv==0.14.0
COPY./requirements.txt/requirements.txt
COPY./student\_age.py/student\_age.py
RUN pip3install-r/requirements.txt
EXPOSE5000
CMD["python","./student\_age.py"]

Je me deplace dans le dossier simple-api et je fait la commande :

**docker build -t simple-api .** pour build l'image simple api

Ensuite je fais la commande

**docker run -d -p 8000:5000 --name simple-api-test -v ./student\_age.json:/data/student\_age.json simple-api**

Ce qui va demarrer un conteneur simple-api-test sur le port 8000 en local et 5000 sur le conteneur docker avec un volume de type bind monté sur conteneur dans le dossier /data .

Test de l'api avec la commande curl

curl -u toto:python -X GET http://localhost:8000/pozos/api/v1.0/get\_student\_ages

![](RackMultipart20240131-1-yysz6z_html_570e2b2b93a13372.png)

1. **Infrastructure as code docker compose de deploiement de la solution globale api + front end**

Docker-compose.yml à la racine du dossier

version: '3.8'
services:
   website:
      image: php:apache
      environment:
         - USERNAME=toto
         - PASSWORD=python
         - API\_PORT=5000
         - HOST\_API=api
      volumes:
         - ./website:/var/www/html
      ports:
         - "8080:80"
      networks:
        - app\_net
   api:
      build: ./simple\_api
      ports:
         - "81:5000"
      volumes:
         - ./simple\_api/student\_age.json:/data/student\_age.json
      networks:
         - app\_net
      container\_name: api
networks:
   app\_net:
   driver: bridge

Modification du fichier **index.php** dans le dossier webiste

![](RackMultipart20240131-1-yysz6z_html_79397d2bea90e918.png)

![](RackMultipart20240131-1-yysz6z_html_e3195239d6e08e7d.png)

Build du docker-compose avec la commande **docker-compose up**

Vérification des conteneurs en marche **docker ps**

![](RackMultipart20240131-1-yysz6z_html_22d47992db17280c.png)

Verification et test de l'application deployée

![](RackMultipart20240131-1-yysz6z_html_46b1e3f1a2e28d3f.png)

1. **Mise en place du registry local avec interface graphique**

Creation d'un dossier docker registry et creation d'un docker-compose.yml donc le deployment du registry et de UI

version: '3.8'
services:
registry-ui:
   image: joxit/docker-registry-ui
   restart: always
   ports:
      - 8089:80
   environment:
      - REGISTRY\_URL=http://localhost:5001
      - REGISTRY\_TITLE=Docker Registry UI
      - DELETE\_IMAGES=true
      - REGISTRY\_HTTP\_HEADERS\_Access-Control-Allow-Origin="['http://localhost:8089']"
   container\_name: registry-ui
networks:
   - registry-network
registry-server:
i  mage: registry:2.8.2
   restart: always
   ports:
      - 5001:5000
   volumes:
      - ./registry/data:/var/lib/registry
   container\_name: registry-server
   networks:
      - registry-network
networks:
   registry-network:
driver: bridge

**cd docker-registry && docker-compose up**

**docker ps**

![](RackMultipart20240131-1-yysz6z_html_63581577a7b8a62a.png)

![](RackMultipart20240131-1-yysz6z_html_f427b8294184c9a.png)

Tag et push d'une image sur le docker hub local

![](RackMultipart20240131-1-yysz6z_html_edcfe32094a94636.png)

![](RackMultipart20240131-1-yysz6z_html_e7ed8bdfd2b8d2c3.png)

![](RackMultipart20240131-1-yysz6z_html_578bf7829fdb8767.png)
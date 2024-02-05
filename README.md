# Mini Projet Docker – Deploiement de l'application POZO

Ce mini projet est le resultat de la formation sur docker du bootcamp DevOps session 17 .
**Tâches :**

1. Build et test de l'api dans le dossier sample-api
2. Infrastructure as code docker compose de deploiement de la solution globale api + front end
3. Mise en place du registry local avec interface graphique
4. Rendu des livrables


## 1. **Build et test de l'api dans le dossier sample-api**

#### Création du dockerfile
> 

Dans le dossier simple-api se trouve le Dockerfile de la creation de l'image de l'api .[ici](https://github.com/CoolZie/student-list-master/blob/main/simple_api/Dockerfile)


#### Build et run de l'image api pour test 
`docker build -t simple-api .` qui va créer une image simple-api dans le docker local 

`docker run -d -p 8000:5000 --name simple-api-test -v ./student\_age.json:/data/student\_age.json simple-api` qui va lancer un conteneur docker sur l'image simple-api en montant un volume bind dans le dossier /data du conteneur sur le pour 8000 en externe et 5000 en interne .
`curl -u toto:python -X GET http://localhost:8000/pozos/api/v1.0/get\_student\_ages`

## 2. **Infrastructure as code docker compose de deploiement de la solution globale api + front end**

### docker-compose.yml 
> Voir doker-compose.yml à la racine du repository [ici](https://github.com/CoolZie/student-list-master/blob/main/docker-compose.yml)

### Modification du fichier index.php

    $username = getenv('USERNAME');
    $password = getenv('PASSWORD');
    $port = getenv('API_PORT');
    $api_ip_or_name = getenv('HOST_API');
    if ( empty($username) ) $username = 'fake_username';
    if ( empty($password) ) $password = 'fake_password';
    $context = stream_context_create(array(
         "http" => array(
               "header" => "Authorization: Basic " .base64_encode("$username:$password"), )));
	$url = 'http://'.$api_ip_or_name.':'.$port.'/pozos/api/v1.0/get_student_ages';
    $list = json_decode(file_get_contents($url, false, $context), true);


You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

### Build du docker-compose

    docker-compose up
    docker ps


## 3. **Mise en place du registry local avec interface graphique**

### Création d'un dossier docker-registry avec un docker-compose.yml retrouver [ici](https://github.com/CoolZie/student-list-master/blob/main/docker-registry/docker-compose.yml)
Tout en etant dans le dossier docker-registry faire les commandes suivantes
  > docker-compose up
    docker ps

docker tag student-list-master-api localhost:5001/student-list-master-api
### Tag de l'image générée par le docker-compose up

> docker tag student-list-master-api localhost:5001/student-list-master-api

### Push de l'image sur le docker register privé local 

> docker push localhost:5001/student-list-master-api



##### By Sidiki Coulibaly

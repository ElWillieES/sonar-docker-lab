# Sonar Docker Lab

![Docker](https://img.shields.io/badge/Docker-2496ED?&style=flat&logo=docker&logoColor=ffffff)&nbsp;
![SonarQube](https://img.shields.io/badge/Sonarqube-5190cf?style=flat&logo=sonarqube&logoColor=white)&nbsp;
![Postgresql](https://img.shields.io/badge/Postgresql-FFFFFF?style=flat&logo=postgresql&logoColor=316192)&nbsp;

Más ejemplos de Badges en [markdown-badges](https://ileriayo.github.io/markdown-badges/), [Badges 4 README.md Profile](https://github.com/alexandresanlim/Badges4-README.md-Profile) y [Awesome Badges](https://github.com/Envoy-VC/awesome-badges)

## Introducción

Este repo se ha creado para complementar los Posts sobre [SonarQube](https://elwillie.es/tag/sonarqube/) del Blog [El Willie - The Geeks invaders](https://elwillie.es/), y comparte un pequeño laboratorio en formato de Docker Compose, con el que podemos arrancar los siguientes componentes:

* **PostgreSQL** como base de datos para Sonar, utilizando un volumen Docker.
* **SonarQube Communiry**, utilizando volúmenes Docker. Depende de PostreSQL.

De esta forma, podemos arrancar este laboratorio y trastear con Sonar de forma rápida y sencilla gracias a Docker, pero con volúmenes que nos ofrezcan persistencia.

Algunos de los Posts de Sonar en los que se basa este Docker Compose son:

* [SonarQube – Introducción e instalación de SonarQube, SonarScanner CLI, y SonarLint](https://elwillie.es/2022/12/11/sonarqube-introduccion-e-instalacion-de-sonarqube-sonarscanner-cli-y-sonarlint/)
* [Python – Exportando datos de Sonar con sonar-exporter](https://elwillie.es/2023/04/03/python-exportando-datos-de-sonar-con-sonar-exporter/)

**Puedes apoyar mi trabajo siguiéndome, haciendo "☆ Star" en el repo, o nominarme a "GitHub Star"**. Muchas gracias :-) 

[![GitHub Star](https://img.shields.io/badge/GitHub-Nominar_a_star-yellow?style=for-the-badge&logo=github&logoColor=white&labelColor=101010)](https://stars.github.com/nominate/)

En mi Blog personal ([El Willie - The Geeks invaders](https://elwillie.es)) y en mi perfil de GitHub, encontrarás más información sobre mi, y sobre los contenidos de tecnología que comparto con la comunidad.

[![Web](https://img.shields.io/badge/GitHub-ElWillieES-14a1f0?style=for-the-badge&logo=github&logoColor=white&labelColor=101010)](https://github.com/ElWillieES)

# Variable de Entorno SONAR_ES_BOOTSTRAP_CHECKS_DISABLE y el error "vm.max_map_count [65530] is too low"


Al arrancar Sonar, se realizan varios chequeos, y un error habitual que nos podemos encontrar es el siguiente:

```
...
sonar-docker-lab-sonarqube-1  | ERROR: [1] bootstrap checks failed. You must address the points described in the following [1] lines before starting Elasticsearch.
sonar-docker-lab-sonarqube-1  | bootstrap check failure [1] of [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
sonar-docker-lab-sonarqube-1  | ERROR: Elasticsearch did not exit normally - check the logs at /opt/sonarqube/logs/sonarqube.log
...
```

Esto se puede solucionar principalmente de dos formas:

* **La manera más sencilla**. Evitar esos chequeos, estableciendo la variable de entorno **SONAR_ES_BOOTSTRAP_CHECKS_DISABLE a true**, que es lo que hacemos en los ejemplos de este Post (tanto con Docker como con Docker Compose). **No es recomendable**, pero es muy sencillo y para un entorno de laboratorio y pruebas, vale, indiferentemente de qué sistema operativo uses.
* **La manera correcta**. Corregir la configuración que indica el error. Se trata de un error del Elasticsearch que trae embebido Sonar, y la solución depende del sistema operativo utilizado. Más info: [Install Elasticsearch with Docker](elastic.co/guide/en/elasticsearch/reference/current/docker.html). También suele ser útil revisar las recomendaciones de configuración del Docker Host indicadas para la imagen de SonarQube: [Docker Hub - SonarQube](https://hub.docker.com/_/sonarqube)

Este Docker Compose usa la variable SONAR_ES_BOOTSTRAP_CHECKS_DISABLE, como solución sencilla. Si deseas realizar un uso profesional, es recomendable quitar esa variable de entorno y seguir las configuraciones del Docker Host indicadas en [Docker Hub - SonarQube](https://hub.docker.com/_/sonarqube)

# Docker - Ejecución en local con Docker Compose

Lo primero, en la raíz del repo, debemos **editar el fichero .env** donde se establecen variables que podemos utilizar en los contenedores definidos en el Docker Compose, como sería la Zona Horaria. 

Por ejemplo, para usar Madrid deberíamos dejar así la variable TZ en el ficher .env

```shell
TZ=Europe/Madrid
```

Los siguientes comandos ejecutados en la raíz del Proyecto, muestra cómo arrancar todos los contenedores con Docker Compose, como comprobar su estado, así como la forma de poder comprobar los logs de su ejecución.

```shell
docker-compose up -d
docker-compose ps
docker-compose logs
```

Podemos abrir una sesión bash en cualquiera de los contenedores ejecutando con comando como el siguiente:

```shell
docker exec -it sonar-docker-lab-postgres-1 bash
```

En caso necesario, podemos parar (sin eliminar) y volver a arrancar todos los contenedores Docker con los siguientes comandos:

```shell
docker-compose stop
docker-compose start
```

Si deseamos parar y/o arrancar un único contenedor (ej: sonarqube), podemos utilizar comandos como los siguientes:

```shell
docker-compose stop sonarqube
docker-compose start sonarqube
```

Podemos parar y eliminar todos los contenedores con el siguiente comando:

```shell
docker-compose down
```

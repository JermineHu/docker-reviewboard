# docker-reviewboard
A Docker image based on Alpine Linux for running the code review tool
[Review Board](https://www.reviewboard.org).


## Usage

### docker-compose.yml:
```yml
version: '3'
volumes:
  mysql:
  reviewboard:
services:
  mysql:
    container_name: mysql
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_DATABASE=reviewboard
      - MYSQL_USER=reviewboard
      - MYSQL_PASSWORD
      - MYSQL_ROOT_PASSWORD
    volumes:
      - mysql:/var/lib/mysql
  memcached:
    container_name: memcached
    image: memcached:alpine
    restart: always
  reviewboard:
    container_name: reviewboard
    image: jermine/reviewboard
    restart: always
    links:
      - mysql:db
      - memcached
    ports:
      - "8080:8000"
    environment:
      - DB_PASSWORD
      - RB_ADMIN_PASSWORD
      - RB_ADMIN_EMAIL
      - UWSGI_PROCESSES
      - VIRTUAL_HOST
    volumes:
      - reviewboard:/var/www
```

or used External database for postgresql :

```yml

version: '3'
services:
  memcached:
    container_name: memcached
    image: memcached:alpine
    restart: always
  reviewboard:
    container_name: reviewboard
    image: jermine/reviewboard
    restart: always
    links:
      - memcached
    ports:
      - "8080:8000"
    environment:
      - DB_HOST: 192.168.1.19
      - DB_TYPE: postgresql
      - DB_PORT: 2379
      - DB_NAME: reviewboard
      - DB_USER: Jermine
      - DB_PASSWORD: 123456
      - RB_COMPANY: KangPany
      - RB_ADMIN: KangPany.admin
      - RB_ADMIN_PASSWORD: Jermine.KangPany.com
      - RB_ADMIN_EMAIL: admin@KangPany.com
      - UWSGI_PROCESSES: 10
    volumes:
      - /data/reviewboard:/var/www


```


### Set an .env file for container like below codes:
```sh
MYSQL_ROOT_PASSWORD=parssw0rd
MYSQL_PASSWORD=parssw0rd
DB_PASSWORD=parssw0rd
DB_HOST=db
RB_COMPANY="kongmpany "
RB_ADMIN_PASSWORD=parssw0rd
RB_ADMIN_EMAIL=admin@review.box
UWSGI_PROCESSES=10
#VIRTUAL_HOST=reviews.example.com
```

### Start the containers:
```sh
$ docker-compose up -d
```

## Environment variables

  * `DB_TYPE`
    * The type of database to be used, `mysql` or `postgresql`
    * Default: `mysql`
  * `DB_PORT`
    * The port for the database
    * Default: `3306`
  * `DB_HOST`
    * The host address of the database
    * Ｄefault: `db`  
  * `DB_NAME`
    * The name of the database
    * Default: `reviewboard`
  * `DB_USER`
    * The database user
    * Default: `reviewboard`
  * `DB_PASSWORD`
    * The password associated to the database user
    * Default: `reviewboard`
  * `RB_COMPANY`
    * Your company's name
    * Default: `Example Inc`
  * `RB_ADMIN`
    * The login name for the Review Board admin
    * Default: `admin`
  * `RB_ADMIN_PASSWORD`
    * The password for the Review Board admin
    * Default: `admin`
  * `RB_ADMIN_EMAIL`
    * The e-mail address for the Review Board admin
    * Default: `admin@example.com`
  * `UWSGI_PROCESSES`
    * The number of thread to be used by the web server
    * Default: `10`
  * `VIRTUAL_HOST`
    * The fully qualified domain name for Review Board
    * Default: `*` (insecure!)
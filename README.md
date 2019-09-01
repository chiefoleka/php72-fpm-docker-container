# Simple docker container setup for running PHP applications

Following the guide provided [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-laravel-nginx-and-mysql-with-docker-compose), I've created configurations to setup a mysql, nginx and php service using docker-compose to enable you run Lumen and Laravel applications.

## Configurations
### PHP
Update the `local.ini` file in the php directory with some extensions or configs you want to add to your php setup.

Do not forget to update `upload_max_filesize` and `post_max_size` if you want to support uploading of large files.

### MySQL
Update the `my.cnf` file in the mysql directory with configs you want to add to your mysql setup.

### Nginx
Update the `default.conf` file in the nginx directory with configs you want to add to your nginx conf. You can also add other configuration files in it if you will use the `nginx` service for multiple domains/subdomains. 

Do not forget to update `client_max_body_size` if you want to support uploading of large files.

## Setup
Update the `docker-compose.yml` file with the details for your own setup (database name, password, etc). Then run:
```bash
$ docker compose up
```
and it will create the services and run your application for you.

If you want to run `docker-compose` in the background, simply run:
```bash
$ docker compose up -d
```

## Running commands
You are likely to run certain commands specific to the individual containers like `php artisan migrate`. You can do it like this:
```
$ docker-compose exec app php artisan migrate
```

If you want to get into any of the container for any reason, simply do:
```
$ docker-compose exec app bash
$ docker-compose exec db bash
```

**NOTE:** The `nginx` container does not have a `bash` interface, so you will not get into the container.

## Extras
I created these scripts because I needed to setup an environment to enable several developers work on different services for a distributed application. All the services are simple and are all built on PHP/Lumen.

I had to deal with exposing ports to my host machine, since I had to run about 7 services as at the time I created this script. To do that successfully, I had to map different ports for different applications. I didn't need to update the applications to use those ports (especially for mysql) because `3306` was exposed to all services on the `docker` network. Incase you didn't know that, don't go stressing yourself when connections are not working.

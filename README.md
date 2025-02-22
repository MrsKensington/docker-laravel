# Laravel Docker Container

Simple docker container which can be used for running laravel applications. The container can be run in 3 different roles to provide a complete laravel infrastructure. 

# Environment Variables

## CONTAINER_ROLE
The containers roll can be controled by setting the CONTAINER_ROLE environement variable to one of the following.

- app - Container to run the Laravel application served up by php-fpm
- schedular - Container  to run the Laravel schedular every minute
- queue - Container to continuously process the Laravel queue

Default: app

## APP_ENV
The container can be run in two different modes:>

- local - To be used during development. No caching is performed
- production - To be used when doing production deployments. On bootup artisan is run to cache the config, routes and views to speed up the application

Default: production

## QUEUE_TIMEOUT
Controls the job timeout (in seconds) used for the queue process. If a job takes longer than the timeout then the queue will kill it.

Default: 90

## QUEUE_RETRIES
Controls the number of times a queue must fail before it's considered a failed job.

Default: 3

# Example Docker file
    version: '3'
    services:
    
      app:
        image: mrskensington/laravel
        container_name: app
        restart: unless-stopped
        tty: true
        environment:
          CONTAINER_ROLE: app
        working_dir: /var/www
        volumes:
          - ./app:/var/www
    
      scheduler:
        image: mrskensington/laravel
        container_name: scheduler
        restart: unless-stopped
        tty: true
        environment:
          CONTAINER_ROLE: scheduler
        working_dir: /var/www
        volumes:
          - ./app:/var/www
    
      queue:
        image: mrskensington/laravel
        container_name: queue
        restart: unless-stopped
        tty: true
        environment:
          CONTAINER_ROLE: queue
        working_dir: /var/www
        volumes:
          - ./app:/var/www

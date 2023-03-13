# pingcrm
Demo Application from InertiaJS with Laravel &amp; Vue.   
        
The original version uses Artisan Serve with SQLite and in this version we are going to use Sail with MySQL.     
        
Original Source: https://github.com/inertiajs/pingcrm    
Sail Version Source: https://github.com/toyo-uc/pingcrm  

![](https://raw.githubusercontent.com/inertiajs/pingcrm/master/screenshot.png)

# Setup   
> You may need to erase MySQL related container volume to avoid problems with MySQL!

Clone the repo locally:   
```
git clone https://github.com/inertiajs/pingcrm.git pingcrm
cd pingcrm
```

Install PHP dependencies, in some cases you have to use composer update:
```
composer install
```

Install NPM dependencies:
```
npm ci
```

Build assets:
```
npm run dev
```

Setup configuration:
```
cp .env.example .env
```

Inside .env change the DB Connection to mysql since we are not going to use sqlite and update username & password to laravel default:
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=sail
DB_PASSWORD=password
```

Create docker-compose.yml
```
# For more information: https://laravel.com/docs/sail
version: '3'
services:
    laravel.test:
        build:
            context: ./vendor/laravel/sail/runtimes/8.1
            dockerfile: Dockerfile
            args:
                WWWGROUP: '${WWWGROUP}'
        image: sail-8.1/app
        extra_hosts:
            - 'host.docker.internal:host-gateway'
        ports:
            - '${APP_PORT:-80}:80'
            - '${VITE_PORT:-5173}:${VITE_PORT:-5173}'
        environment:
            WWWUSER: '${WWWUSER}'
            LARAVEL_SAIL: 1
            XDEBUG_MODE: '${SAIL_XDEBUG_MODE:-off}'
            XDEBUG_CONFIG: '${SAIL_XDEBUG_CONFIG:-client_host=host.docker.internal}'
        volumes:
            - '.:/var/www/html'
        networks:
            - sail
        depends_on:
            - mysql
    mysql:
        image: 'mysql/mysql-server:8.0'
        ports:
            - '${FORWARD_DB_PORT:-3306}:3306'
        environment:
            MYSQL_ROOT_PASSWORD: '${DB_PASSWORD}'
            MYSQL_ROOT_HOST: "%"
            MYSQL_DATABASE: '${DB_DATABASE}'
            MYSQL_USER: '${DB_USERNAME}'
            MYSQL_PASSWORD: '${DB_PASSWORD}'
            MYSQL_ALLOW_EMPTY_PASSWORD: 1
        volumes:
            - 'sail-mysql:/var/lib/mysql'
            - './vendor/laravel/sail/database/mysql/create-testing-database.sh:/docker-entrypoint-initdb.d/10-create-testing-database.sh'
        networks:
            - sail
        healthcheck:
            test: ["CMD", "mysqladmin", "ping", "-p${DB_PASSWORD}"]
            retries: 3
            timeout: 5s
networks:
    sail:
        driver: bridge
volumes:
    sail-mysql:
        driver: local
```

> Be careful here, original repo uses php to execute each command and here we are going to use sail on each command since we are going to run the project on sail.             
```
sail up -d
```

Generate application key:
```
sail artisan key:generate
```

Run database migrations:
```
sail artisan migrate
```

Run database seeder:
```
sail artisan db:seed
```

Login with:   

- **Username:** johndoe@example.com   
- **Password:** secret    

# Compile Vue automatically
```
npm run watch
```

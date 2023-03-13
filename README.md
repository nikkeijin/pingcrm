# pingcrm
Demo Application from InertiaJS with Laravel &amp; Vue.   
Source: https://github.com/inertiajs/pingcrm    

The original version uses Artisan with SQLite and in this version we are going to use Sail with MySQL.    

![](https://raw.githubusercontent.com/inertiajs/pingcrm/master/screenshot.png)

# Setup   
Note: You may need to erase container volume to avoid problems with MySQL

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

Run sail:
```
Sail up -d
```

Generate application key:
```
php artisan key:generate
```

Run database migrations:
```
php artisan migrate
```

Run database seeder:
```
php artisan db:seed
```

Login with:   

- **Username:** johndoe@example.com   
- **Password:** secret    

# Compile Vue automatically
```
npm run watch
```

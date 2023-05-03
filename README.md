# ![Tagh Time App]


# Getting started with Back-end in linux server
## Dependencies
     php 8.1
     mysql or mariadb
     memcached
     fswatch
     supervisor
   
----------

## Installation

Clone the repository

    git clone git@gitlab.com:amidigital/Tagh-Time.git

Switch to the repo folder

    cd Tagh-Time


Switch to the master branch

    git checkout master
    
Install all the dependencies using composer

    composer install --no-scripts --ignore-platform-reqs

----------

## Configuration .env file
Copy the example env file and make the required configuration changes in the .env file


    cp .env.example .env

change  following valuess

| **Key**          	| **Value**                                                 |
|------------------	|--------------------------------------------------	     |
| APP_URL       	| your domin URL e.g : https://taghtime.com/                |
| FRONT_APP_URL 	| your domin URL e.g : https://taghtime-front.com 	     |
| BASE_URL    	     | your domin URL without http schema e.g   : taghtime.com  	|
| DB_DATABASE       | your database name                                   	|
| DB_HOST           | your database host ip e.g 11.222.22.225                  	|
| DB_USERNAME       | your database user                                      	|
| DB_PASSWORD       | your database password                               	|
| MAIN_DB    	     | same as your database name                               	|


Email Configurations

     MAIL_MAILER=smtp
     MAIL_HOST=your stmp server
     MAIL_PORT=your stmp port
     MAIL_USERNAME=your stmp user
     MAIL_PASSWORD=your stmp password
     MAIL_ENCRYPTION=TLS


Generate a new application key

    php artisan key:generate

----------

## Migrations 
Run the database migrations

     php artisan migrate:fresh --path=/database/migrations/company

 Run the database seeders
 
     php artisan db:seed --class=Database\\Seeders\\Company\\PlanSeeder

  

You can now access the server at https://yourdomain

----------

## configuration queue work by supervisor
   make service for fswatch

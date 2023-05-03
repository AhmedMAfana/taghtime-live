# ![Tagh Time App]


# Getting started with Back-end in ubuntu server
## Dependencies
     memcached
     fswatch
     supervisor
     
## Installation

Clone the repository

    git clone git@gitlab.com:amidigital/Tagh-Time.git

Switch to the repo folder

    cd Tagh-Time


Switch to the master branch

    git checkout master
    
Install all the dependencies using composer

    composer install --no-scripts --ignore-platform-reqs


Copy the example env file and make the required configuration changes in the .env file


    cp .env.example .env




Generate a new application key

    php artisan key:generate


Run the database migrations

     php artisan migrate:fresh --path=/database/migrations/company

 Run the database seeders
 
     php artisan db:seed --class=Database\\Seeders\\Company\\PlanSeeder

  

You can now access the server at https://yourdomain

## configuration queue work by supervisor
   make service for fswatch

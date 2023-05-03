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

## Configuration queue work by supervisor
Switch supervisor conf folder  
``` bash
   cd /etc/supervisor/conf.d
    #change the permissions of the folder   
   sudo chmod -R 777 ../conf.d
  
```
create a yourdomain-worker.conf file that starts and monitors queue:work processes: and put this content
```bash
     [program:yourdomain-worker]
     process_name=%(program_name)s_%(process_num)02d
     command=php **PathToYourProject**/artisan queue:work --sleep=3 --tries=3 --max-time=3600
     autostart=true
     autorestart=true
     stopasgroup=true
     killasgroup=true
     user=forge
     numprocs=8
     redirect_stderr=true
     stdout_logfile=**PathToYourProject**/yourdomain-worker.log
     stopwaitsecs=3600
 ```
     
## Starting Supervisor

     sudo supervisorctl reread

     sudo supervisorctl update

     sudo supervisorctl start laravel-worker:*
     
 ----------
 ## Configuration for fswatch to watch any new subdomain and start Supervisor 
 
 Create bash file with name **watch_worker.sh** in root of your project with the given code
 ```bash
 
     sudo nano watch_worker.sh 
     
```
Write this code 
```bash
     #!/bin/bash
     echo "new file is added $1"
     filename=$(basename ${1})
     AMI_HOST=".taghtime.com"     ##modifiy this to your .domain

     SERVER_PATH="/home/ami1/Desktop/test-redme/Tagh-Time/"    ## modifiy this to your backend package path
     RUN_SUPERVISOR_FILE="${SERVER_PATH}run_supervisor.sh"

     echo $filename
     IFS='.'

         if [[ $filename == *$AMI_HOST* ]]; then

          read -ra ADDR <<< "$filename"   # str is read into an array as tokens separated by IFS
             for i in "${ADDR[2]}"; do   # access each element of array
                echo "$i : subdomain added \n" | tee success_domain.txt
                echo "$i"
                bash "${RUN_SUPERVISOR_FILE}"  $i $SERVER_PATH
             done

          else
          echo "not match"
         fi
```
 
 
  Give bash file **watch_worker.sh** a permission
     
``` bash
    #change the permissions of the file   
    sudo chmod -R 777 watch_worker.sh
  
```

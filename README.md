
# DEV
- http://localhost/moodle
- http://localhost/phpmyadmin  
    user: admin   
    pass: ***find on logs***

## Install Moodle
- http://localhost/moodle  
    Data Directory: ```/var/www/moodledata```

## DOCKER IMAGE
Based on https://hub.docker.com/r/mattrayner/lamp  
OS: Ubuntu 16.04.6 LTS  
Apache/2.4.18 (Ubuntu)  
Database client version: libmysql - mysqlnd 7.4.3  
PHP version: 7.4.3 (default)  
MySQL Version: 5.7.29-0ubuntu0.16.04.1-log  
phpMyAdmin Version: 5.0.1       

### Sync Content
- /app            (your PHP files live here)
- /app/moodle     (moodle 3.8.2 - replace with latest) 
- /mysql          (docker will create this and store your MySQL data here)

### Moodledata
- path: ```/var/www/moodledata```

### COMMANDS

> list containers  
```$ docker container ls --all```

> stop container  
```$ docker container stop moodledev1```

> remove container  
```$ docker container prune```

> connect ssh  
```$ docker exec -it moodledev1 /bin/bash```

> change PHP version
    // connect ssh  
    ```root# a2dismod php7.2```  
    ```root# a2enmod php7.3```  
    ```root# service apache2 restart```  
    ```root# update-alternatives --set php /usr/bin/php7.3```

# INSTALL 
**Launch docker image and setup**
// run
> Option with mysql folder synced  
    ```$ docker run --name "moodledev1" -d -i -t -p "80:80" -v ${PWD}/app:/app -v ${PWD}/mysql:/var/lib/mysql mattrayner/lamp:latest-1604```
> Option without mysql folder synced  
    ```$ docker run --name "moodledev1" -d -i -t -p "80:80" -v ${PWD}/app:/app mattrayner/lamp:latest-1604```  

// wait docker to complete  
    check ```$ docker container ls --all```  

// setup moodledata  
    ```$ docker exec -it moodledev1 /bin/bash```  
    ```root# mkdir /var/www/moodledata```  
    ```root# chmod 777 /var/www/moodledata```  
    ```root# ls -alh /var/www```  

// get mysql credentials  
    ```docker logs moodledev1```  

// install missing extensions  
    ```$ apt update```  
    ```$ apt-get upgrade```  
    ```$ apt install php7.4-intl php7.4-xmlrpc php7.4-soap```  
    ```$ service apache2 restart```  

// add PHP 7.2    
```$ apt-get install php7.2 php7.2-cli php7.2-common```    
```$ apt-get install php7.2-curl php7.2-gd php7.2-json php7.2-mbstring php7.2-intl php7.2-mysql php7.2-xml php7.2-zip php7.2-xmlrpc php7.2-soap```  

// add PHP 7.3  
```$ apt-get install php7.3 php7.3-cli php7.3-common```    
```$ apt-get install php7.3-curl php7.3-gd php7.3-json php7.3-mbstring php7.3-intl php7.3-mysql php7.3-xml php7.3-zip php7.3-xmlrpc php7.3-soap```  

// change PHP version  
```$ a2dismod php7.4```  
```$ a2enmod php7.2```  
```$ service apache2 restart```    
```$ update-alternatives --set php /usr/bin/php7.2```  

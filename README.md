# rtmp-test

# Server Side Install #

## Basic Install ##
    sudo apt-get update
    sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev unzip zlib1g zlib1g-dev
    
    wget http://nginx.org/download/nginx-1.13.8.tar.gz
    wget https://github.com/arut/nginx-rtmp-module/archive/master.zip
    
    tar -zxvf nginx-1.13.8.tar.gz
    unzip master.zip
    cd nginx-1.13.8
    
    ./configure \
        --add-module=../nginx-rtmp-module-master  \
        --with-http_ssl_module  \
        --with-http_realip_module \
        --with-http_addition_module \
        --with-http_sub_module \
        --with-http_dav_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_random_index_module \
        --with-http_secure_link_module \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-mail \
        --with-mail_ssl_module \
        --with-file-aio \
        --with-ipv6 \
    
    make
    sudo make install
    
    sudo /usr/local/nginx/sbin/nginx
    
    sudo nano /usr/local/nginx/conf/nginx.conf
    
## Config nginx ##
  Add the following at the very end of the file:
    
    rtmp {
        server {
                listen 1935;
                chunk_size 4096;
                application live {
                        live on;
                        record off;
                }
        }
    }
    
  Restart nginx with:
  
    sudo /usr/local/nginx/sbin/nginx -s stop
    sudo /usr/local/nginx/sbin/nginx
    
## Test ##
  install rtmpdump: sudo apt-get install -y rtmpdump
  
  test command: rtmpdump -r rtmp://[your server ip]/live/gibberish
  
  success response: 
    Connecting ...
    INFO: Connected...
  
## nginx-init-ubuntu ##

    #copy/download/curl/wget the init script
    sudo wget https://raw.github.com/chakkritte/nginx-init-ubuntu/master/nginx -O /etc/init.d/nginx
    sudo chmod +x /etc/init.d/nginx
    
    service nginx status  # to poll for current status
    service nginx stop    # to stop any servers if any
    service nginx start   # to start the server
    
    #[optional] 
    sudo update-rc.d -f nginx defaults

    #[optional remove the upstart script]
    sudo update-rc.d -f nginx remove

    
## Reference ##
  [1] [How to Live Stream to Multiple Services with a RTMP Server](http://linustechtips.com/main/topic/174603-how-to-live-stream-to-multiple-services-with-a-rtmp-server/)
  
  [2] [nginx-init-ubuntu](https://github.com/JasonGiedymin/nginx-init-ubuntu)
  
  [3] [Install LEMP Server (Nginx, MySQL or MariaDB, PHP And phpMyAdmin) On Ubuntu 14.10/14.04/13.10](http://www.unixmen.com/install-lemp-server-nginx-mysql-mariadb-php-phpmyadmin-ubuntu-14-1014-0413-10/)
  
  [4] [nginx-rtmp-php-install-ubuntu](https://github.com/chakkritte/nginx-rtmp-install-ubuntu)
  
  [5] [How to install the zlib library in Ubuntu Linux](https://www.systutorials.com/how-to-install-the-zlib-library-in-ubuntu/)
  
  [6] [Test RTMP connection like ping domain -t in CMD](https://stackoverflow.com/questions/23822144/test-rtmp-connection-like-ping-domain-t-in-cmd)

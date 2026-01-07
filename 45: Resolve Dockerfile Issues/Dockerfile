FROM httpd:2.4.43

WORKINGDIR /usr/local/apache2

RUN sed -i "s/Listen 80/Listen 8080/g" conf/httpd.conf \

&& sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf \ 

&& sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf \

&& sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf 

COPY ./certs/server.crt conf/server.crt 

COPY ./certs/server.key conf/server.key 

COPY ./html/index.html htdocs/index.html
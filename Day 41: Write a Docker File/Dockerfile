FROM ubuntu:24.04
RUN apt-get update \
&& apt-get install -y apache2 \
&& sed -i 's/Listen 80/Listen 8082/' /etc/apache2/ports.conf
EXPOSE 8082

ENTRYPOINT ["apache2ctl", "-D", "FOREGROUND"]
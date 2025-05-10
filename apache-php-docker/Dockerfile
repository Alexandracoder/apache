FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt-get update && \
    apt-get install -y tzdata apache2 php libapache2-mod-php && \
    ln -fs /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get clean

# Copiar el archivo index.html desde apache-php-docker al contenedor
COPY apache-php-docker/index.html /var/www/html/

EXPOSE 8080

# Cambiar la configuración de Apache para usar el puerto 8080
RUN sed -i 's/80/8080/g' /etc/apache2/ports.conf && \
    sed -i 's/80/8080/g' /etc/apache2/sites-available/000-default.conf

CMD ["apachectl", "-D", "FOREGROUND"]

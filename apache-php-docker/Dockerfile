FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y apache2 php libapache2-mod-php && \
    apt-get clean
COPY index.html /var/www/html/
EXPOSE 8080
RUN sed -i 's/80/8080/g' /etc/apache2/ports.conf && \
    sed -i 's/80/8080/g' /etc/apache2/sites-available/000-default.conf
CMD ["apachectl", "-D", "FOREGROUND"]

FROM ubuntu:20.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

# Install tzdata, Apache, PHP, and other dependencies
RUN apt-get update && \
    apt-get install -y tzdata apache2 php libapache2-mod-php && \
    ln -fs /usr/share/zoneinfo/$TZ /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get clean

# Copy the index.html file into the container
COPY apache-php-docker/index.html /var/www/html/

# Expose the port 81 for Apache (this is the port you want Apache to listen to)
EXPOSE 81

# Modify Apache's configuration to use port 81 instead of port 80
RUN sed -i 's/Listen 80/Listen 81/g' /etc/apache2/ports.conf && \
    sed -i 's/<VirtualHost *:80>/<VirtualHost *:81>/g' /etc/apache2/sites-available/000-default.conf

# Command to run Apache in the foreground
CMD ["apachectl", "-D", "FOREGROUND"]


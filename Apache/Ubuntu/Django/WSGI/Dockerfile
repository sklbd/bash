FROM ubuntu:latest

LABEL author="Shakil Ahamed"
LABEL maintainer="sklahamed@gmail.com"

#====================================================================================
# Update your OS
#====================================================================================
RUN apt update -y


#====================================================================================
# Install necessary packages
#====================================================================================
RUN apt-get install apache2 apache2-utils libexpat1 ssl-cert python3 libapache2-mod-wsgi-py3 nano -y
RUN apt install python3.10-venv -y


#====================================================================================
# Add customized apache configuration to the container
#====================================================================================
ADD ./000-default.conf /etc/apache2/sites-available/


#====================================================================================
# Create directory for the project
# I follow a conventions where /home/<domain name>/<project name>/code
# Note: You can modify it to your own choice. I like it in this way.
#====================================================================================
RUN mkdir -vp /home/localhost/deployment/code

#====================================================================================
# Create a virtual environment for the project.
# Note: It is funny to create a virtual environment in a container where a container itself is an isolated virtual machine.
# However, to run the application without VENV remove below line and add RUN ln /usr/bin/python3 /usr/bin/python instead
#====================================================================================
RUN python3 -m venv /home/localhost/deployment/sklenv


#====================================================================================
# Copy your code from host to the container
#====================================================================================
COPY ./code /home/localhost/deployment/code


#====================================================================================
# Activate VENV. It is alternative to the command: source ./<venv>/bin/activate
#====================================================================================
ENV PATH="/home/localhost/deployment/sklenv/bin:$PATH"

#====================================================================================
# Set the current working directory into the container.
# Usually after activating our VENV we go to the directory where manage.py is located this is what we are doing here.
# This would be the directory where manage.py is located.
# Similar to command, <VENV>: cd /home/localhost/deployment/code
#====================================================================================
WORKDIR /home/localhost/deployment/code

#====================================================================================
# Magic command to install project's dependency
#====================================================================================
RUN pip install wheel
RUN pip install -r requirements.txt

#====================================================================================
# We would like to run the application in port 80 of the container
#====================================================================================
EXPOSE 80

#====================================================================================
# Run apache
#====================================================================================
CMD ["apache2ctl", "-D", "FOREGROUND"]


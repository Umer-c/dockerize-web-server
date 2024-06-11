# dockerize-web-server
Run Docker-Container on Ubuntu server and Containerise an App.

Use the Vagrantfile uploaded here, it contains the auto provisioning of Docker installation. To verify Docker is working correctly, run:

```
sudo docker run hello-world
```
You should see the following output:

Hello from Docker!
This message shows that your installation appears to be working correctly.

### Manage Docker Images and Containers

List Docker images, containers and the stopped ones

```
docker images
docker ps
docker ps -a
```

### Test Run of official Nginx Web Server Image

Run the following command to start an Nginx web server container, mapping port 9080 on the host to port 80 on the container:

```
docker run --name web01 -d -p 9080:80 nginx
```

To check if the web server is accessible, get the IP address of the VM:

```
ip addr show
```

Access the web server in your browser using http://<VM_IP>:9080. If you cannot see the page, check your firewall settings.

### Build Your Own Docker Image

Create a new directory for your images:

```
mkdir -p images/nano
cd images
```

Download and extract a website template:

```
wget https://www.tooplate.com/zip-templates/2077_modern_town.zip
unzip 2077_modern_town.zip -d nano
```

Create a tarball of the website contents:

```
cd nano/2077_modern_town
tar czvf ../nano.tar.gz *
cd ..
mv nano.tar.gz ../
```

Create a Dockerfile
Inside the nano directory, create a file named Dockerfile with the following content:

```
FROM ubuntu:latest
LABEL "Author"="Myself"
LABEL "Project"="nano"
RUN apt update
RUN apt install apache2 git -y
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
EXPOSE 80
WORKDIR /var/www/html
VOLUME /var/log/apache2
ADD nano.tar.gz /var/www/html
#COPY nano.tar.gz /var/www/html
```

Build and verify the Docker image:

```
docker build -t nanoimage .
docker images
```
Run a container from your image:

```
docker run -d --name mypersonalwebsite -p 9090:80 nanoimage
```

Access your website in your browser using http://<VM_IP>:9090.

### Conclusion

By following these steps, you can containerize a web server application using Docker. This project provides a foundational understanding of Docker, Vagrant, and basic Linux system administration.





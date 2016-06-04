Docker for running magento 1
============================

This docker setup was created for OS X. I'll confirm in the future if it works on other OS, but for the moment I can confirm it works on a mac.

Also this docker should be setup on the non sensitive disk parition. When I tried to do the same on my case sensitive partion the folders couldn't be mounted in  the running containers.

Setup:
----------------------------
Follow these steps to set your own docker setup and run a magento or any other website on your local machine:

1. Install docker from here: https://www.docker.com/products/docker-toolbox.

2. After install is complete start "Docker Quick Start Terminal" or run this into a terminal on a MAC:
	```
	$ sh /Applications/Docker/Docker Quickstart Terminal.app/Contents/Resources/Scripts/start.sh
	```
	On different OS this path may be differnt.
	
	This should start the docker environment.

3. Create a new work folder for ex:
	```
	$ mkdir -p ~/Work/docker/magento && cd ~/Work/docker/magento.
	```

	I'll be using this ~/Work/docker/magento as a home folder.

4. Clone the current repository into this folder.
	```
	$ git clone https://github.com/popoviciandrei/dockerplay.git .
	```
5. Get the docker ip via this command:
	```
	$ docker-machine ip default
	```
	Take this ip and replace the default 192.168.99.100 on this line 'XDEBUG_CONFIG: remote_host=192.168.99.100' inside the 'docker-compose.yaml' if different. This is needed to make the xdebug to work later.

6. Add a new line in the /etc/hosts:  192.168.99.100 magento.docker.example.com . Replace 192.168.99.100 with the ip found at step 5
7. Initialie docker containers with:
	```
	$ docker-compose up -d
	```
8. Up to now all the commands were run inside the home folder created at step 3. Now create a new folder in here 'www' and go into it:
	```
	$ mkdir www && cd www
	```
9. Inside www copy/clone your magento instalation and make sure the index.php of magento is in this folder.

10. Create the local.xml inside app/etc and make sure that you set these values inside the <connection> tag:
	```xml
	<host><![CDATA[192.168.99.100]]></host>
	<username><![CDATA[root]]></username>
	<password><![CDATA[root]]></password>
	<dbname><![CDATA[magento]]></dbname>
	```
11. Now everything should be in place. So if you navigate to http://magento.docker.example.com/ after a few moments you should see a default magento instalation.

12. If you want to use n98-magerun to do specific magento tasks you need to connect into php docker container.
	* First run this on command line:
	```
	$ docker ps
	```
	You should get a list of active containers. Identify the name of the one that has image "devdocker_php". By deafult is 'devdocker_php_1'.
	* Run this from command line:
	```
	$ docker exec -it devdocker_php_1 /bin/bash
	```
	Now you'll enter into an interactive mode bash into that container. By default you are connected into /var/www/html and n98-magerun is already available from command line. Type
	```
	$ n98-magerun sys:check
	```
13. Let's create a default admin users.
	* Connect into php container as per step 12
	* Run this command:
	```
	n98-magerun admin:user:create admin admin@example.com password123 FirstName Lastname Administrators
	```
	* Go to url http://magento.docker.example.com/admin. Enter user: admin, password: password123. You should be able to login.
14. Also on this docker setup we have phpmyadmin to have a db interface gui.
	You can access it from with from this url: http://192.168.99.100:8080. Enter user: root, password: root.	
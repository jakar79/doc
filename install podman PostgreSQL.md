# install podman podman-Desktop PostgreSQL on debian
## 1- install podman
	sudo apt install podman podman-compose

	** Note: Debian 13 (Trixie) ships with Podman 5.4.x.  For rootless functionality, ensure uidmap, passt, and fuse-overlayfs are installed (sudo apt install uidmap passt fuse-overlayfs). 

* To check if Podman has been successfully installed, you can run:

  		podman version
If there is no repository to fetch and install container Images by Podman, then we need to add that manually. We can use popular repositories such as Docker.io, Redhat, and Fedora.

Modify the Registry configuration file by executing the following command:

	nano /etc/containers/registries.conf
If you want to add the following registries, Please navigate to the bottom of the document and insert the provided line.
	
	[registries.search]
	registries = ['quay.io', 'docker.io']

Once you’ve finished, save the file by pressing Ctrl+X, followed by the Y key, and exit by pressing Enter.

You can employ Podman for creating and managing containers, along with other features. However, we won’t enumerate all of its capabilities in this context.

Step 4. Using Podman
Just like Docker, you can use almost the same commands to pull or run containers. To pull the alpine image for example, you can run:

	podman pull docker.io/alpine

You can also simply run podman pull alpine and select from which container registry to pull the image.

Running an Nginx container with port mapping can be done with this command:

	podman run -dit --name nginx -p 8080:80 docker.io/nginx

This will run an Nginx container in the background, mapping port 80 from the container to port 8080 on our server.

You can also check the containers that are currently running, or all containers that were also previously running with the commands:

	podman ps 
	podman ps -a 
You can also create pods, for example. To create an empty pod, you can run this:

	podman pod create --name web
Now that the pod is created, we can add a container to it:

	podman run -dt --pod web alpine:latest top
To check the containers running inside a pod, you can now run:

	podman ps --pod

## 2- Pull Postgesl Image
	podman pull docker.io/library/postgres:16

## 3- Create Persistent Volume
	podman volume create pgdata

## 4-run PostgreSQL Container
	podman run -d \
		--name postgres-db \
		-e POSTGRES_USER=admin \
		-e POSTGRES_PASSWORD='admin@123' \
		-e POSTGRES_DB=mydb \
		-v pgdata:/var/lib/postgresql/data \
		-p 5432:5432 \
		docker.io/library/postgres:16
	
## 5- Allow Remote Connections (pg_hba.conf)
	podman exec -it postgres-db bash -c "echo 'host all 0.0.0.0/0 md5'>>
	/var/lib/postgresql/data/pg_hba.conf"

## 6- Restart Container
	podman restart postgres-db

## 7- Open Firewall Port
	sudo ufw allow 5432/tcp
	sudo ufw reload

## 8- Test Postgresql (CLI)
### From server:
	sudo apt install -y postgresql-client
	psql -h localhost -U admin -d mydb
	
	Password:
	admin@123

### From remote machine:

	psql -h 192.168.136.128 -U admin -d mydb

## 9- Run pgAdmin (GUI via Podman - NO ERRORS)
	podman run -d \
		--name pgadmin \
		-p 8080:80 \
		-e PGADMIN-DEFAULT_EMAIL=admin@exemple.com \
		-e PGADMIN-DEFAULT_PQSSWORD=admin@123 \
		docker.io/dpage/pgadmin4
	
## 10- Access pg§Admin (Browser)

	Open:
	http://192.168.136.128:8080
	
	Login:
	* Email: `admin@example.com`
	* Password: `admin@123`	

## 11- Add Postgresql Server in pgAdmin

Inside pgAdmin:

	General/Name
	
	** Postgres-Server
	
	Connection tab:
	* Host: `192.168.136.128`
	* PORT: `5432`
	* Username: `admin`
	* Password: `admin@123`

## 12- Install Flatrpak
	sudo apt install -y flatpak
	flatpak remote-add --if-not-exists --user flathub https://flathub.org/repo/flathub.flatpakrepo   
* Install Podman Desktop: Use Flatpak to install the desktop environment.
  
		flatpak install -y --user flathub io.podman_desktop.PodmanDesktop   

* Launch: Start the application using:

		flatpak run io.podman_desktop.PodmanDesktop   

## 13- Useful Commands:

Check running containers:
		
	podman ps

Logs:

	podman logs postgres-db

Stop:

	podman stop postgres-db

Start:
podman start postgres-db

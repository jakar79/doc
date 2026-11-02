# install podman podman-Desktop PostgreSQL on debian
## 1- install podman
	sudo apt install -y podman

	** Note: Debian 13 (Trixie) ships with Podman 5.4.x.  For rootless functionality, ensure uidmap, passt, and fuse-overlayfs are installed (sudo apt install uidmap passt fuse-overlayfs). 

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

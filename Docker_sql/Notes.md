#### This notes take in count the next information:
SO: Ubuntu 22.04
Recently instalation

# 🎥 Introduction to Docker

## To install Docker in ubuntu:
https://docs.docker.com/engine/install/ubuntu/

## Add User to the Docker Group:
Add your user to the docker group to grant it permission to interact with the Docker daemon. Run the following command:
```bash
sudo usermod -aG docker $USER
```
After running this command, you will need to log out and log back in (or restart your system) for the changes to take effect. Make sure to open a new terminal window after logging in again.

## Verify Group Membership:

To verify that your user has been added to the docker group, you can run:

```bash
groups
```
Ensure that docker is listed among the groups for your user.

## Run docker
The following commands should be executed in the terminal. You need to run 'build' if you have made any changes to the Dockerfile or pipeline.py file

```bash
docker build -t test:pandas .
docker run -it test:pandas
```

## Run Docker Command Without sudo:
After adding your user to the docker group, you should be able to run Docker commands without using sudo. Try running your docker run command again without sudo:

# 🎥 Ingesting NY Taxi Data to Postgres
## Install postgresql

```bash
docker run -it \
    -e POSTGRES_USER="root" \
    -e POSTGRES_PASSWORD="root" \
    -e POSTGRES_DB="ny_taxi" \
    -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
    -p 5432:5432 \
    postgres:13
```
## Install pgcli
In order to install pgcli ,  it’s necessary to instal pip wich is in python

```bash
sudo apt install python3-pip
```

Now install pgcli

```bash
sudo apt install pip pgcli
```

Whit this command you can go into the data base the was create:
```bash
pgcli -h localhost -p 5432 -u root -d ny_taxi
```

## Using Jupiter Notebook

I will install anaconda.
https://docs.anaconda.com/free/anaconda/install/linux/

Configure the path of jupyter Notebook

jupyter notebook --generate-config

gedit ~/.jupyter/jupyter_notebook_config.py

Find and Modify the c.NotebookApp.notebook_dir:

c.NotebookApp.notebook_dir = '/path/to/your/notebook/folder'

## Download NY Taxi Data
Original links of data
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page
https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

Source for this project
https://github.com/DataTalksClub/nyc-tlc-data

## converRun
go to docker_sql folder (<your root folder>/Docker_sql/)
```bash
gunzip yellow_tripdata_2021-01.csv.gz 
```


# Connecting pgAdmin and Postgres

docker pull dpage/pgadmin4

# Running pgAdmin
If you create this pgadmin connection you can`t connect with the data base that you create before because there are in differents conteiner.

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4
```
# create a connection

### Running Postgres and pgAdmin together

Create a network

```bash
docker network create pg-network
```

## Run Postgres (change the path)

```bash
docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
```

## Run pgAdmin

```bash
docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin \
  dpage/pgadmin4
```

## convert ipynb to py
```bash
jupyter nbconvert --to=script Upload-data.ipynb
```


# Docker-Compose 

Install docker compose in ubuntu
```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

Note: to make pgAdmin configuration persistent, create a folder `data_pgadmin`. Change its permission via

```bash
mkdir data_pgadmin
sudo chown 5050:5050 data_pgadmin
```


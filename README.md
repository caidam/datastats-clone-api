# Deploying Your App on an EC2 Instance at Launch

How to deploy a containerized app on an EC2 instance at launch using user data.

## Create an EC2 Instance

Create an EC2 Ubuntu instance and pass the below user data to be ran at launch. You can update the environment variables to your liking. Just make sure not to modify the POSTGRES_SERVICE_IP.

## User Data Script

```bash
#!/bin/bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install git -y
sudo apt-get install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo apt-get install docker-compose -y
git clone https://github.com/caidam/datastats-clone-api.git
cd datastats-clone-api
echo "POSTGRES_USER=docker" >> .env
echo "POSTGRES_PASSWORD=docker" >> .env
echo "POSTGRES_DB=docker" >> .env
echo "POSTGRES_PORT=5432" >> .env
echo "POSTGRES_SERVICE_IP=172.20.0.2" >> .env
echo "DEBUG=True" >> .env
echo "FILE_URL=https://gist.githubusercontent.com/caidam/0df73c14f35e27faf7f3ebdead5ba37e/raw/3524e2027ecdc8375e6f9528a21f4c8659f025ea/workshop5-wcs-datastats.csv" >> .env
sudo docker build -t flask-api . && sudo docker-compose up -d
```
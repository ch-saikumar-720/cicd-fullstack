**Required ports:**
http port -80
Backend communication port- 5000
(Port 5000 is commonly used for running backend applications, especially Flask and Express.js servers.)
Mysql-server port- 3306
npm port for frontend- 3000
(Port 3000 is commonly used for running backend applications, especially node.js and react.js servers.)

**Backend Configuration:**

**Installing and creating a database**
sudo apt update && sudo apt upgrade -y
sudo apt install -y mysql-server
sudo mysql -u root -p
CREATE USER 'your_user'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'your_user'@'%' WITH GRANT OPTION;
SHOW GRANTS FOR 'youruser'@'%';

sudo mysql -u username -p
CREATE DATABASE mydatabase;
SHOW DATABASES;

**Starting the backend flask app:**

sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
sudo apt install mysql-client -y

(In the app.py channge the DATABASE hostname to localhost if sql is in instance,credentials and database names.)

python3 -m venv venv
source venv/bin/activate  # Activate the virtual environment

python3 app.py  #run this command in virtual env

To access the backend app
http://IPaddress:5000

**Frontend Configuration:**

Open the frontend folder
This is a react.js application and requires npm modules and libraries

cd src/
vim Login.js 
#change the database url to http://IPaddressofinstance:5000/login
vim Signup.js
#change the database url to http://IPaddressofinstance:5000/signup

sudo apt update
sudo apt install -y nodejs npm
node -v   # Check Node.js version
npm -v    # Check npm version

npm start


We can access the frontend app with
http://IPofinstance:3000

**Deployment with containers**
Use the instance with 2vCPUs t2.large and 15 GB volume

**Docker container creation for mysql:**
Create a mysql container with the below command

sudo docker run -d  --name mysql-container -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=kartikdatabase  -e MYSQL_USER=kartikuser -e MYSQL_PASSWORD=password -p 3300:3306 mysql:latest || true


**Backend docker container**

docker inspect containerid  # use the IP address of the mysql container and paste it in the app.py of the backend.As we are not using the localhost or external RDS so we have to use the IP address of the mysql container.

Run the docker commands
docker build -t backend .   #creates a docker images of backend
docker run -d -p 5000:5000 backend  ## creates a docker container of the backend.

curl http://localhost:5000 (or)
http://IPofinstance:5000

**Frontend docker container**

docker build -t frontend .
docker run -d -p 3000:80 --name frontend-container frontend


Make changes in Login.js and Signup.js 
change the backend configuration to http://publicIPofinstance:5000/signup    do similarly to login.js also

If you are using the docker network then use the docker "containername:5000" instean of IPofinstance

access the frontend with IP of the instance:3000 




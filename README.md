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
Use the instance with 2CPUs t2.nano or t3.medium

**Docker container creation for mysql:**
Create a mysql container with the below command

sudo docker run -d  --name mysql-container -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=kartikdatabase  -e MYSQL_USER=kartikuser -e MYSQL_PASSWORD=password -p 3300:3306 mysql:latest || true

docker inspect containerid  # use the IP address of the mysql container and paste it in the app.py of the backend.As we are not using the localhost or external RDS so we have to use the IP address of the mysql container. 

**Backend docker container**

Run the docker commands
docker build -t backend .   #creates a docker images of backend
docker run -d -p 5000:5000 backend  ## creates a docker container of the backend.

use curl IPof the backendcontainer:5000  to access the backend.

**Frontend docker container**

docker build -t frontend .
docker run -d -p 3000:3000 frontend

access the frontend with IP of the instance:3000 




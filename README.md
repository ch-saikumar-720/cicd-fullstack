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



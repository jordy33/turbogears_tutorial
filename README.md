## TURBO GEARS 2.3 ##

How to setup:

* Ubuntu 16.04 install:

```objc
sudo apt-get -y update && sudo apt-get -y upgrade
sudo apt-get -y install python-minimal
sudo apt-get -y install python-dev
sudo apt-get -y install python-virtualenv
sudo apt-get -y install python-pip
pip install --upgrade pip

sudo apt-get -y install build-essential python-dev libyaml-dev
sudo apt-get -y install libssl-dev libffi-dev
sudo apt-get -y install libmysqlclient-dev
sudo apt-get -y  install libjpeg-dev
sudo apt-get -y install supervisor
sudo systemctl enable supervisor
sudo systemctl start supervisor

sudo service apparmor stop
sudo update-rc.d -f apparmor remove
sudo apt-get -y remove apparmor apparmor-utils
sudo apt-get -y install mysql-server
# pick user root password
sudo apt-get -y install mysql-client
```

* Setup mysql

mysql -u root -p
```objc
CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'qazwsxedc';
GRANT ALL PRIVILEGES ON * . * TO 'myuser'@'localhost';
```
mysql -u myuser -p
```objc
create database mydatabase
```

* Create project using Turbogears 2.3:

mkdir myproject  
cd myproject  
virtualenv venv  
source venv/bin/activate  
pip install turbogears2 tg.devtools MySQL-python

```objc
The quickstart command creates the Turbogears project also the command provides a bunch of options 
to choose which template engine to use, which database engine to use various other options:

optional arguments:
  -a, --auth            add authentication and authorization support
  -n, --noauth          No authorization support
  -m, --mako            default templates mako
  -j, --jinja           default templates jinja
  -k, --kajiki          default templates kajiki
  -g, --geo             add GIS support
  -p PACKAGE, --package PACKAGE
                        package name for the code
  -s, --sqlalchemy      use SQLAlchemy as ORM
  -i, --ming            use Ming as ORM
  -x, --nosa            No SQLAlchemy
  --disable-migrations  disable sqlalchemy-migrate model migrations
  --enable-tw1          use toscawidgets 1.x in place of 2.x version
  --skip-tw             Disables ToscaWidgets
  --skip-genshi         Disables Genshi default template
  --minimal-quickstart  Throw away example boilerplate from quickstart project
```

* The following will create all the project files and structure according the parameters passed  

gearbox quickstart -s -a -m myprojectname  
cd myprojectname


```objc
The file setup.py (created within the project structure) contains all the modules of the project. 
If you need modules add in the list install-requires contained in setup.py (Example below)
Pip install -e . "what it does is to take the install_requires list to install the required modules"
Every time you add a new module you must run again pip install -e .

setup.py example:
install_requires = [
    "TurboGears2 >= 2.3.9",
    "Beaker >= 1.8.0",
    "Kajiki >= 0.3.5",
    "Mako",
    "zope.sqlalchemy >= 0.4",
    "sqlalchemy",
    "alembic",
    "repoze.who",
    "tw2.forms",
    "tgext.admin >= 0.6.1",
    "WebHelpers2",
    "transaction <= 2.0.2"
]
```

* The following command will install all the modules required in the project: 
* Warning!!! be sure to add the transaction<=2.0.2 on install_requires 

pip install -e .

```objc
Modify the file development.ini (created within the project structure) and change:
sqlalchemy.url = sqlite:///%(here)s/devdata.db
with
sqlalchemy.url = mysql://myuser:qazwsxedc@127.0.0.1/mydatabase

Also change host and port
host = 0.0.0.0
port = 80

This will allow to be accessed in all networks on the port 80

```

* Its necessary to have a production file just copy the development.ini (is the same format)  

cp development.ini production.ini

```objc
The gearbox setup-app command runs the websetup.setup_app function of your project to initialize the database schema and data.
By default the setup-app command is run on the development.ini file, to change this provide a different one to the --config option:
```

* The following command will initialize the database  

gearbox setup-app -c development.ini

```objc
The serve command provides a bunch of options to start the serve in daemon mode, automatically restart the application whenever the code changes and many more:

optional arguments:
  -c CONFIG_FILE, --config CONFIG_FILE
                        application config file to read (default:
                        development.ini)
  -n NAME, --app-name NAME
                        Load the named application (default main)
  -s SERVER_TYPE, --server SERVER_TYPE
                        Use the named server.
  --server-name SECTION_NAME
                        Use the named server as defined in the configuration
                        file (default: main)
  --daemon              Run in daemon (background) mode
  --pid-file FILENAME   Save PID to file (default to gearbox.pid if running in
                        daemon mode)
  --reload              Use auto-restart file monitor
  --reload-interval RELOAD_INTERVAL
                        Seconds between checking files (low number can cause
                        significant CPU usage)
  --monitor-restart     Auto-restart server if it dies
  --status              Show the status of the (presumably daemonized) server
  --user USERNAME       Set the user (usually only possible when run as root)
  --group GROUP         Set the group (usually only possible when run as root)
  --stop-daemon         Stop a daemonized server (given a PID file, or default
                        gearbox.pid file)
```
* The following command will start the project in production mode (will run without debug)  

gearbox serve -c production.ini --reload  

```objc
or  
```
* The following command will start the project in development mode (will run with debug)  

gearbox serve -c development.ini --reload    

```objc
or  
```    
* The following command will start the project in production mode daemon (will run in the background)      

gearbox serve -c production.ini --daemon

```objc
or  
```    
* The following command will stop the project in production mode daemon        

gearbox serve -c production.ini --stop-daemon

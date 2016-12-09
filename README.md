# Linux-Server-Configuration
IP Address: 35.160.197.79

SSH Port: 2200

URL: http://ec2-35-160-197-79.us-west-2.compute.amazonaws.com

Software Installed:

	apache2
	libapache2-mod-wsgi
	python-psycopg2
	Git
	Flask
	Virtualenv
	Pip
	httplib2
	requests
	flask-seasurf
	oauth2client
	sqlalchemy
	libpq-dev
	python-dev
	Flask-SQLAlchemy
  
Summary of Main Configuration Changes Made (Note: this doesn’t include removing errors that I had and debugging):

	Created a new user named grader
	  sudo adduser grader
	  Gave the grader the permission to sudo
	    sudo nano /etc/sudoers.d/grader
	    add: "grader ALL=(ALL) NOPASSWD:ALL" to file
	    
	Updated all currently installed packages
	  apt-get update
	  sudo sudo apt-get upgrade
	  
	Changed the SSH port from 22 to 2200
	  sudo nano /etc/ssh/sshd_config
	  Changed line “Port 22” to “Port 2200"
	  
	Configured the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
	  sudo ufw default deny incoming
	  sudo ufw default allow outgoing
	  sudo ufw allow 2200
	  sudo ufw allow 80
	  sudo ufw allow 123
	  
	Configured the local timezone to UTC
	  Used “date” command and found that it was already configured to UTC
	  
	Installed and configured Apache to serve a Python mod_wsgi application
	  sudo apt-get install apache2
	  sudo apt-get install libapache2-mod-wsgi
	  
	Installed and configured PostgreSQL:
	  sudo apt-get install postgresql postgresql-contrib
	  Changed database connection in database_setup.py, __init__.py, and add items.py after step 11:C
	  Disabled remote connections
	  Checked in /etc/postgresql/9.3/main/pg_hba.conf to make sure no connections were allowed
	
	Created a new user named catalog that had limited permissions to the catalog application database
	sudo adduser catalog
	  As postgres user in sql:
	    CREATE USER catalog WITH PASSWORD ‘Passwd goes here’;
	    Allowed user to make tables: ALTER USER catalog CREATED;
	    Create Database: CREATE DATABASE catalog WITH OWNER catalog;
	    In database revoked all rights and granted access to only catalog: 
	    REVOKE ALL ON SCHEMA public FROM public;
	    GRANT ALL ON SCHEMA public TO catalog;
	
	Installed git, cloned and setup my Catalog App project 
	  Installed Git and connected github account
	  Enabled Apache to serve Flask applications:
	    sudo apt-get install libapache2-mod-wsgi python-dev
	  Cloned Github Rep in /var/www/Catalog/Item-Catalog
	  Installed Pip and Virtualenv
	  Setup virtualvenv in Item-Catalog
	  Installed Flask inside environment
	  Configured/enabled new catalog virtual host:
	    sudo nano /etc/apache2/sites-available/catalog.conf
	  Copied first host’s text into second’s and added/changed server’s name and admin, WSGI, directory and static file paths
	  Created wsgi file in /var/www/Catalog/ and added basic wsgi file template and changed sys.path to Item-Catalog’s path
	  installed needed modules/packages
	  Edited catalog.conf file by adding my hostname which I got from http://www.hcidata.info/host2ip.cgi
	  Added hostname/ public ip address to Authorized Javascript Origins and host name + oauth2callback to the Authorized redirect URIs in Google’s Developer Console


Resources:

	https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps


	https://github.com/stueken/FSND-P5_Linux-Server-Configuration


	http://www.hcidata.info/host2ip.cgi


	https://discussions.udacity.com/t/interact-with-database/29136/2


	https://discussions.udacity.com/t/apache-cant-find-file-in-main-directory/24498/5


	https://discussions.udacity.com/t/getting-the-rest-of-my-app-to-run-almost-there/30237


	https://discussions.udacity.com/t/how-to-move-a-flask-app-from-using-a-sqlite3-db-to-postgresql/7004/6


	https://discussions.udacity.com/t/cannot-setup-database-using-postgresql-b-c-of-user-authorization/30430/11


	https://discussions.udacity.com/t/step-10-catalog-user/157413/4


	https://discussions.udacity.com/t/step-10-catalog-user/157413


	https://discussions.udacity.com/t/create-a-new-user-named-catalog-that-has-limited-permissions-to-your-catalog-application-database/46363/2


	https://discussions.udacity.com/t/disable-remote-login-of-root-user-limited-permission-role/43644


	https://discussions.udacity.com/t/question-on-postgresql-configuration/39314/2


	https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps


	https://help.github.com/articles/set-up-git/#platform-linux


	https://help.ubuntu.com/community/UFW


	http://askubuntu.com/questions/94102/what-is-the-difference-between-apt-get-update-and-upgrade


	https://discussions.udacity.com/t/wsgi-error-no-module-named-catalog/35734


	https://discussions.udacity.com/t/i-dont-see-my-application-i-only-get-hello-world-its-working/175954


	https://discussions.udacity.com/t/invalidclientsecretserror-file-not-found-client-secrets-json/32482


	https://www.postgresql.org/docs/8.0/static/sql-alteruser.html


	https://discussions.udacity.com/t/p5-how-i-got-through-it/15342/2


	http://dba.stackexchange.com/questions/35316/why-is-a-new-user-allowed-to-create-a-table


	https://docs.python.org/2/tutorial/modules.html


	http://stackoverflow.com/questions/30969296/flask-from-app-import-app


	https://www.loggly.com/ultimate-guide/access-and-error-logs/


	http://blog.trackets.com/2013/08/19/postgresql-basics-by-example.html


	https://discussions.udacity.com/t/disallowing-remote-connections-to-my-postgresql-db/36364


	https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04


	http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name


	https://discussions.udacity.com/t/specific-questions-on-project-5/41095/


	https://discussions.udacity.com/t/configuring-utc-time-zone/35776


	https://discussions.udacity.com/t/how-to-connect-grader-login-in-the-root/32659/


	https://help.ubuntu.com/community/Sudoers


	https://discussions.udacity.com/t/configuring-a-linux-server/166552


	https://discussions.udacity.com/t/how-to-login-to-my-aws-virtual-server-as-new-user-grader/201164



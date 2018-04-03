## Introduction

This project serves as documentation for the server which serves the item catalog.

You can find the server here [udacity.lost-world.dk](http://udacity.lost-world.dk/).

## Services available

### SSH
SSH is available on port 2200 with the user `grader`, you can only log in with an SSH-Key.
```
$ ssh grader@udacity.lost-world.dk -p 2200
```

### HTTP (Item catalog)
The project itself is served on port 80 over HTTP. For accessing the Item 
Catalog just visit [udacity.lost-world.dk](http://udacity.lost-world.dk/).


## Software installed
- Python 3 with PIP
- Git
- UFW
- Apache 2
- MySql


## Configuration

### Firewall
The firewall is configured to blacklist everything as default, so the following
rules are the only ports which are open.

- Added SSH (2200/TCP)
- Added HTTP (80/TCP)
- Added NTP (123/TCP)

### User
There has been created a user called `grader`, this user has sudo access and
can only log in with the SSH-key which has been attached to the submission of 
this task.

### MySQL
The database has only local accessed, so only users which are logged into the 
server can access it.

### Apache
Apache has been set to serve our flask application on [udacity.lost-world.dk](http://udacity.lost-world.dk/)
with the use of WSGI.

Our WSGI file looks like this 
```python
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/Udacity-item_catalog")

from udacity_item_catalog import app as application
```

and our Apache site configuration looks like this
```
<virtualhost *:80>
    ServerName udacity.lost-world.dk

    WSGIDaemonProcess webtool user=www-data group=www-data threads=5 home=/var/www/udacity-item_catalog/
    WSGIScriptAlias / /var/www/udacity-item_catalog/item_catalog.wsgi

    <directory /var/www/udacity-item_catalog>
        WSGIProcessGroup webtool
        WSGIApplicationGroup %{GLOBAL}
        WSGIScriptReloading On
        Order deny,allow
        Allow from all
    </directory>
</virtualhost>
```

### Resources
Thanks to the following sites for tutorials on how to set up the system

- [digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [flask.pocoo.org](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/)
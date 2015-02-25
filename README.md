# Chelsea: Workbench Installation

FarCry workbench for Chelsea project development.

## Setting up the Daemonite Vagrant Workbench

This only needs to be the once per workstation. If you already have a working Vagrant environment you can skip this part.

1.  Install VirtualBox: https://www.virtualbox.org/wiki/Downloads
2.  Install Vagrant http://www.vagrantup.com/downloads.html
3.  Install vagrant-hostmanager plugin: `vagrant plugin install vagrant-hostmanager`


## Setting Up Project Environment

Requires an existing installation of the standard Daemonite Vagrant Workbench.

    git clone https://github.com/modius/farcry-env-chelsea.git
    cd farcry-env-chelsea
    git submodule update --init
    vagrant up

_note: you may be asked for a password the first time you run vagrant up. this is the host manager plugin asking for permission to modify your hosts file._

# Running the Workbench

## Starting the App 

Change directory to the root of the project and `vagrant up`.  

Site access:

- http://chelsea.local (or http://IPADDRESS)
- http://chelsea.local/webtop (farcry/farcry)
- http://chelsea.local/lucee/admin/server.cfm (pwd: farcry)

To restart Lucee server `vagrant ssh` onto the virtual:

```
sudo /etc/init.d/lucee_ctl restart
```

The website listens for `*.vagrant.com` host headers so `vagrant share` should work :)

## Refreshing Database

This VM installs the project database from the install files in the project.
Therefore re-exporting and committing the installation files is equivilent to
updating the development environment.

To refresh the database in the Vagrant VM it's easiest to DROP and CREATE the database.

```
vagrant ssh
mysql -u root
...
mysql> DROP DATABASE projectdatabase;
Query OK, 82 rows affected (0.08 sec)
mysql> CREATE DATABASE projectdatabase;
Query OK, 1 row affected (0.00 sec)
quit
```

Then <http://localhost:8080/webtop/install> to reinstall FarCry app.


# Simple Deployment Pipeline

Assuming you have an existing installation on a production Ubuntu box you can update the environment to the exact tags and/or branches nominated in the project `./install/deploy.txt` file by running the `update.yml` playbook.

1.  check code into repo
2.  Logon to production server:
    `ssh -i ~/.ssh/projectname.pem ubuntu@000.000.000.000`
3.  Run update playbook
    `sudo ansible-playbook -c local /opt/provision/provisioning/update.yml`



# Vagrant Setup

* Update ruby (>= 2.3.0) & gems (>= 2.5.1)

## Getting Started

* Install VirtualBox.
https://www.virtualbox.org/
* Install Vagrant.
http://www.vagrantup.com/
* Install the vagrant-hostsupdater plugin. (Windows does not allow to change hosts files. Please add pm-vagrant.local 192.168.33.10 by yourself!)
```
$ vagrant plugin install vagrant-hostsupdater
```
* Run `vagrant up`.
* Visit WordPress on the Vagrant in your browser
Visit http://pm-vagrant.local/ or http://192.168.33.10/

## Environments

### phpMyAdmin
Needs to be installed manually `sudo apt-get install phpmyadmin`
http://pm-vagrant.local/phpmyadmin
* Database: `wordpress`
* Username: `admin`
* Password: `admin`

or
* Username: `root`
* Password: `root`

### forward ssh key
* run `ssh-add ~/.ssh/pm-vm-NAME.pem`

### ssh, access project within vagrant vm, npm

* run `vagrant ssh`
* run `cd /vagrant`
* run `npm install`

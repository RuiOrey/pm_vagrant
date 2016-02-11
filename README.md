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

http://pm-vagrant.local/ (http://192.168.33.10) - this folder should now point to your projects folder (/pm_vagrant/projects).
The project folder is also mounted inside your vm:

* run `vagrant ssh`
* run `cd /vagrant/projects`

### forward ssh key

In order to access the P&M Git repos from within our VM
* run (in your host os): `ssh-add ~/.ssh/pm-vm-NAME.pem`

### install phpMyAdmin

Needs to be installed manually inside the VM: `sudo apt-get install phpmyadmin`
Then, navigate to http://pm-vagrant.local/phpmyadmin in your host os. 

## add Projects

Move to your projects folder and clone th project you want to add.
This project should contain a confi.yaml, containing that apache config (change at least the 'myProject' strings):
```yaml
apache:
    modules:
        - proxy_fcgi
        - rewrite
        - headers
    vhosts:
        projects_myProject:
            servername: myProject.local
            serveraliases:
                - myProject.local
            docroot: /var/www/projects/myProject/source
            port: '80'
            setenv:
                - 'APP_ENV dev'
            setenvif:
                - 'Authorization "(.*)" HTTP_AUTHORIZATION=$1'
            custom_fragment: ''
            ssl: '0'
            ssl_cert: ''
            ssl_key: ''
            ssl_chain: ''
            ssl_certs_dir: ''
            ssl_protocol: ''
            ssl_cipher: ''
            aliases:
                -
                    alias: /phpmyadmin
                    path: /usr/share/phpmyadmin
            directories:
                avd_xeehkr0antpp:
                    path: /var/www/projects/pm_3deluxe/source
                    options:
                        - Indexes
                        - FollowSymlinks
                        - MultiViews
                    allow_override:
                        - All
                    require:
                        - 'all granted'
                    custom_fragment: ''
                    files_match:
                        avdfm_sgzidr20lopr:
                            path: \.php$
                            sethandler: 'proxy:fcgi://127.0.0.1:9000'
                            custom_fragment: ''
                            provider: filesmatch
                    provider: directory
                avd_ghjon5wr92mr:
                    path: /usr/share/phpmyadmin
                    options:
                        - Indexes
                        - FollowSymlinks
                        - MultiViews
                    allow_override:
                        - All
                    require:
                        - 'all granted'
                    custom_fragment: ''
                    files_match:
                        avdfm_kadnclhtbktw:
                            path: \.php$
                            sethandler: 'proxy:fcgi://127.0.0.1:9000'
                            custom_fragment: ''
                            provider: filesmatch
                    provider: directory   
```

Be aware, that the apache module section OVERWRITES the modules config of the global config.yaml!

Now, include this config in pm_vagrant/Vagrantfile add a line 
```configCustom.deep_merge!(YAML.load_file("#{dir}/projects/myProject/config.yaml")) # add such a row for each project``` 
under the line 
```# ADD YOUR PROJECTS HERE``` 

The config will be included after running:
```
$ vagrant reload --provision
```
Then, you should have a generated config-custom.yaml file in the folder pm_vagrant/phphpet, containing a merge of all the config.yaml files of your projects.

## Troubleshooting

#### Always use an up-to-date vagrant box

If vagrant tells you that your box can be updated:
* run `vagrant destroy`
* run `vagrant box update`
* run `vagrant up`

#### Problems with Vagrant Plugins

If Problems come from your ~/.vagrant.d/gems/ folder, you probaly need to update rubygems 
* run `sudo gem install rubygems-update`
* run `sudo update_rubygems`
* run `sudo gem update --system`

It could also help to remove your ~/.vagrant.d/gems folder and ~/.vagrant.d/plugins.json file
and reinstall your vagrant plugins (see Getting Started).

## How To Install Recon-ng on Kali Linux Instance with Vagrant on Local Machine?

### Step 1: Install Kali linux VM on local Vagrant VM

#### Login Vagrant and install Kali VM

Click `Git bash` on your local computer, go to your Vagrant directory where contains Vagrantfile:

`cd ~/Documents/Cybersecurity-Bootcamp/Linux-Module`

Make sure Vagrantfile file in this directory. Every vagrant command should be run from the directory containing Vagrantfile provided by your instructor.

Vagrantfile:
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "kalilinux/rolling"
  config.vm.box_check_update = false
  config.vm.hostname = "kali-linux"
  config.vm.provider "virtualbox" do |vb|
  # Display the VirtualBox GUI when booting the machine
     vb.gui = false
     vb.memory = "2048"
   end
end
```

Run `vagrant up`

Then run `vagrant ssh` to login Kali VM:

![vagrant ssh](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/vagrant%20ssh.png)

`vagrant@kali-linux:~`

Verify docker-compose.yml inside '`recon-ng`' folder:

`vagrant@kali-linux:~/recon-ng$ cat docker-compose.yml`

docker-compose.yml

```
version: '3.7'

services:

    web:
        build: .
        image: recon-ng
        container_name: recon-ng
        ports:
            - '5000:5000'
        command: ./recon-web --host 0.0.0.0
        volumes:
            - .:/recon-ng
            - ~/.recon-ng:/root/.recon-ng
        environment:
            - REDIS_URL=redis://redis:6379/0
        depends_on:
            - redis

    worker:
        image: recon-ng
        command: rq worker -u redis://redis:6379/0 recon-tasks
        volumes:
            - .:/recon-ng
            - ~/.recon-ng:/root/.recon-ng
        environment:
            - REDIS_URL=redis://redis:6379/0
        depends_on:
            - redis

    redis:
        image: redis
```

### Step 2: Install Recon-ng

#### Introduction

Recon-ng (Web Reconnaissance Framework) is an powerful tool for Open Source Intelligence Gathering (OSINT), a full-featured Web Reconnaissance Framework written in Python, with interface similar to Metasploit.

#### Preparation
In Kali Linux it’s very simple to install.
Since this is the first time to install on my local machine, I run following commands:
```
vagrant@kali-linux:~$ sudo apt update
vagrant@kali-linux:~$ sudo apt upgrade
```

![sudo apt update and upgrade](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/kali-linux.png)

![sudo apt upgrade](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/kali-linux-1.png)

```
Do you want to continue? [Y/n] y
```

Python3 is required:

`vagrant@kali-linux:~$ sudo apt install -y git python3-pip`
![Python3](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/python3-pip.png)

`vagrant@kali-linux:~/recon-ng$ pip3 install PyPDF3 pyaes bs4`

![pip3 install PyPDF3 pyaes bs4](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/kali-linux-3.png)

#### Recon-ng Installation

- Install in Kali Linux

	In Kali Linux it's very somple to install. Just type command as follows:

	`vagrant@kali-linux:~$ sudo apt install recon-ng`

- Install from source
```
# Clone the recon-ng repository
vagrant@kali-linux:~$ git clone https://github.com/lanmaster53/recon-ng.git
	
# Change into the recon-ng directory
vagrant@kali-linux:~$ cd recon-ng
vagrant@kali-linux:~/recon-ng$
```

![Clone the recon-ng repository](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/Clone-the-recon-ng-repository.png)

In order to run everything smoothly, make sure to check for the presence of the following dependencies in REQUIREMENTS file.

`vagrant@kali-linux:~/recon-ng$ cat REQUIREMENTS`

```
pyyaml
dnspython
lxml
mechanize
requests
flask
flask-restful
flasgger
dicttoxml
XlsxWriter
unicodecsv
rq
jsonrpclib
slowaes
```

Use command `pip3` (not `pip`) to install dependencies for `recon-ng` modules.

```
#Install dependencies

vagrant@kali-linux:~/recon-ng$ pip3 install -r EQUIREMENTS
```

![Install Dependencies](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng-pip3-install-dependencies.png)
```
#Launch recon-ng
vagrant@kali-linux:~/recon-ng$ ./recon-ng
```

If there’s no errors, the Recon-NG console will be loaded and framework banner will appear:

![Launch recon-ng](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng.png)

#### Recon-ng modules

Recon-ng is a completely modular framework.

- Recon modules – for reconnaissance activities;
- Reporting modules – for reporting results on a file;
- Import modules – for importing values from a file into a database table;
- Exploitation modules – for explotation activities;
- Discovery modules – for discovery activities.

To display recon-ng commands, type `help`:

![recon-ng commands](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng-help.png)

To display a list of installed modules, just type `modules search` command: 

`[recon-ng][default] > modules search`

##### Attempt to install some modules

![Install recon-ng modules](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng-install-module-hackertarget.png)

As the warning messages displayed, `shodan` dependency is required.

- Exit recon-ng, add `shodan` to REQUIREMENTS file, 

```
vagrant@kali-linux:~/recon-ng$ vi REQUIREMENTS

pyyaml
dnspython
lxml
mechanize
requests
flask
flask-restful
flasgger
dicttoxml
XlsxWriter
unicodecsv
rq
jsonrpclib
slowaes
shodan
```

- Run `pip3 install -r REQUIREMENTS` again.
- Launch Recon-ng:
 
	`vagrant@kali-linux:~/recon-ng$ ./recon-ng`

![Launch recon-ng](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng-v5.1.1.png)

Just explore those modules and you’ll soon become an expert.

![recon-ng modules](https://github.com/zwang21/How-To-Install-Recon-ng-on-Kali-Linux-Instance-with-Vagrant-on-Local-Machine/blob/main/images/recon-ng-modules.png)

#### Remove and reinstall recon-ng in Kali Linux

```
sudo apt update
sudo apt remove recon-ng
sudo apt install recon-ng
```
#### Exit recon-ng:

`[recon-ng][default] > exit`

Log out Kali Linux:

```
vagrant@kali-linux:~/recon-ng$ exit
logout
Connection to 127.0.0.1 closed.

HPE@HPE-HP MINGW64 /c/users/hpe/documents/cybersecurity-bootcamp/Linux-Module

$ vagrant suspend
==> default: Saving VM state and suspending execution...
```

### NOTES

Whenever you make a change to the Vagrantfile, restart the machine for the changes to take effect.

```
$ vagrant reload

To stop the instance, run
$ vagrant halt

If you would like to save the current state of the VM while stopping it, run
$ vagrant suspend

With this, you’ll return to exactly the same state at a later time when VM is started.

Destroy the Vagrant machine when done by running
$ vagrant destroy
```

### Wrapping Up

In this guide, we have shown you **"How To Install Recon-ng on Kali Linux Instance with Vagrant on Local Machine"**.

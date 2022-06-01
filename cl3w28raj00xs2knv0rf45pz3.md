## Bastion Automation Server

> Purpose: To provide a guide on how to set up a Bastion Server to be used on Network, Cloud and Other Automation tasks you may have. 

This server was setup using Ubuntu Server 22.04, but you may use the lists provided in this article to install them on your desired distribution. 

You may download this distribution at: https://ubuntu.com/download/server click on Option2 and Manual Server installation. 

>Note: I will not cover how to install this distribution, but I can advise not to install any of the packets that come with the distribution, like docker, as it may cause unexpected behavior that may conflict with my instructions below.  

You may proceed to install this OS on either bare metal, vmware, qemu or other method. It does not matter. 

For simplicity I have created differnt categories you should follow based on your needs. 


## ==> Code Editor
### Visual Studio Code. 

| Download | https://code.visualstudio.com/Download |

My recomendation goes to Visual Studio Code to be used as the prefered Code Editor. 

The following are the Extensions I recommend to install: 

```
	- Remote - SSH
	- Python
	- Python for VSCode
	- Python Auto Venv
	- Python Docstring Generator
	- Docker
	- Dracula Official
	- Remote WSL
	- Remote SSH
	- GitLab Workflow
	- JSON
	- Live Server
	- REST Client
	- XML Tools
	- YAML
	- YANG
	- Yangster
	- Better Jinja
	- autoDocstring
	- Code Runner
	- HashiCorp Terraform
	- Jupyter  
	- Markdown All in One  
```
<br />

## ==> Programming 

### Python: (Interpreted, High-Level and general purpose programming language)

| Documentation | https://docs.python.org/3/ | 

 - Make sure you have the most updated version of Python:

```
    python3 -V			(Check the current version)
	Python 3.10.4

	whereis python3		(Check folder installations)
	
    sudo apt-get install python-is-python3 
```
 - Add corresponding aliases (~/.bash_aliases or .bashrc)
  
```
    alias				(show current alias)
	echo alias python=python3 >> ~/.bashrc			
	echo alias pip=pip3 >> ~/.bashrc
	source ~/.bashrc	(loads the file)
```

 - Install PIP3: (Python Package Manager)
 
```
	sudo apt install python3-pip -y

	pip3 -V			(to check the current version)
	pip 22.0.2 from /usr/lib/python3/dist-packages/pip (python 3.10)
	
	sudo pip3 freeze   (to review your installed modules)
```

 - Update PIP3 and SetupTools: 

```
	sudo pip3 install --upgrade setuptools
	sudo pip3 install --upgrade pip
```

 - Install the following Modules/Libraries:
   
```
    pip3 freeze | grep <>
 	pip3 install 
		paramiko	(SSH API)
		netmiko		(SSH API)
		napalm		(Unified API to Network Devices)
		requests 	(Used for restconf)
		ncclient	(Used for netconf)
		xmltodict	(XML Parser, dictionary...)
		pylint		(Bug and quality checker)
		nornir
		django
```
    
 - Modiles/Libraries already integrated with Python:

```
		pprint		("pretty-print" Python Data Structures)
		lxml		(XML Parser ElementTree API) 
		pyyaml		(YAML Parser and emitter)
		jinja2		(Templating language for Python)
		unittest	(Python testing framework)
		pytest		(Python testing framework)
		json		(JavaScript Object Notation parser)
```
<br />

## ==> Projects

### Poetry (dependency management and packaging in Python)

| Documentation | https://python-poetry.org/docs/ | 

```
	curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3

	source $HOME/.poetry/env
	
	poetry --version or ~/.poetry/bin$ ./poetry --version		(Check the current version)
    Poetry version 1.1.1
```
<br />

## Cookiercutter (project templates)

| Documentation | https://cookiecutter.readthedocs.io/en/2.1.0/ | 


```
	pip install --user cookiecutter
	cookiecutter -V
	Cookiecutter 1.7.3 from /home/server/.local/lib/python3.10/site-packages (Python 3.1)
    Poetry version 1.1.1
```

## ==> Version Control 

### Git: (version-control system for tracking changes in source code)

| Documentation | https://git-scm.com/doc | 

 - Make sure you have the latest version of git

```
	sudo apt install git-all
	git 
	git --version			(Check the current version)
	git version 2.34.1
```

## ==> Containers 

### Docker (OS-level virtualization to deliver software in packages called containers)

| Documentation | https://docs.docker.com/engine/install/ubuntu/ | 

- Install Docker:

```
	sudo apt-get install \
		apt-transport-https \
		ca-certificates \
		curl \
		gnupg-agent \
		software-properties-common
		
	sudo add-apt-repository \
	   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
	   $(lsb_release -cs) \
	   stable"
   
	sudo apt-get update
	sudo apt-get install docker-ce docker-ce-cli containerd.io -y
	
	docker --version
	docker version
	Docker version 20.10.14, build a224086349
```

If you don't want to use SUDO every single time you call docker you may consider changing Permissions: 

```
	usermod -aG docker $USER
	chgrp docker /usr/bin/docker /usr/bin/docker-compose
	chmod g=rwx /var/run/docker.sock
	chmod g=rwx /usr/bin/docker /usr/bin/docker-compose
```

Test your installation: 

```
	docker run hello-world			(Test installation)
	docker images					(Show current docker images)
```
<br />

### Docker-compose (To define and run multi-container docker applications)
| Documentation | https://docs.docker.com/compose/ | 

```
	sudo apt-get install docker-compose

	docker-compose version

	docker-compose version 1.29.2, build unknown
	docker-py version: 5.0.3
	CPython version: 3.10.4
	OpenSSL version: OpenSSL 3.0.2 15 Mar 2022
```
<br />

##  ==> Automation-Orquestration 

### Ansible (automation tool)

| Documentation | https://docs.ansible.com/ | 

```
	sudo apt install ansible -y

	ansible --version				(Check version)
	ansible 2.10.8

	- Test Ansible install: 
	ansible localhost -m ping
	ansible all -i "localhost," -m debug -a "msg='Hello Ansible'"
```
<br />

### Terraform (Infraestructure as Code IaC)

| Documentation | https://www.terraform.io/docs | 

How to install: https://learn.hashicorp.com/tutorials/terraform/install-cli

```
	sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
	curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
	sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
	sudo apt-get update && sudo apt-get install terraform

	terraform -version
	Terraform v1.2.1
	on linux_amd64

	terraform -help
```
<br />

##  ==> Cloud 

### Azure CLI 
| Documentation | https://docs.microsoft.com/en-us/cli/azure/ | 

How to Install: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

```
	az upgrade
	az --version
	azure-cli                         2.37.0
```
<br />

### AWS CLI 

| Documentation | https://docs.aws.amazon.com/index.html | 
How to install: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html 

```
	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
	unzip awscliv2.zip
	sudo ./aws/install

	To Update:
	sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update

	aws --version
	aws-cli/2.7.3 Python/3.9.11 Linux/5.15.0-33-generic exe/x86_64.ubuntu.22 prompt/off

```
<br />


## This is it, I hope you enjoyed it !!!
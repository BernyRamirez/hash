## Bastion Automation Server

> Purpose: To provide a guide on how to set up a Bastion Server to be used on Network, Cloud and Other Automation tasks you may have. 

This server was setup using Ubuntu Server 22.04, however the majority of the software if not all of it mentioned in this article can be installed on your desired distribution. 

You may download this distribution at this [Ubuntu Site](https://ubuntu.com/download/server) click on Option2 and Manual Server installation. 

>Note: I will not cover how to install this distribution, but I can advise not to install any of the packets that come with the distribution, like docker, as it may cause unexpected behavior that may conflict with my instructions below.  

You may proceed to install this OS on either bare metal, vmware, qemu or other method. It does not matter. 

For simplicity I have created different categories you should follow based on your needs. 

Before proceeding, make sure you update all of your packages by fetching the latest version of the package list from your distro's software repository, and any third-party repositories *(apt update)* and then download and install the updates for each outdated package and dependency on your system *(apt upgrade)*

```
   sudo apt update && sudo apt upgrade
```

## > Code Editor
_________________
### Visual Studio Code. 

| [Download](https://code.visualstudio.com/Download) |

My recommendation goes to Visual Studio Code to be used as the preferred Code Editor. 

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

## > Programming 
_________________
### Python: (Interpreted, High-Level and general-purpose programming language)

| [Documentation](https://docs.python.org/3/) |

 - Make sure you have the most updated version of Python:

```
sudo apt install software-properties-common  
sudo add-apt-repository ppa:deadsnakes/ppa  
sudo apt update
sudo apt install python3    

    python3 -V			(Check the current version)
	Python 3.10.4

	whereis python3		(Check folder installations)
	
    sudo apt-get install python-is-python3 
    sudo apt-get install python-dev
```
 - Add corresponding aliases (~/.bash_aliases or .bashrc)
  
```
    alias				(show current alias)
	echo alias python=python3 >> ~/.bashrc			
	echo alias pip=pip3 >> ~/.bashrc
	source ~/.bashrc	(loads the file)
```

-Add corresponding ENV variable only if you have issues with the current ENV PATH, but this should work as by default /usr/bin is added into the PATH. 

```
export PATH="$PATH:/usr/bin/python"
 env | grep py
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

- Create a requirements file which will include all the packages we require to install:

```
touch requirements.txt
vi requirements.txt

###### All packages will install the latest version ######
# If you want to install an specific version you should specify it after the name. 

ipython        #(Interactive python)
paramiko     #(SSH API)
netmiko       #(SSH API)
napalm        #(Unified API to Network Devices)
requests     #(Used for restconf)
ncclient        #(Used for netconf)
xmltodict    #(XML Parser, dictionary...)
pylint            #(Bug and quality checker)
nornir          #(Automation framework)
nornir_utils  #(nornir utilities)
django        #(Web Framework)

prettyprint          #("pretty-print" Python Data Structures)
lxml              #(XML Parser ElementTree API) 
pyyaml      #(YAML Parser and emitter)
jinja2          #(Templating language for Python)
unittest2      #(Python testing framework)
pytest          #(Python testing framework)
jsons         #(JavaScript Object Notation parser)


:wq!
```

 - Install the following Modules/Libraries:
```
    pip3 install -r requirements.txt   

    python -m pip list     
    pip3 freeze
```

## > Projects
_________________
### Poetry (dependency management and packaging in Python)

| [Documentation](https://python-poetry.org/docs/) |

```
	curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3

	source $HOME/.poetry/env
	
	poetry --version or ~/.poetry/bin$ ./poetry --version		(Check the current version)
    Poetry version 1.1.1
```

## Cookiercutter (project templates)

| [Documentation](https://cookiecutter.readthedocs.io/en/2.1.0/) |

```
	pip install --user cookiecutter
	cookiecutter -V
	Cookiecutter 1.7.3 from /home/server/.local/lib/python3.10/site-packages (Python 3.1)
    Poetry version 1.1.1
```

## > Version Control 

### Git: (version-control system for tracking changes in source code)

| [Documentation](https://git-scm.com/doc) |

 - Make sure you have the latest version of git

```
	sudo apt install git-all
	git 
	git --version			(Check the current version)
	git version 2.34.1
```

## > Containers 
_________________
### Docker (OS-level virtualization to deliver software in packages called containers)

| [Documentation](https://docs.docker.com/engine/install/ubuntu/) |

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

### Docker-compose (To define and run multi-container docker applications)
| [Documentation](https://docs.docker.com/compose/) |

```
	sudo apt-get install docker-compose

	docker-compose version

	docker-compose version 1.29.2, build unknown
	docker-py version: 5.0.3
	CPython version: 3.10.4
	OpenSSL version: OpenSSL 3.0.2 15 Mar 2022
```

##  > Automation-Orquestration 
_________________
### Ansible (automation tool)
| [Documentation](https://docs.ansible.com/) |

```
	sudo apt install ansible -y

	ansible --version				(Check version)
	ansible 2.10.8

	- Test Ansible install: 
	ansible localhost -m ping
	ansible all -i "localhost," -m debug -a "msg='Hello Ansible'"
```

### Terraform (Infrastructure as Code IaC)
| [Documentation](https://www.terraform.io/docs) |

How to [Install](https://learn.hashicorp.com/tutorials/terraform/install-cli)

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

## > Cloud 
_________________
### Azure CLI 
| [Documentation](https://docs.microsoft.com/en-us/cli/azure/) |

How to [Install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt)

```
	curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
	az upgrade
	az --version
	azure-cli                         2.37.0
```

### AWS CLI 

| [Documentation](https://docs.aws.amazon.com/index.html) |

How to [Install](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

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

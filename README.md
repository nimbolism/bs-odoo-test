[![Doodba deployment](https://img.shields.io/badge/deployment-doodba-informational)](https://github.com/Tecnativa/doodba)
[![Last template update](https://img.shields.io/badge/last%20template%20update-v8.1.0-informational)](https://github.com/Tecnativa/doodba-copier-template/tree/v8.1.0)
[![Odoo](https://img.shields.io/badge/odoo-v17.0-a3478a)](https://github.com/odoo/odoo/tree/17.0)
[![BSL-1.0 license](https://img.shields.io/badge/license-BSL--1.0-success})](LICENSE)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://pre-commit.com/)

# doodba-test-odoo - a Doodba deployment

This project is a Doodba scaffolding. Check upstream docs on the matter:

- [General Doodba docs](https://github.com/Tecnativa/doodba).
- [Doodba copier template docs](https://github.com/Tecnativa/doodba-copier-template)
- [Doodba QA docs](https://github.com/Tecnativa/doodba-qa)

# Credits

This project is maintained by: Doodba-testers



#installing python and invoke on the Ubuntu-24.04 system
sudo apt update
sudo apt install python3
sudo apt install python3-pip

#initializing virtual environment (using venv) for python
#1. install venv for ubuntu
sudo apt install python3.12-venv

#2. create the virtual env named .odoo and activate it
python3 -m venv .odoo
source .odoo/bin/activate

#3. since virtual env is activated we can now start installing pipx and other tools!
python3 -m pip install pipx

#4. install tools using pipx
pipx install copier
pipx install invoke
pipx install pre-commit
pipx ensurepath

#5. install docker engine
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

#6. getting the engine
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#7. to check if docker is installed run:
sudo docker run hello-world

#8. check if git is installed (it should be installed already)
git -v

#create directories and clone doodba copier template
mkdir projects
cd projects
mkdir bs-odoo-test
cd bs-odoo-test

#clone the project
git clone https://github.com/Tecnativa/doodba-copier-template.git .
cd ..
cd ..

#running copier
copier copy gh:Tecnativa/doodba-copier-template projects/bs-odoo-test --trust

#answering the jinja file questions
1. odoo version 17.0 
2. Traefik v2.4
3. en-US
4. password: admin
5. DONT list database publicly
6. (empty) we dont have image registry 
7. author -> Doodba-testers
8. project name -> doodba-test-odoo
9. boost software license
10. (empty) we dont have GitLab right now!
11. (empty) we dont have the yaml file needed
12. (default) we dont have paths to prevent web crawlers
13. (empty) whitelisting cdrs
14. PostgreSQL 15
15. postgres username -> odoo
16. postgres database -> prod
17. postgres password -> odoo
18. (NO) not exposing database
19. (^prod) database filter
20. outgoing mail -> doodba-test@example.com
21. SMTP host -> mail.example.com
22. (empty) no amazon s3 duplicities

#creating a docker group
sudo groupadd docker

#adding user to it
sudo usermod -aG docker <username>

#giving the user permissions:
sudo chown root:docker /var/run/docker.sock
sudo chown -R root:docker /var/run/docker

#testing if docker works:
docker run hello-world

#open up workspace file in vscode
#wait for python interpreters to be discovered
invoke start


#odoo giving this error:
#Traceback (most recent call last):
#  File "/opt/odoo/common/entrypoint", line 75, in <module>
#    os.execvp(extra_command[0], extra_command)
#  File "/usr/local/lib/python3.10/os.py", line 575, in execvp
#    _execvpe(file, args)
#  File "/usr/local/lib/python3.10/os.py", line 617, in _execvpe
#    raise last_exc
#  File "/usr/local/lib/python3.10/os.py", line 608, in _execvpe
#    exec_func(fullname, *argrest)

# the possible solve might be:
invoke git-aggregate

#after that you will get this error from database
#you have to initialize the database
#2024-10-17 12:40:33.844 GMT [37] ERROR:  relation "ir_module_module" does not exist at character 53
#2024-10-17 12:40:33.844 GMT [37] STATEMENT:  
#                    SELECT latest_version
#                    FROM ir_module_module
#                     WHERE name='base'

# to initiate database run:
docker compose run --rm odoo odoo -i base --stop-after-init

#now you can go to port 17069 and login using admin as username and password.


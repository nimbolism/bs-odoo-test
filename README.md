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



### Installing Python and Invoke on Ubuntu 24.04

## 1. Install Python and Pip
First, update the package list and install Python 3 and pip:

```
bash
sudo apt update
sudo apt install python3
sudo apt install python3-pip
```


### 2. Initialize a Virtual Environment
# 2.1 Install venv
Install the venv module:
```
sudo apt install python3.12-venv
```

## 2.2 Create and Activate the Virtual Environment
Create a virtual environment named .odoo and activate it:
```
python3 -m venv .odoo
source .odoo/bin/activate
```

## 2.3 Install Pipx and Other Tools
With the virtual environment activated, install ``` pipx ```:
```
python3 -m pip install pipx
```

## 2.4 Install Tools Using Pipx
Install the necessary tools:
```
pipx install copier
pipx install invoke
pipx install pre-commit
pipx ensurepath
```

### 3. Install Docker Engine
## 3.1 Add Docker's Official GPG Key
Run the following commands to prepare for Docker installation:
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

## 3.2 Add the Docker Repository
Add the Docker repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

## 3.3 Install Docker Engine
Now, install the Docker packages:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3.4 Verify Docker Installation
To check if Docker is installed correctly, run:
```
sudo docker run hello-world
```

### 4. Check Git Installation
Verify that Git is installed:
```
git -v
```

### 5. Create Directories and Clone Doodba Copier Template
Create a project directory and clone the repository:
```
mkdir projects
cd projects
mkdir bs-odoo-test
cd bs-odoo-test

git clone https://github.com/Tecnativa/doodba-copier-template.git .
cd ..
cd ..
```

### 6. Run Copier
Execute the copier command and answer the Jinja file questions:

### Questions to Answer:
1. Odoo version: 17.0
2. Traefik: v2.4
3. Language: en-US
4. Password: admin
5. Database listing: Don't list publicly
6. Image registry: (empty)
7. Author: Doodba-testers
8. Project name: doodba-test-odoo
9. License: Boost Software License
10. GitLab: (empty)
11. YAML file: (empty)
12. Web crawlers: (default)
13. Whitelisting: (empty)
14. PostgreSQL: 15
15. PostgreSQL username: odoo
16. PostgreSQL database: prod
17. PostgreSQL password: odoo
18. Exposing database: (NO)
19. Database filter: (^prod)
20. Outgoing mail: doodba-test@example.com
21. SMTP host: mail.example.com
22. S3 duplicities: (empty)

### 7. Create Docker Group
Create a Docker group and add your user:
```
sudo groupadd docker
sudo usermod -aG docker <username>
```

## Set Permissions
Set the appropriate permissions:
```
sudo chown root:docker /var/run/docker.sock
sudo chown -R root:docker /var/run/docker
```

## Test Docker Again
Verify Docker functionality:
```
docker run hello-world
```

### 8. Open Workspace in VSCode
Open the workspace file in VSCode and wait for the Python interpreters to be discovered:
```
invoke start
```

### 9. Common Errors and Solutions
## Odoo Error
If you encounter the following error:
```
Traceback (most recent call last):
...
```
Run:
```
invoke git-aggregate
```

## Database Initialization Error
If you see this error regarding the database:
```
relation "ir_module_module" does not exist at character 53
```

Initialize the database:
```
docker compose run --rm odoo odoo -i base --stop-after-init
```

## Accessing Odoo
You can now access Odoo at http://localhost:17069 using admin as both the username and password.

### accessing via WSL
# check for distro options:
```
wsl --list --online
```

# download ubuntu24.04
```
wsl --install -d ubuntu-24.04
```

now you should open vscode and download extensions needed for wsl,
then you will connect to ubuntu distro you downloaded and after opening
a terminal, you can start to run commands above

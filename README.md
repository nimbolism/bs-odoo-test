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



Installing Python and Invoke on Ubuntu 24.04
This guide will help you set up Python, Docker, and the necessary tools for your project on an Ubuntu 24.04 system.

Step 1: Install Python and pip
First, update your package list and install Python 3 along with pip:

bash
Copy code
sudo apt update
sudo apt install python3 python3-pip
Step 2: Initialize a Virtual Environment
1. Install venv
To create a virtual environment, install the venv module:

bash
Copy code
sudo apt install python3.12-venv
2. Create and Activate the Virtual Environment
Create a virtual environment named .odoo and activate it:

bash
Copy code
python3 -m venv .odoo
source .odoo/bin/activate
3. Install pipx
With the virtual environment activated, install pipx:

bash
Copy code
python3 -m pip install pipx
4. Install Tools Using pipx
Install the necessary tools:

bash
Copy code
pipx install copier
pipx install invoke
pipx install pre-commit
pipx ensurepath
Step 3: Install Docker Engine
1. Add Docker's Official GPG Key
bash
Copy code
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
2. Add the Docker Repository
Add the Docker repository to your APT sources:

bash
Copy code
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
3. Install Docker
Update your package list and install Docker:

bash
Copy code
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
4. Verify Docker Installation
Check if Docker is installed correctly:

bash
Copy code
sudo docker run hello-world
Step 4: Check Git Installation
Ensure Git is installed (it should be by default):

bash
Copy code
git -v
Step 5: Set Up Project Directories
Create the project directories and clone the Doodba copier template:

bash
Copy code
mkdir -p ~/projects/bs-odoo-test
cd ~/projects/bs-odoo-test
git clone https://github.com/Tecnativa/doodba-copier-template.git .
Step 6: Run Copier
Run the copier to set up your project:

bash
Copy code
copier copy gh:Tecnativa/doodba-copier-template projects/bs-odoo-test --trust
Answer the Prompts
When prompted, answer as follows:

Odoo version: 17.0
Traefik: v2.4
Language: en-US
Password: admin
Do not list database publicly: Yes
Image registry: (empty)
Author: Doodba-testers
Project name: doodba-test-odoo
License: Boost Software License
GitLab: (empty)
YAML file: (empty)
Web crawlers paths: (default)
Whitelist CDRs: (empty)
PostgreSQL version: 15
PostgreSQL username: odoo
PostgreSQL database: prod
PostgreSQL password: odoo
Expose database: NO
Database filter: ^prod
Outgoing mail: doodba-test@example.com
SMTP host: mail.example.com
Amazon S3 duplicities: (empty)
Step 7: Create Docker Group
Create a Docker group and add your user:

bash
Copy code
sudo groupadd docker
sudo usermod -aG docker <your-username>
Update Permissions
Set the appropriate permissions:

bash
Copy code
sudo chown root:docker /var/run/docker.sock
sudo chown -R root:docker /var/run/docker
Test Docker Again
Verify Docker is working:

bash
Copy code
docker run hello-world
Step 8: Open Workspace in VS Code
Open your project in Visual Studio Code:

bash
Copy code
invoke start
Troubleshooting Odoo Errors
If you encounter errors like:

sql
Copy code
Traceback (most recent call last):
  ...
You may need to run:

bash
Copy code
invoke git-aggregate
If you get database initialization errors, run:

bash
Copy code
docker compose run --rm odoo odoo -i base --stop-after-init
Finally, access your Odoo instance at http://localhost:17069 and log in with admin as the username and password.



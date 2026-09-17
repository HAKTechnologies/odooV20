# HAK TECH — Odoo 20 Installation on Ubuntu 24.04 LTS

Complete command reference for installing **Odoo 20 Community Edition** on an Ubuntu 24.04 LTS server.

**1. Login to Ubuntu Server**

```bash
ssh username@server_ip
ssh -p port_number username@server_ip
ssh -i /path/to/your/key.pem username@server_ip

lsb_release -a

**2. Update Server**
sudo apt-get update
sudo apt-get upgrade -y

**3. Secure Server**
sudo apt-get install -y openssh-server
sudo apt-get install -y fail2ban

sudo systemctl start fail2ban
sudo systemctl enable fail2ban
sudo systemctl status fail2ban


**4. Install Required Packages**
sudo apt-get install -y python3-pip python3-venv python3-dev \
git build-essential libxml2-dev libxslt1-dev zlib1g-dev \
libsasl2-dev libldap2-dev libssl-dev libffi-dev \
libjpeg-dev libpq-dev liblcms2-dev libblas-dev \
libatlas-base-dev npm node-less

sudo apt-get install -y nodejs

nodejs --version

sudo ln -s /usr/bin/nodejs /usr/bin/node

sudo npm install -g less less-plugin-clean-css


**5. Install PostgreSQL**
sudo apt-get install -y postgresql postgresql-client

sudo su - postgres

createuser --createdb --username postgres --no-createrole --superuser --pwprompt odoo20

exit


**6. Create Odoo System User**
sudo adduser --system --home=/opt/odoo20 --group odoo20


**7. Download Odoo 20 from GitHub**
sudo apt-get install -y git

sudo su - odoo20 -s /bin/bash

cd /opt/odoo20

git clone https://github.com/odoo/odoo.git --depth 1 --branch 20.0 --single-branch .

ls

exit


**8. Create Python Virtual Environment**
sudo apt install -y python3-venv

sudo python3 -m venv /opt/odoo20/venv

sudo -s

cd /opt/odoo20

source venv/bin/activate

pip install --upgrade pip

pip install -r requirements.txt

deactivate

exit

**9. Install wkhtmltopdf**
sudo wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.6.1/wkhtmltox_0.12.6.1-2.jammy_amd64.deb

sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb

sudo apt install -f

wkhtmltopdf --version


**10. Configure Odoo**
sudo cp /opt/odoo20/debian/odoo.conf /etc/odoo20.conf

sudo nano /etc/odoo20.conf

Add:

[options]
admin_passwd = your_admin_password
db_host = localhost
db_port = 5432
db_user = odoo20
db_password = your_database_password
addons_path = /opt/odoo20/addons
logfile = /var/log/odoo/odoo20.log



**11. Set Permissions**
sudo chown odoo20: /etc/odoo20.conf

sudo chmod 640 /etc/odoo20.conf

sudo mkdir -p /var/log/odoo

sudo chown odoo20:root /var/log/odoo


**12. Create Odoo Systemd Service**
sudo nano /etc/systemd/system/odoo20.service

Add:

[Unit]
Description=Odoo 20
Documentation=https://www.odoo.com

[Service]
Type=simple
User=odoo20
Group=odoo20
ExecStart=/opt/odoo20/venv/bin/python3 /opt/odoo20/odoo-bin -c /etc/odoo20.conf

[Install]
WantedBy=multi-user.target


**13. Start Odoo Service**
sudo systemctl daemon-reload

sudo systemctl start odoo20

sudo systemctl enable odoo20

sudo systemctl status odoo20


**14. Access Odoo**

Open your browser:

http://your_server_ip:8069


**15. Check Odoo Logs**
sudo tail -f /var/log/odoo/odoo20.log

Or:

sudo journalctl -u odoo20 -f


HAK TECH

Odoo ERP Development • Customization • Migration • Integration

HAK Technologies (SMC-Private) Limited
HAK TECH

haktechnologiesoffice@gmail.com

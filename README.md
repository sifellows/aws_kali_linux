# AWS Kali Linux setup commands

## Upgrade OS
```bash
sudo apt-get update -y && apt-get upgrade  -y
sudo apt-get dist-upgrade -y
```
## Get required packages
```bash
sudo apt-get install xrdp lxde-core lxde xfce4 tigervnc-standalone-server -y
```
## Set the session manager
```bash
sudo update-alternatives --config x-session-manager
* choose xfce4-session
```

## New sudo user for password-based connection (ec2-user uses an SSH key)
```bash
sudo useradd -m kali
sudo passwd kali
sudo usermod -a -G sudo kali
sudo chsh -s /bin/bash kali
```

## edit xrdp.ini - reducing colour depth reduces bandwidth and can resolve black screen issues
```bash
sudo vi /etc/xrdp/xrdp.ini
```
```
autorun=sesman-any
max_bpp=16
[sesman-any]
ip=127.0.0.1
* changed username and password to the kali user
```


## Allow more than root to access the system
```bash
sudo vi /etc/X11/Xwrapper.config
```
```
allowed_users=anybody
```

## start services
```bash
sudo service xrdp start
sudo service xrdp-sesman start
```
## configure services to auto-start on boot
```bash
sudo update-rc.d xrdp enable
sudo systemctl enable xrdp-sesman.service
```

# Tests:
## Test: services should be green and active
```bash
sudo service xrdp status
sudo service xrdp-sesman status
```

## Test: use an rdp client
Connecting to the Kali IP with an RDP client and the new user's password should present you with the Kali desktop.

# Transfer files to Kali
```
scp -i kali_linux.pem <file> <ip-addr/hostname>:<path>
```

# Additional software
## Nessus
Go to https://www.tenable.com/downloads/nessus
Download (64-bit) version for Kali (e.g. 8.11.0)
```
dpkg -i Nessus-8.11.0-debian6_amd64.deb
```

### If Nessus keeps looping through initialization stage...
```
service nessusd stop 
/opt/nessus/sbin/nessuscli fix --reset 
/opt/nessus/sbin/nessuscli fetch --register ACTIVATION-CODE-HERE  
/opt/nessus/sbin/nessusd -R 
service nessusd start
```

## InsightVM (was Nexpose)
Download free trial version (use burner email address)
Copy to Kali VM and install
Use code in burner email address

## PDF readers
```
sudo apt install -y xpdf okular
```

## Git
```
sudo apt install -y git
```

## Guacamole (needed???)
```
git clone https://github.com/MysticRyuujin/guac-install.git /tmp/guac-install
cd /tmp/guac-install/
sudo ./guac-install.sh --nomfa --installmysql --mysqlpwd S3cur3Pa$$w0rd --guacpwd P@s$W0rD
systemctl status tomcat9 guacd mysql
```
## Install TigerVNC server (needed???)
```
sudo apt install -y tigervnc-standalone-server
mkdir ~/.vnc/ && wget https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-project/-/raw/master/nethunter-fs/profiles/xstartup -O ~/.vnc/xstartup
vncserver :1
```


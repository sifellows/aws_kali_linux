# AWS Kali Linux setup commands

## Upgrade OS
```bash
sudo apt-get update -y && apt-get upgrade  -y
sudo apt-get dist-upgrade -y
```
## Get required packages
```bash
sudo apt-get install xrdp lxde-core lxde tigervnc-standalone-server -y
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

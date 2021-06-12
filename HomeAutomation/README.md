# HomeAutomation
home automation of my room


#1. set up rasbian
install rasbian 10 on usb storage
Raspberry Pi Imager : https://www.raspberrypi.org/%20downloads/
install - raspberry pi os (32 bit)

set up headless install 
   
create 2 files
 -- wpa_supplicant.conf
```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=IN(contry code)

network={
ssid="SSID"
psk="PASSWORD"
}
```

-- ssh
```bash
```

boot the raspberry pi 4 using the usb storage 
(note may require firmware update on older versions)
run commands on terminal
   

```bash
#to find the ip
sudo nmap -sP 192.168.1.0/24

# to ssh into the pi 
# important - use your ip in place of 192.168.1.15
# password - raspberry
ssh pi@192.168.1.15
```
   
run basic commands on pi

```bash
# update raspberry pi
sudo apt-get update && sudo apt-get upgrade -y
```

set up VNC player
```bash
#Restart VNC Server each time the system is booted using systemd:
sudo systemctl enable vncserver-x11-serviced.service

#Start VNC Server on a Linux system using systemd:
sudo systemctl start vncserver-x11-serviced.service
```

open vnc player 
host name - your ip
user name - pi
password - raspberry

configure display setting 
```bash
# raspberry pi configuration tool
sudo raspi-config
```

set the display resolution and connect back

---

# 2. set up docker
install docker
```bash
#get the required docker installer files
curl -fsSL https://get.docker.com -o get-docker.sh

# install the required packages for Raspbian Linux distribution
sudo sh get-docker.sh

# check docker version
docker version

# deploy a hello world docker
docker run hello-world
```

install portainer 
```bash
# get the required portainer files
sudo docker pull portainer/portainer-ce:linux-arm

# run docker with parameters
# port - 9000
# docker name - portainer
sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:linux-arm
```
open a browser with a url - your ip:9000 (example - 192.168.1.15:9000)
create a new user with password and log into the portainer

resetting admin password
```bash
#to list all the running dockers
docker ps

# stop the docker 
docker stop "id-portainer-container"

#get the admin password
docker run --rm -v portainer_data:/data portainer/helper-reset-password

# restart docker and login
docker start "id-portainer-container"

```

# 3. Install Home Assistant on Linux/Docker 
install required dependencies
```bash
#docker hub image - 
homeassistant/raspberrypi4-homeassistant
```

```bash
#install esphome
sudo docker run -d --name="esphome" --net=host -p 6052:6052 -p 6123:6123 -e TZ=Asia/Kolkata -v /homeassistant/esphome:/config esphome/esphome
```

esp home switch code

```bash
# D0 is the onboard led of node mcu
switch:
  - platform: gpio
    pin: D0
    name: "test led"
```

# 4. run applications at startup raspberry pi
run applications at boot 
```bash
# add the commands into the file save it 
sudo nano /etc/rc.local
```

kde connect application autostart

```bash
kdeconnect-indicator
```

# 5. enable netflix, amazon prime 
download resources
```bash
sudo apt install libwidevinecdm0
```

# 6. install pihole

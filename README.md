# tequ-rpi-setup
Raspberry PI 3/4 preparation for basic DAQ usage. 

| Software      | Version       | 
| ------------- |:-------------:| 
| node-red	    | 	            |
| node.js       |               |
| tfjs-node-gpu |               | 
| python        |               | 



## Actions

### 1. Prepare Raspberry Pi OS SD-card with Raspberry Pi Imager

https://www.raspberrypi.org/software/

32 GB SD-card is preferable

Pre-configure your installation by pressing CTRL+SHIFT+X after you select OS in installer.

- SSH
- Hostname
- WiFi
- Locale settings

### 2. Boot your RPi with SD-card

### 3. Update your system

### 4. Configure & enable interfaces

```sudo raspi-config```

- Enable Serial port (without console)
- Enable SSH
- Enable 1-wire
- Enable I2C
- Enable Camera


### 5. Install Node-RED

```bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)```

### 6. Configure Node-RED to autostart on boot

```sudo systemctl enable nodered.service```

### 7. Install Node-RED libraries that are often needed
```
cd ~./node-red
npm install node-red-auth-github &&
npm install node-red-dashboard && 
npm install node-red-node-base64 &&
npm install node-red-node-cf-cloudant && 
npm install node-red-node-daemon &&
npm install node-red-node-email && 
npm install node-red-node-exif && 
npm install node-red-node-rbe &&
npm install node-red-node-smooth &&
npm install node-red-node-suncalc && 
npm install node-red-contrib-aggregator && 
npm install node-red-contrib-binary && 
npm install node-red-contrib-buffer-parser && 
npm install node-red-contrib-calc &&
npm install node-red-contrib-camerapi && 
npm install node-red-contrib-cos && 
npm install node-red-contrib-counter &&
npm install node-red-contrib-fs && 
npm install node-red-contrib-fs-ops && 
npm install node-red-contrib-ibm-watson-iot &&
npm install node-red-contrib-image-info &&
npm install node-red-contrib-image-output &&
npm install node-red-contrib-modbus && 
npm install node-red-contrib-moment && 
npm install node-red-contrib-scx-ibmiotapp &&
npm install node-red-contrib-sunevents  
```

### 8. Install or update Python & Picamera

```sudo apt-get install python-picamera python3-picamera```

### 9. Install &configure hardware watchdog

```
sudo su
echo 'dtparam=watchdog=on' >> /boot/config.txt
reboot
```

```
sudo apt-get update
sudo apt-get install watchdog
```

```
sudo su
echo 'watchdog-device = /dev/watchdog' >> /etc/watchdog.conf
echo 'watchdog-timeout = 15' >> /etc/watchdog.conf
echo 'max-load-1 = 24' >> /etc/watchdog.conf
```

```
sudo systemctl enable watchdog
sudo systemctl start watchdog
sudo systemctl status watchdog
```



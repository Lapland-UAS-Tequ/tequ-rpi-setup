# tequ-rpi-setup
Raspberry PI 3/4 preparation for basic DAQ usage. 

| Software      |               | 
| ------------- |:-------------:| 
| node-red	    | 	            |
| node.js       |               |
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

Shutdown your Raspberry. Insert SD-card. Plug in keyboard, mouse, display and power cable.

### 3. Update & configure your system

Follow wizard appearing to screen after first boot.

### 4. Configure & enable interfaces

```
sudo raspi-config
```

- Enable Serial port (without console)
- Enable SSH
- Enable 1-wire
- Enable I2C
- Enable Camera


### 5. Install Node-RED

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

### 6. Configure Node-RED to autostart on boot

```
sudo systemctl enable nodered.service
```

```
reboot
```

Open browser and go to http://localhost:1880 to see if Node-RED is working.

### 7. Install Node-RED libraries that are often needed
```
cd ~/.node-red
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
npm install node-red-contrib-multipart-stream-decoder &&
npm install node-red-contrib-multipart-stream-encoder &&
npm install node-red-contrib-scx-ibmiotapp &&
npm install node-red-contrib-sunevents  
```

```
node-red-restart
```

### 8. Secure Node-RED editor 

https://nodered.org/docs/user-guide/runtime/securing-node-red

BCrypted password creation:
```
cd ~/.node-red 
```

```
node-red-admin hash-pw
```

Edit /.node-red/settings.js and replace password in "adminAuth"

```
adminAuth: {
    type: "credentials",
    users: [
        {
            username: "admin",
            password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
            permissions: "*"
        }
    ]
}
```

### 9. Install Python & Picamera

```
sudo apt-get install python-picamera python3-picamera
```

### 10. Install & configure hardware watchdog

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



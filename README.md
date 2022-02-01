This repository is developed in Fish-IoT project

https://www.tequ.fi/en/project-bank/fish-iot/ 

---
# tequ-rpi-setup
Raspberry PI 3/4 preparation for DAQ & AI usage. 

| Software      | Version       | 
| ------------- |:-------------:| 
| node.js       | 16.13.2       |
| node-red	    | 2.2.0	        |
| tfjs-node-gpu | 3.13.0	    | 

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
npm install node-red-contrib-moment && 
npm install node-red-contrib-multipart-stream-decoder &&
npm install node-red-contrib-multipart-stream-encoder &&
npm install node-red-contrib-scx-ibmiotapp &&
npm install node-red-contrib-browser-utils &&
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
node-red admin hash-pw
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

### 11. Update node.js to version 16

```
sudo apt update
```

```
sudo apt-get update
```

```
curl -sSL https://deb.nodesource.com/setup_16.x | sudo bash -
```

```
sudo apt-get install -y nodejs
```

## (Optional) Transfer Raspberry Pi OS to SSD 

Tested with:
- Raspberry PI 4
- Argon ONE M.2 Aluminum Case for Raspberry Pi 4 - With SSD support via M.2
- Samsung MZ-N6E250BW - SSD 860 EVO M.2 250GB SATA 6 Gb/s

If you want to use the RPI without the SD card, you can transfer to RPI OS to an external USB, SSD or HDD. This will make it run a lot faster, when using it for anything data related.

### Open the Raspberry Pi config menu
```
sudo raspi-config
```
- Select Advanced options
- In Advanced options, select Bootloader version and select Latest, click yes if asked.
- Now close out of the config menu and reboot.
- Once rebooted, reopen the config menu and go back to Advanced options.
- Now select Boot Order and select USB Boot, then exit and reboot.
- Now you can connect your SSD or HDD.
- Open SD Card Copier from the accessories tab, then select your source and destination and start.
- Once copying is finished you can shutdown the Pi and remove the SD card, it should now boot up without it.


## (Optional) Install Tensorflow & Canvas for Object Detection in Node-RED

```
cd ~/.node-red
```

```
npm install @tensorflow/tfjs-node-gpu
```


```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```

```
sudo npm install --global node-pre-gyp
```

```
sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
```

```
npm install canvas 
```

Go to https://github.com/Lapland-UAS-Tequ/tequ-api-client for example flows how to use tensorflow


## (Optional) Install Basler Pylon & Pypylon

https://www.baslerweb.com/fp-1597837316/media/downloads/documents/application_notes/AW00162902000_How_to_Build_pylon_Applications_on_Raspberry_Pi.pdf

```
cd $home
```

```
wget https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/pylon_6.2.0.21487_armhf_setup.tar.gz
```

```
mkdir ./pylon_setup
```

```
tar -C ./pylon_setup -xzf ./pylon_6.2.0.21487_armhf_setup.tar.gz
```

```
cd ./pylon_setup
```

```
sudo mkdir /opt/pylon
```

```
sudo tar -C /opt/pylon -xzf ././pylon_6.2.0.21487_armhf.tar.gz
```

```
sudo chmod 755 /opt/pylon
```

```
sudo /opt/pylon/share/pylon/setup-usb.sh
```

```
pip3 install pypylon
```


# (Optional) Install Gstreamer plugin for Basler cameras 

```
sudo apt-get install git cmake libgstreamer-plugins-base1.0-dev liborc-0.4-dev gstreamer1.0-tools libx264-dev libjpeg-dev
```

```
sudo apt-get install libgstreamer1.0-dev \
     libgstreamer-plugins-base1.0-dev \
     libgstreamer-plugins-bad1.0-dev \
     gstreamer1.0-plugins-ugly \
     gstreamer1.0-tools \
     gstreamer1.0-gl \
     gstreamer1.0-gtk3
```
```
git clone https://github.com/Lapland-UAS-Tequ/gst-plugins-vision
```

```
cd gst-plugins-vision
```

```
sudo nano cmake/modules/FindPylon.cmake
```

Replace "/opt/pylon5" => "/opt/pylon"

```
mkdir build
```

```
cd build
```

Directory might be different on your device, check where gstreamer-1.0 folder is located

```
cmake -DCMAKE_INSTALL_PREFIX=/usr/lib/arm-linux-gnueabihf ..
```

```
make
```

```
sudo make install
```

```
sudo nano /etc/ld.so.conf.d/pylon.conf
```

Add following path to the file:

```
/opt/pylon/lib
```

```
sudo ldconfig
```

Example pipelines, depending on camera settings
```
gst-launch-1.0 pylonsrc config-file=config.pfs ! queue ! videoconvert ! jpegenc ! queue ! tcpclientsink port=55555
```

More Gstremer examples:

https://github.com/Lapland-UAS-Tequ/tequ-basler-gstreamer

# (Optional) Install RTSP Simple server

```
cd $home
```

```
mkdir rtsp-simple-server
```

```
cd rtsp-simple-server
```

```
wget https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/rtsp-simple-server_v0.17.15_linux_armv7.tar.gz
```

```
tar -zxvf rtsp-simple-server_v0.17.15_linux_armv7.tar.gz
```

```
rtsp-simple-server
```


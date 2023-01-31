This repository is developed in Fish-IoT project

https://www.tequ.fi/en/project-bank/fish-iot/ 

---
# tequ-rpi-setup
Raspberry PI 3/4 preparation for DAQ & AI usage. 

| Software      | Version       | 
| ------------- |:-------------:| 
| node.js       | 18.12.1       |
| node-red	    | 3.0.2      |
| tfjs-node-gpu | 3.13.0	    |
| pylon         | 7.2.1      |

It is optimal to have an remote desktop connection to the RPI.

## Actions

### 1. Prepare Raspberry Pi OS SD-card or SSD with Raspberry Pi Imager

## NOTE!
If you want to use an SSD for faster read/write speeds, you can connect an SSD to your device and select that instead of an SD card. The process will be completely the same otherwise.

---

https://www.raspberrypi.org/software/

32 GB SD-card/SSD is preferable, or better.

Use 64 bit version.

Preconfigure your installation by pressing CTRL+SHIFT+X after you select OS in installer.

- SSH
- Hostname
- WiFi
- Locale settings

### 2. Booting and configuration

Insert the SD card in the RPI or the SSD into a USB port. 
Then connect to the RPI via SSH with your preconfigured settings, once connected follow below steps.

```
sudo raspi-config
```

- Enable Serial port
- Enable 1-wire
- Enable I2C
- Enable Camera

Remember to reboot after enabling settings.


### 3. Install Node-RED

```
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
``` 
Create users and setup passwords. If you selected no during the setup, run the command below.
```
node-red admin init
```

### 4. Configure Node-RED to autostart on boot

```
sudo systemctl enable nodered.service
sudo reboot
```

Open browser and go to http://localhost:1880 to see if Node-RED is working. If not, run:
```
node-red-start
```

### 5. Install Node-RED libraries that are often needed
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

### 6. Using a camera

To use a camera with the RPI, you'll need the libcamera library, it should come preinstalled, but if not run:

```
sudo apt install libcamera-apps
sudo reboot
```

To test that everything is working, plug in a monitor and the camera and run:
```
libcamera-hello
```
Keep in mind to use the RPI terminal, not remote desktop terminal.
If you see the camera view on your monitor, then the camera is working.

### 7. Install & configure hardware watchdog

```
sudo su
echo 'dtparam=watchdog=on' >> /boot/config.txt
reboot
```

```
sudo apt update
sudo apt install watchdog
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

### 8. Update node.js to version 18

```
sudo apt update
sudo apt upgrade
curl -sSL https://deb.nodesource.com/setup_18.x | sudo bash -
sudo apt-get install -y nodejs
```

# (Optional) Install Tensorflow & Canvas for Object Detection in Node-RED

Tensorflow tjfs-node-gpu installation
```
cd .node-red
```

```
npm install --ignore-scripts @tensorflow/tfjs-node-gpu
```

```
npm rebuild @tensorflow/tfjs-node-gpu --build-from-source
```

```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```

Canvas installation
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
./rtsp-simple-server
```

Install RTSP GStreamer plug-in
```
sudo apt install gstreamer1.0-rtsp
```

## (Optional) Install Basler Pylon & Pypylon

Install Basler ARM64 package and Pypylon Python libraries to use Basler camera in 64-bit Raspberry PI system-

Instructions: 
- https://www.baslerweb.com/fp-1666012566/media/downloads/software/pylon_software/INSTALL~9.txt
- https://www.baslerweb.com/fp-1597837316/media/downloads/documents/application_notes/AW00162902000_How_to_Build_pylon_Applications_on_Raspberry_Pi.pdf
- https://www.baslerweb.com/en/downloads/software-downloads/software-pylon-7-2-1-linux-arm-64bit-debian/ 
- https://www.baslerweb.com/en/downloads/software-downloads/pylon-7-2-1-linux-arm-64-bit/

Notes:
- Remember that the USB camera must be plugged into 3.0 USB port.


```
cd $home
```

```
mkdir basler
```

```
cd basler
```

```
wget https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/pylon_7.2.1.25747_aarch64_debs.tar.gz
```

```
tar -xvf pylon_7.2.1.25747_aarch64_debs.tar.gz
```

```
cd ./pylon_setup
```

```
sudo apt-get install ./pylon_*.deb 
```

```
sudo /opt/pylon/share/pylon/setup-usb.sh
```

```
pip3 install pypylon
```

To find pylonviewer, navigate to: /opt/pylon/bin/pylonviewer
Open pylonviewer with "pylon Viewer" application which is under Sound & Video category.


# (Optional) Install Gstreamer plugin for Basler cameras 

https://github.com/Lapland-UAS-Tequ/tequ-basler-gstreamer

https://github.com/basler/gst-plugin-pylon

```
sudo apt remove meson ninja-build
```

```
sudo -H python3 -m pip install meson ninja --upgrade
```

```
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev cmake
```

```
sudo apt-get install gstreamer1.0-tools
```

```
export PYLON_ROOT=/opt/pylon
```

```
git clone https://github.com/basler/gst-plugin-pylon.git
```

```
cd gst-plugin-pylon
```

```
meson setup builddir --prefix /usr/
```

```
ninja -C builddir
```

```
ninja -C builddir test
```

```
sudo ninja -C builddir install
```

```
gst-inspect-1.0 pylonsrc
```

Example pipelines, depending on camera settings

```
gst-launch-1.0 pylonsrc ! queue ! videoconvert ! jpegenc ! queue ! tcpclientsink port=55555
```

1. Show camera video on display
```
```

2. Publish H264 encoded camera video to RTSP server
```
```

3. Publish camera video stream as JPEG stream to TCP server
```
```

4. Combine 1&2&3 in single pipeline
```
```

# (Optional) Use GStreamer with native Raspberry PI HQ camera

1. Show camera video on display
```
```

2. Publish H264 encoded camera video to RTSP server
```
```

3. Publish camera video stream as JPEG stream to TCP server
```
```

4. Combine 1&2&3 in single pipeline
```
```


https://platypus-boats.readthedocs.io/en/latest/source/rpi/video/video-streaming-gstreamer.html


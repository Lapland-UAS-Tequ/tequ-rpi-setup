# tequ-rpi-setup
Raspberry PI 3/4 preparation for basic DAQ usage. 

| Software      | Version       | 
| ------------- |:-------------:| 
| node-red	    | 	            |
| node.js       |               |
| tfjs-node-gpu |               | 
| python        |               | 



## Actions

1. Prepare Raspberry Pi OS SD-card with Raspberry Pi Imager

https://www.raspberrypi.org/software/

32 GB SD-card is preferable

Pre-configure your installation by pressing CTRL+SHIFT+X after you select OS in installer.

- SSH
- Hostname
- WiFi
- Locale settings

2. Boot your RPi with SD-card

3. Update your system

4. Configure & enable interfaces

```sudo raspi-config```

- Enable Serial port (without console)
- Enable SSH
- Enable 1-wire
- Enable I2C
- Enable Camera


5. Install Node-RED

```bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)```

6. Install or update Python & Picamera

```sudo apt-get install python-picamera python3-picamera```

7. Install &configure hardware watchdog

```sudo su
echo 'dtparam=watchdog=on' >> /boot/config.txt
reboot```

```sudo apt-get update
sudo apt-get install watchdog```

```sudo su
echo 'watchdog-device = /dev/watchdog' >> /etc/watchdog.conf
echo 'watchdog-timeout = 15' >> /etc/watchdog.conf
echo 'max-load-1 = 24' >> /etc/watchdog.conf```

```sudo systemctl enable watchdog
sudo systemctl start watchdog
sudo systemctl status watchdog```



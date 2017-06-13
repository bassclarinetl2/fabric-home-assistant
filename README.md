# fabric-home-assistant

NOTE: This is a personal fork of the HASS Fabric Installer.  It has been modified to reflect my banana pro.  In other words, this is not the fork you're looking for.

 ![image](images/hass_plu_fabric_logo.png)

This varant of the [Raspberry Pi All-In-One Installer (originally created by onathan Baginski)](https://github.com/home-assistant/fabric-home-assistant) deploys a complete Home Assistant server including support for MQTT with websockets, Z-Wave, and the Open-Zwave Control Panel.  As well as fixing permissions after copying in a backed up config and support for editing via sftp.

The only requirement is that you have a Banana Pi/Pro with a fresh installation of [Armbian Server (Mainline kernel)](https://www.armbian.com/banana-pi-pro/) connected to your network.

*  Login to the Banana Pi/Pro. For example with `ssh pi@your_banana_ip`
*  Run the following command

```bash
$ wget -Nnv https://raw.githubusercontent.com/bassclarinetl2/fabric-home-assistant/master/hass_rpi_installer.sh && bash hass_rpi_installer.sh
```
*Note this command is one line and not run as sudo*

Installation will take approx. 1-2 hours depending on the Raspberry Pi model the installer is being run against.

[Will Heid](http://willheid.com) has created a guide on how to get Armbian and the AIO working.

Once rebooted, your Banana will be up and running with Home Assistant. You can access it at http://your_banana_ip:8123.

The Home Assistant configuration is located at `/home/homeassistant/.homeassistant`. The virtualenv with the Home Assistant installation is located at `/srv/homeassistant/homeassistant_venv`. As part of the secure installation, a new user is added to your Raspberry Pi to run Home Assistant as named, **homeassistant**. This is a system account and does not have login or other abilities by design. When editing your configuration.yaml files, you will need to run the commands with "sudo" or by switching user.
*Windows users* - Setting up WinSCP to allow this seemlessly is detailed below.

By default, installation makes use of a Python Virtualenv. If you wish to not follow this recommendation, you may add the flag `-n` to the end of the install command specified above.

The All-In-One Installer script will do the following automatically:

*  Create all needed directories
*  Create needed service accounts
*  Install OS and Python dependencies
*  Setup a python virtualenv to run Home Assistant and components inside.
*  Run as `homeassistant` service account
*  Install Home Assistant in a virtualenv
*  Build and install Mosquitto from source with websocket support running on ports 8883 and 9100
*  Build and Install Python-openzwave in the Home Assistant virtualenv
*  Build openzwave-control-panel in `/srv/homeassistant/src/open-zwave-control-panel`
*  Add both Home Assistant and Mosquitto to systemd services to start at boot
*  Add the system user **sftpedit** to permit editing of device.


To upgrade the All-In-One setup:

*  Login to Raspberry Pi ```ssh pi@banana_ip```
*  Change to hass user `sudo su -s /bin/bash homeassistant`
*  Change to virtual enviroment `source /srv/homeassistant/homeassistant_venv/bin/activate`
*  Update HA `pip3 install --upgrade homeassistant`

To launch the OZWCP webapp:

*  Login to Raspberry Pi `ssh pi@banana_ip`
*  Change to the ozwcp directory `cd /srv/homeassistant/src/open-zwave-control-panel/`
*  Launch the control panel `sudo ./ozwcp -p 8888`
*  Open a web browser to `http://banana_ip:8888`
*  Specify your zwave controller, for example `/dev/ttyACM0` and hit initialize
  
*don't check the USB box regardless of using a USB based device*


*Windows Users* - Please note that after running the installer, you will need to modify settings allowing you to "switch users" to edit your configuration files. The needed change within WinSCP is: Environment -> SCP/Shell -> Shell and set it to `sudo su -`.

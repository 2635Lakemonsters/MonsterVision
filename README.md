# MonsterVision Documentation

This should run on RPI5. No testing done on RPI4 yet

## How to setup/use MonsterVision:

Illustration for vision hardware setup on robot:
<img width="1000" height="600" alt="VisionHardware" src="visionHardware.png"/>

### How to set up Raspberry Pi 5 with WPILibPi and MonsterVision (FOR SETUP ON ROBOT)
1. Starting on your local machine (NOT THE PI), run `git clone https://github.com/2635Lakemonsters/MonsterVision.git`
1. Download most recent WPILibPi image from [here](https://github.com/wpilibsuite/WPILibPi/releases) (scroll down to "Assets" and select WPILibPi, not Romi)
1. Download and install Raspberry Pi Imager from [here](https://www.raspberrypi.com/software/)
1. Insert a micro SD card
9000. Select the device you have, "Use Custom" under Operating System, and the micro SD card in Raspberry Pi Imager
1. Insert SD card into Pi and plug into an Aux port in your radio on the robot (make sure you turn the robot on and give the pi power)
1. May need to wait 2-5 minutes for pi to boot for the first time
1. ssh into the Raspberry Pi with `ssh pi@wpilibpi.local` (if you get a "man-in-the-middle error, run `ssh-keygen -R wpilibpi.local -f <your known_hosts file path>`)
2. Navigate to [wpilibpi.local](http://wpilibpi.local) and click "Writable" at the top of the page
1. Navigate to the "Application" tab on wpilibpi.local and click "choose file" then select your MonsterVision.tar.gz file and click "Upload" (Do not check extract)
1. In the ssh run these commands (if MV5 is already on there then remove it before proceeding):
```shell
tar -xzf MonsterVision.tar.gz
rm MonsterVision.tar.gz
cd MonsterVision
dos2unix *
sudo sh resize.sh
sudo sh setup.sh <TEAM NUMBER>
```

### How to set up MonsterVision on the robot
1. Follow all the above steps for setting up the vision hardware and setting up the Pi on the robot
1. Open [wpilibpi.local](http://wpilibpi.local) and open the "Vision" tab, this will open the camera stream on the robot

## Troubleshooting/Quick Fixes


### How to toggle the camera server
1. Open command prompt
2. `ssh pi@wpilibpi` or `ssh pi@wpilibpi.local`
3. Go to [wpilibpi.local webserver](http://wpilibpi.local/) and change it to writable
4. `sudo nano /boot/mv.json`
5. Change the `'showPreview'` key to `false` or `true` depending on whether or not you need it
6. Restart MonsterVision

How to Restart MonsterVision:
1. Go to [wpilibpi.local webserver](http://wpilibpi.local/) and go to Vision Status
2. Click the red Kill button

IF KILL/TERMINATE doesn't seem to work then you have to go into htop and SIGKILL the main MonsterVision4.5.py:
1. Open command prompt
2. `ssh pi@wpilibpi` or `ssh pi@wpilibpi.local`
3. Go to [the wpilibpi.local webserver](http://wpilibpi.local/) and change it to writable
4. `htop` in terminal to view all processes
5. Click on the `python3 ./MonsterVision4.5.py` (should be in green)
6. Type `fn+f9` and then '9' to execute the SIGKILL command to kill the process
7. Hit enter to execute the command

### How to Disable Network Adapters for Competition
Laptop network wifi needs to be disabled for competition. Also secondary ethernet besides the one on the adapter for hardwiring needs to be disabled
1. Type `windows+r` to open up Windows command Runner
2. Run `ncpa.cpl`
3. Disable Wifi and any secondary ethernet connector (likely the highest number adapter)

### How to change model used
1. Open command prompt
2. `ssh pi@wpilibpi` or `ssh pi@wpilibpi.local`
3. Go to [wpilibpi.local webserver](http://wpilibpi.local/) and change it to writable
4. Type `cd ./<path to MonsterVision>/models`
6. Type `sudo cp ./<desired model .json file> /boot/nn.json`
7. Type `mv ./<latest best.blob> ./<appropriate name for .blob given season>`
8. Type `sudo nano /boot/nn.json`
9. Add between lines 6 and 7 (6.5) `"blob": "<chosen appropriate name given season>", `

6 7?

## How to do MonsterVision Development 

### How to use Pi's for MV Development
This document covers installing MonsterVision on a Raspberry Pi development machine.

It is recommended (but not required) that you use an SSD rather than an SD card on your Pi.  If you do, you may need to enable your Pi to boot from the SSD.  This only needs to be done once.  [Follow these instructions.](https://peyanski.com/how-to-boot-raspberry-pi-4-from-ssd/#:~:text=To%20boot%20Raspberry%20Pi%204%20from%20SSD%20you,USB%20to%20boot%20raspberry%20Pi%204%20from%20SSD.)
At this time it is important to note that SSDs are not competition legal for FRC.

Once you've gotten your Pi up and running, start with installing Visual Studio Code. Visual Studio Code is the preferred development environment for consistency with our general FRC code development. It is officially distributed via the Raspberry Pi OS APT repository in both 32- and 64-bit versions. Install via:
```shell
sudo apt update
sudo apt install code
```
It can be launched from a terminal via `code` or via the GUI under the **Programming** menu.

Start a Terminal session. Within the session, clone the MonsterVision repo:
```shell
git clone https://github.com/2635Lakemonsters/MonsterVision.git
```

For development, it is best to use a Python virtual environment to keep from descending into "version hell."  Create the virtual environment and activate it. This also prevents the package managers from clashing and can make the process of installing smoother. Change to the MonsterVision directory:
```shell
cd MonsterVision
```

### Edit code through Pi:
1. Go to [wpilibpi.local webserver](http://wpilibpi.local/) and change it to writable
2. Open VSCode
3. Navigate to Remote Explorer extension on left-hand menu
4. Make sure the dropdown menu at the top has `Remotes (Tunnels/SSH)` selected
5. Under `SSH`, select the desired server (probably wpilibpi) and open in current window or new window by clicking on icons next to server name
6. Enter password multiple times in the top menu bar
7. Click on the Explorer icon in far top-left corner in VSCode
8. Enter password (if needed)
9. Click Open Folder and hit enter to select the default directory (probably /home/pi)


### How to transfer initial repo from laptop to pi:
Assume local repo is in c:/dev/MonsterVision
Assume /home/pi/MonsterVision exists and is empty
Assume wpilibpi.local is the server you want to push code to
1. Open Command Prompt
2. Type `scp -rp c:/dev/MonsterVision/. pi@wpilibpi.local:/home/pi/MonsterVision/` (If this doesn't work, use Admin Command Prompt)


### How to transfer updated code to computer and GitHub from Pi:
1. Ensure all saves have been committed on remote server
2. ssh into remote server
3. Zip up contents of MonsterVision file using `zip -r <name of zip file to be created> <directory you want to zip>`
4. Get laptop IP it from `ifconfig` or `ip a` on Linux laptop or `ipconfig` on Windows laptop
5. From the pi: `scp -p <name zip file> <host user name>@<laptop ip>:<directory where you want it on laptop>`
6. Unzip on laptop
7. Copy into MonsterVision directory connected to GitHub on laptop (overwriting in the process)
8. Commit and push to GitHub

### OLD STEPS (for transfer of updated code):
1. Ensure all saves have been committed on remote server
2. ssh into remote server
3. Zip up contents of MonsterVision file using `zip -r <name of zip file to be created> <directory you want to zip>`
4. Run `ipconfig` on the laptop computer and find the correct ip address (will make more specific later)"legacy command`scp -rp pi@wpilibpi.local:/home/pi/MonsterVision/. c:/dev/MonsterVision/`"
5. sudo scp -rp /home/pi/MonsterVision/. <pc ip address>:c:/dev/MonsterVision/6. Open VSCode7. Git pull and push as required
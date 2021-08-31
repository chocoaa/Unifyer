# Unifyer

A tiny tool to transform your logitech Nordic Semiconductor nRF24LU1+ based nano receivers to Unifying receiver. Extracted and modified from BastilleResearch/nrf-research-firmware .

## Requirements

- Python (2.7)
- PyUSB

Install dependencies on Ubuntu:

```
sudo apt-get install python2-dev 
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py # Fetch get-pip.py for python 2.7 
sudo python2 get-pip.py
sudo pip install -U pip
sudo pip install -U -I pyusb
```

## Supported Hardware

- Logitech dongle (model C-U0007, Nordic Semiconductor based)

## Flash a Logitech Unifying dongle

*The most common Unifying dongles are based on the nRF24LU1+, but some use chips from Texas Instruments.
This tool is only supported on the nRF24LU1+ variants, which have a model number of C-U0007. The flashing
script will automatically detect which type of dongle is plugged in, and will only attempt to flash the nRF24LU1+ variants. In case you get the "Incompatible Logitech Unifying dongle" error while flashing on a C-U0007 dongle, comment out line 146 of unifying.py*

Update the linked official firmware repo from Logitech. Then, run the following command to flash the Logitech firmware onto the dongle, for C-U0007 dongles, use firmware under RQR12 directory:

```
git git submodule init
git submodule update
sudo ./prog/usb-flasher/logitech-usb-restore.py [path-to-firmware.hex]
```

## Places to modify

You may have to modify all the self.send_command() calls in unifying.py depending on your dongle. Usually you only need to modify the fourth input and the last "ep=0x82" ones if needed. 

For the fourth input, change it to (config.bNumInterfaces-1) if you are experiencing PyUSB error “USBError: [Errno 2] Entity not found”. 

For ep, try within 0x80 to 0x89 if the script cannot read back responses.
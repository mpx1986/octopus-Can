# octopus-Can

Octopus F446


sudo apt install git

cd ~
git clone https://github.com/th33xitus/kiauh.git
./kiauh/kiauh.sh

install Klipper experimental Phyton 3

cd ~
git clone https://github.com/Arksine/CanBoot


CanBoot

cd CanBoot
make menuconfig

U2C and RJ11

![CanBoot U2c](https://user-images.githubusercontent.com/66954448/185453142-44af78a4-3e55-4373-a3b3-531bfbb6ae7e.PNG)

DFU Mode connecting USB to Pi

lsusb (this number 0483:df11 can variat)

you see STMicroelectronics STM Device in DFU Mode

install CanBoot on Octopus (this number 0483:df11 can variat)
sudo dfu-util -a 0 -D ~/CanBoot/out/canboot.bin --dfuse-address 0x08000000:force:mass-erase:leave -d 0483:df11

verify Canboot
lsusb OpenMoko device stm32f446
ls /dev/serial/by-id/ should show a device serial.

note the /dev/serial/by-id/usb-CanBoot_stm32f446xx_<NUMBERSHERE>-if00 is needed for the next steps

  
Klipper
  
cd ~/klipper
make menuconfig
  
for U2C

Canbus on PA11/PA12 (USB C Port)
EBB V1.1/1.2 PB01/02 or EBB V1.0 PB8/PB9

![Octopus USB](https://user-images.githubusercontent.com/66954448/185452484-6c577663-7c2e-4472-923f-88ff9b32c189.PNG)



for RJ11

USB to Canbus Bridge PA11/PA12
PD0/PD1

![octopus RJ11](https://user-images.githubusercontent.com/66954448/185452500-aa0825a4-c8c0-4402-8d93-54f149ee3b81.PNG)


/dev/serial/by-id/usb-CanBoot_stm32f446xx_ your device number
  
 flash Klipper with
  
 python3 ~/CanBoot/scripts/flash_can.py -d  /dev/serial/by-id/usb-CanBoot_stm32f446xx_<NUMBERSHERE>-if00

CanO Network
  
sudo nano /etc/network/interfaces.d/can0
  
allow-hotplug can0
iface can0 can static
 bitrate 250000
 up ifconfig $IFACE txqueuelen 256
 pre-up ip link set can0 type can bitrate 250000
 pre-up ip link set can0 txqueuelen 256 
  
ctrl+x to save
  
sudo reboot
  
wire it up

lsusb you seen the can adapter
  
start the Octopus with 24v

  ~/CanBoot/scripts/flash_can.py -i can0 -q
  
all is ok - you will see a Canboot uuid
  
check the network with 
  ifconfig

 at the top is CanO
  
  
  
 WIRE THE EBB
  
  ~/CanBoot/scripts/flash_can.py -i can0 -q
  there will ne two uuids 
  
  

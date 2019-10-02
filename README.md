# Raspberry-DUMP1090
ADS-B Receiver to Track AirPlane with Raspberry PI

Work on:
- Raspberry PI 3 B
- Raspberry PI 3 B+
- Raspberry PI 4

Here's the code:
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git git-core cmake libusb-1.0-0-dev build-essential lsof

install the RTL-2832U USB dongle driver
git clone https://git.osmocom.org/rtl-sdr
cd rtl-sdr
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make -j4
sudo make install
sudo ldconfig
sudo ldconfig

**You should now do these steps to tell the system about what the new device is allowed to do, and reboot the system:
cd ~
sudo cp ./rtl-sdr/rtl-sdr.rules /etc/udev/rules.d/
sudo reboot
rtl_test

**On most versions of Raspian except very early ones, you will need to "blacklist" the rtl2830 device to stop the OS using the DAB/TV drivers for the device.
cd /etc/modprobe.d
sudo nano no-rtl.conf

**Add the following lines
blacklist dvb_usb_rtl28xxu
blacklist rtl2832
blacklist rtl2830

sudo reboot
rtl_test -t
**If you get error messages such as "cb transfer status: 1, canceling." it may be because the dongle is taking too much power from the Raspberry Pi. 

**Installing DUMP1090
cd ~ 
git clone git://github.com/flightradar24/dump1090
cd dump1090
sudo modprobe -r dvb_usb_rtl28xxu
sudo nano /etc/modprobe.d/rtl-sdr-blacklist.conf

**add the lines
blacklist dvb_usb_rtl28xxu

make
./dump1090 --interactive --net --net-http-port 8080

thanks to:
https://www.fontenay-ronan.fr/feed-flightradar24-and-flightaware-concurrently-with-dump1090/

Note:
I still getting the maps in Developer View. If you fix that please share it to us.

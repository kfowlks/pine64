# Pine64
pine64 ubuntu config

# Resize filesystem image

sudo /usr/local/sbin/resize_rootfs.sh

# Install basic tools

apt-get install wget
apt-get install vim
apt-get install wireless-tools

# Create interface script
/etc/network/interfaces.d/wlan0
~~~~
auto wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
iface wlan1 inet manual
~~~~
# wpa_supplicant.conf
/etc/wpa_supplicant/wpa_supplicant.conf
~~~~
#ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
#update_config=1
network={
   ssid="kktravel"
   psk=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   key_mgmt=WPA-PSK
}
~~~~
# Configure Wifi Module

# Show all network interfaces
ifconfig -a

# Display all availible wi-fi networks
iwlist wlan0

# Generate wpa-psk encoded password 
wpa_passphrase kktravel <plain text password> > /etc/wpa_supplicant.conf

wpa_cli -- ? Verify that the access point in connected enter "scan_results"

ifconfig  wlan0 up

ifup wlan0

# Verify that configuration is working

# disable ethernet

/etc/network/interfaces.d/eth0
~~~~
#auto eth0
#iface eth0 inet dhcp
~~~~

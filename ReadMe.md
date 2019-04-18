to set up the WIFI internet using 802.1x you can do the following:
1 - add section into /etc/wpa_supplicant/wpa_supplicant.conf
========================================================
network={
    ssid="XXXX8021x"
    proto=RSN
    key_mgmt=WPA-EAP
    pairwise=CCMP
    auth_alg=OPEN
    eap=PEAP
    identity="serhii_strizhak@XXXX.com"
    password=hash:<your_encrypted_password>
}
========================================================
2 - in file /etc/network/interfaces, add following:

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
    pre-up wpa_supplicant -B -Dwext -i wlan0 -c/etc/wpa_supplicant/wpa_supplicant.conf -f /tmp/wlan0.log
    post-down killall -q wpa_supplicant

========================================================
to encrypt your password, use this commands:

echo -n <password> | iconv -t utf16le | openssl md4
========================================================

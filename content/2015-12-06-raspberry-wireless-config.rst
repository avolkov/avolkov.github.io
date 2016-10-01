============================
Raspberry Pi Wireless config
============================

:date: 2015-12-06 00:57:30
:tags: raspberrypi networking wireless realtek ralink usb

Here's working wireless configuration I use to connect my Raspberry Pis with USB wifi dongles to my home network.

I've been using Realtek RTL8188CUS and Ralink RT5370 based devices rated for 150Mbps.

Dongles based on Ralink chip have been working flawlessly, but I have experienced two issues with the Realtek-based device. The adapter just doesn't work in AP mode, and if there's no network activity it will drop in power saving mode that can only be resumed packets are sent from the host.

In practical terms the host becomes unavailable for anyone who attempts to initiate a remote connection, however, this can be mitigated by creating a cron job that pings the router every minute.

This configuration that doesn't rely on GUI or NetworkManager starting up, assumes that **wpasupplicant** package is installed and wireless adapter is recognized by the system as **wlan0**.

Add the following to **/etc/network/interfaces**


.. code-block:: bash

    allow-hotplug wlan0
    iface wlan0 inet manual
    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp


Edit **/etc/wpa_supplicant/wpa_supplicant.conf** as such.

.. code-block:: bash

    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
            ssid="AP Name"
            psk="AP Secret"
            proto=RSN
            key_mgmt=WPA-PSK
            pairwise=CCMP
            auth_alg=OPEN
    }
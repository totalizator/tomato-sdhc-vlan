


---


# Multiple SSIDs (kept for reference) #

Notes on experimental/initial support of multiple SSIDs (guest wireless, no GUI support at this moment).

## WARNING ##

This whole thing is HIGHLY experimental and most likely, quite buggy. These instructions have only been tested on a WRT54GL v1.1, so proceed at your own risk.


---


# Setup #

## STEP 0 ##

Log in via telnet/SSH on your router and run "nvram get wl0\_corerev". If you get anything smaller than the number 9 (nine) for nvram variable "wl0\_corerev", please stop now (do not proceed any further).

## STEP 1 ##

Backup your router configuration, then install/flash a multiSSID-enabled firmware image suitable for your router and wait for your router to reboot.

## STEP 2 ##

Go to Basic/Network (basic-network.asp) and create/set up a new LAN bridge. Before considering this part 'done' and proceeding to the next step, make sure you have (at least) two LAN bridges configured (i.e. br0 and br1), each of them on its own IP/subnet (you probably want to enable DHCP on them, so double-check the whole thing before moving on). Also, make sure the 'standard' wireless interface is properly configured and enabled. Once you're done, hit 'save' at the bottom of the page and wait for a few moments as a few services/subsystems might need to be restarted on your router (i.e. network/firewall and/or other services).

## STEP 3 ##

Now go to Advanced/VLAN (advanced-vlan.asp), create a new VLAN and assign/link it to that new LAN bridge you just created on the last step. While not absolutely required, it might be a good idea to (re)assign at least one physical ethernet port on your router to this new VLAN (for troubleshooting purposes). Once you're done configuring VLANs, ports and LAN bridge assignments, hit 'save' at the bottom of the page and wait for your router to reboot (it's strongly recommended to just leave alone that 'Wireless' section on this page for now).

## STEP 4 ##

Once your device finishes rebooting and is back online, head on again to the same admin/pages mentioned above (Basic/Network and Advanced/VLAN) to triple-check your settings. At this point, you might want to test if DHCP is indeed working as it should on each of your LAN bridges/VLANs (as well as your wired/ethernet ports assignments).

## STEP 5 ##

Since we don't have a GUI just yet, open a telnet/SSH session to your router and get ready to configure your guest WLAN :-)


---


# WLAN Settings #

Consider the following example/scenario:

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_1_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_1_basic_network.png)

There are 2 LAN bridges configured (br0 and br1). DHCP is enabled on both.

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_2_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_2_advanced_vlan.png)

Ports 1, 2 and 3 are assigned to VLAN 1 (bridged to the primary LAN/br0). Port 4 is assigned to VLAN 3 (bridged to LAN1/br1). The WAN port is part of VLAN 2 (which is our WAN, anyways).

So, to create a guest wireless network without any encryption (open) and SSID 'guest', we'd use the following commands:

```
nvram set wl0_vifs='wl0.1'           # wl0/virtual interfaces
nvram set wl0.1_ifname='wl0.1'       # wl0.1 interface name
nvram set wl0.1_bss_enabled='1'      # this BSSID should be enabled
nvram set wl0.1_ssid='guest'         # the SSID of this network
nvram set wl0.1_closed='0'           # broadcast SSID
nvram set wl0.1_mode='ap'            # access point mode
nvram set wl0.1_radio='1'            # see NOTE below
nvram set lan1_ifnames='vlan3 wl0.1' # interfaces bridged to LAN1/br1
```
NOTE: http://tomatousb.org/forum/t-264199/multiple-ssid#post-1313099

Then, we need to restart the network subsystem on the router:
```
service net restart
```

And after a few moments... you'll have your 'guest' wireless network up and running:

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_3_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_example_3_advanced_vlan.png)

## Example settings for WEP ##
```
nvram set wl0_vifs='wl0.1'
nvram set wl0.1_bss_enabled='1'
nvram set wl0.1_radio='1'
nvram set wl0.1_security_mode='wep'
nvram set wl0.1_wep='enabled'
nvram set wl0.1_wep_bit='128'
nvram set wl0.1_ifname='w0.1'
nvram set wl0.1_ssid='guest'
nvram set wl0.1_closed='0'
nvram set wl0.1_key1='9FDF3BFDFB10AFEB0925EF9605'
nvram set wl0.1_key2='79966B0F8AF9ADB2F03DCCF0FC'
nvram set wl0.1_key3='D6272A95700B2BB90F1FFC1BF4'
nvram set wl0.1_key4='00E754864AA708294298A57DE0'
nvram set wl0.1_key='1'
nvram set wl0.1_passphrase='test'
nvram set wl0.1_mode='ap'
nvram set lan1_ifnames='vlan3 wl0.1'
service net restart
```

## Example settings for WPA2 Personal ##

```
nvram set wl0_vifs='wl0.1'
nvram set wl0.1_ssid='guest'
nvram set wl0.1_wpa_psk='supersecret'
nvram set wl0.1_bss_enabled='1'
nvram set wl0.1_radio='1'
nvram set wl0.1_mode='ap'
nvram set wl0.1_ifname='wl0.1'
nvram set wl0.1_security_mode='wpa2_personal'
nvram set wl0.1_crypto='tkip+aes'
nvram set wl0.1_wpa_gtk_rekey='3600'
nvram set wl0.1_akm='psk2'
nvram set wl0.1_auth_mode='none'
nvram set wl0.1_closed='0'
nvram set lan1_ifnames='vlan3 wl0.1'
service net restart
```

NOTE: if you want to configure/use any kind of WPA security in more than one wireless interface (WPA/WPA2, Personal/Enterprise and any/all variants/combinations), you should set the following nvram variable before running "service net restart":

```
nvram set nas_alternate='1'
```

When this nvram variable is set, an alternative method will be used to start as many instances of the "nas" service as needed (nas is the proprietary binary tool that sets up dynamic encryption on the wireless device).

Also, please notice this variable is only relevant on K24 builds (and silently ignored on K26 builds - see section "Possibly relevant for K26 builds", below).

Once you're done changing settings, you should save/commit nvram (so they will be 'remembered' by the router across reboots/restarts):
```
nvram commit
```

## ANOTHER WARNING ##

Sometimes I can't connect to the additional/guest SSID right after rebooting my WRT54GL (seems to be related having more than one wireless interface configured to use WPA). Currently, the only workaround is to connect via telnet/SSH to my router and restart networking services:
```
service net restart
```

Keep in mind this is a work in progress and... have fun!


---


# See Also #

### Possibly relevant for K26 builds ###

  * http://www.linksysinfo.org/index.php?threads/vlan-tags-work-for-you.34478/page-5#post-171805

### Some references: ###

  * http://tomatousb.org/forum/t-264199/multiple-ssid#post-1223051

  * http://linksysinfo.org/index.php?threads/vlan-tags-work-for-you.34478/page-3#post-171060

  * http://linksysinfo.org/index.php?threads/can-we-disable-linksys-e3000-guest-wireless-yet.35184/#post-171434

  * http://www.dd-wrt.com/phpBB2/viewtopic.php?t=16652

  * http://www.pennock.nl/dd-wrt/Multiple_BSSIDs.html

  * http://wiki.openwrt.org/oldwiki/wireless.nas

  * http://www.bingner.com/openwrt/wpa.html

  * http://repo.or.cz/w/tomato.git/shortlog/refs/heads/VLAN-MultiSSID

# WPAx not working on guest/virtual WLAN #

  * http://tomatousb.org/forum/t-264199/multiple-ssid#post-1307230

In brief: double-check _all_ your settings related to MAC addresses (on each/every interface). As an example, here's how to check your current/runtime settings for a virtual interface named 'wl0.1':

```
ifconfig wl0.1
wl -i wl0.1 bssid
```

If 'ifconfig' reports a different MAC address for your suspicious/problematic wireless interface, you can try setting it to the same value reported by the 'wl' command (and restart the 'nas' daemon right after that):

```
ifconfig wl0.1 down
ifconfig wl0.1 hw ether xx:xx:xx:xx:xx:xx
ifconfig wl0.1 up
```

Also, there's something else you could try (but you probably want to take a backup before running this next command on your router):

```
nvram unset wl0.1_hwaddr
nvram commit
reboot
```
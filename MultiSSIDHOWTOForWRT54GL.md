



---


# MultiSSID HOWTO for WRT54GL #


---


## BEFORE YOU BEGIN ##

This whole thing is HIGHLY experimental and most likely, quite buggy. These instructions have only been tested on a WRT54GL v1.1, so proceed at your own risk. Lots of things could go wrong: there are many risks involved, including, but not limited to bricking your router and/or even damaging it beyond repair. Please think carefully about what you're about to do before proceeding!

It's mostly a bunch of hacks I put together over the past few months for my very own/personal I'm sharing with everyone out there as open-source software (specially since this whole thing has been built on top of the GPL code in Tomato).

On the other hand, this might just work on your router (as it did on mine).

Feel free to drop a note either way.

Best of luck!



---



# Setup #



---



## STEP 0 - Getting what you'll need ##


  * one Linksys WRT54GL router/gateway/wireless access point
http://en.wikipedia.org/wiki/Linksys_WRT54G_series#WRT54GL
  * b) one Cat 5 ethernet cable
http://en.wikipedia.org/wiki/Category_5_cable
  * a computer with a graphical interface, running a W3C compliant browser
http://en.wikipedia.org/wiki/Standards-compliant
  * a Tomato firmware image compatible with your particular router (Teaman-ND or Teaman-ND-SDHC v0019 or later)
http://code.google.com/p/tomato-sdhc-vlan/downloads/list



---



## STEP 1 - Install firmware and reset any/all config settings ##


Connect your computer to your router via the ethernet cable and plug the power cord. Once the router is up and running, point your browser to the web administration interface. If you're already running another version of Tomato on your router, you can just go to the Administration -> Upgrade page and install a compatible Tomato firmware image/build on your router. Please make sure to mark/enable the checkbox saying "After flashing, erase all data in NVRAM memory " in order to wipe out all NVRAM settings and reset things to default values (username/password will be reset to 'admin'/'admin' and the IP address of the router should be 192.168.1.1).

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_01_admin_upgrade.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_01_admin_upgrade.png)



---



## STEP 2 - Configure WAN, LAN bridges and (primary) wireless interfaces ##


Go to the Basic -> Network page and configure your WAN/Internet settings. Next, create an additional LAN bridge (LAN1/br1):

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_a_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_a_basic_network.png)

Hit the 'Add' button to include it on the list:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_b_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_b_basic_network.png)

Also, you might want to change settings and/or any security options on your primary wireless interface:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_c_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_02_c_basic_network.png)

Once you're done, hit the 'Save' button and wait for the router to restart all services.



---



## STEP 3 - Configure VLANs ##

Go to the Advanced -> VLAN page and take a look at the current settings for VLANs and port assignments. On that page should look see something like this:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_a_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_a_advanced_vlan.png)

At this moment, you should create an extra VLAN and (re)assign at least one physical ethernet port to it. Although not strictly necessary, it might be advisable to rearrange things related to VLAN 0 into something else (i.e. mostly, in hopes of avoiding possible/future issues related to 802.1q trunks and any other devices). See this page for more details:
http://www.dd-wrt.com/wiki/index.php/Reconfigure_VLANs_for_802.1q_Compatibility

### STEP 3a - WAN ###

Click on VID 1 (WAN) and reassign it as VID 2:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sa_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sa_advanced_vlan.png)

### STEP 3b - LAN ###

Click on VID 0 (LAN), reassign it as VID 1 and unmark the checkbox for Port 4:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sb_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sb_advanced_vlan.png)

Once you're done, hit the 'OK' button (leave the 'Tagged' checkboxes alone for this moment, unless you really know what you're doing).

### STEP 3c - LAN1 ###

Add a new VLAN (VID 3), assign Port 4 (mark the checkbox for Port 4), and choose LAN1 in the last column:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sc_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sc_advanced_vlan.png)

### STEP 3d - Review settings and save ###

Have a good look at that page and double-check those settings before saving. Here's what it looks on mine:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sd_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_03_sd_advanced_vlan.png)

Once you hit the 'Save' button, the router will be rebooted (leave the 'Wireless' section alone for this moment).



---



## STEP 4 - Test your new VLANs and port assignments ##

Once your router is back online, you should be able to validate your new VLAN settings and port assignments (DHCP will offer IP addresses in different ranges, depending on which port you're plugging your computer). Do not skip this step: make sure DHCP is actually working as expected on all your LAN interfaces before proceeding.

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_04_device_list.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_04_device_list.png)



---



## STEP 5 - Configure your Guest wireless network ##

Go to the Advanced -> Virtual Wireless page and you'll see something like this:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_a_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_a_adv_virtual_wireless.png)

Enter your new SSID and select LAN1 on the last column, then hit 'Add' (only Access Point mode is supported at this time):

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_b_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_b_adv_virtual_wireless.png)

You'll notice the web interface will change and focus on details for this particular VIF:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_c_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_c_adv_virtual_wireless.png)


### Notes about non-primary/virtual wireless interfaces ###

A couple of notes worth mentioning about non-primary/virtual wireless interfaces:

**as the word 'virtual' suggests, there are a few settings only relevant and meaningful for primary wireless VIFs (i.e. wireless modes, channel/frequency, etc...), unavailable on these tabs.** the MAC address of new VIFs will be shown as just a bunch of zeroes and will be (re)generated when saving settings (also, be aware even after 'saving' stuff, the MAC addresses of any non-primary VIFs could change at any moment in the future without any warning and/or apparent reason...)

Anyways - you might wish to enable some security on your guest wireless network, change the SSID and/or enable/disable SSID broadcast, etc... - take your time and do this now. Also, please notice the 'Save' button is disabled on all tabs except for the 'Overview' page (this is intentional):

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_d_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_d_adv_virtual_wireless.png)

Once you're done configuring any specifics on each of your wireless VIFs, get back to the Overview page, review your settings. Once you're done, you mighy wanna hit the 'Save' button and wait for your router to restart:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_e_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_e_adv_virtual_wireless.png)


NOTE 1: if you've enabled WPAx security on more than one wireless interface (primary and/or virtual), you might need to enable/mark the "Use alternate NAS startup sequence" checkbox.

NOTE 2: this is only relevant on K24 builds (K26 builds don't even have this option available in the web interface).

NOTE 3: from the OpenWRT wiki - http://wiki.openwrt.org/doku.php?id=oldwiki:wireless.nas

> nas is the proprietary binary tool responsible for the dynamic encryption (WEP/WPA) in conjunction with the Broadcom proprietary drivers

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_f_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_05_f_adv_virtual_wireless.png)


---



## STEP 6 - Test and enjoy ##

Open the Status -> Overview page:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_06_a_status_overview.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_06_a_status_overview.png)

And take a look at the Status -> Device List page to check things out:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_06_b_device_list.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_06_b_device_list.png)



---



## STEP 7 - Optional - Define some access rules between your LAN bridges ##

By default, LAN bridges are isolated/unreachable from each other when created (that is, as soon as WAN is up and/or the firewall service kicks in). However, if you wish to allow some devices to be accessed from your guest network (i.e. perhaps a printer?), you might wanna head on to the Advanced -> LAN Access page to setup some access rules between your networks. Here's an example:

![http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_07_lan_access.png](http://tomato-sdhc-vlan.googlecode.com/files/wrt54gl_multissid_07_lan_access.png)

These rules would allow any devices on your primary LAN to reach devices on your guest network, but not the other way around: devices on your guest network could only reach/access services running on that specific IP address.



---



# See also #

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/ExperimentalMultiSSID


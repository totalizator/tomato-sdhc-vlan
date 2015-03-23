



---


# MultiSSID HOWTO for E3000 #


---


## BEFORE YOU BEGIN ##

This whole thing is HIGHLY experimental and most likely, quite buggy. These instructions have only been tested on a E3000 v1.0, so proceed at your own risk. Lots of things could go wrong: there are many risks involved, including, but not limited to bricking your router and/or even damaging it beyond repair. Please think carefully about what you're about to do before proceeding!

It's mostly a bunch of hacks I put together over the past few months for my very own/personal I'm sharing with everyone out there as open-source software (specially since this whole thing has been built on top of the GPL code in Tomato).

On the other hand, this might just work on your router (as it did on mine).

Feel free to drop a note either way.

Best of luck!



---



# Setup #



---



## STEP 0 - Getting what you'll need ##


  * one Cisco/Linksys E3000 router/gateway/wireless access point
http://en.wikipedia.org/wiki/Linksys_routers#E3000
  * one Cat 5 ethernet cable
http://en.wikipedia.org/wiki/Category_5_cable
  * a computer with a graphical interface, running a W3C compliant browser
http://en.wikipedia.org/wiki/Standards-compliant
  * a Tomato firmware image compatible with your particular router
http://code.google.com/p/tomato-sdhc-vlan/downloads/list



---



## STEP 1 - Install firmware and reset any/all config settings ##


Connect your computer to your router via the ethernet cable and plug the power cord. Once the router is up and running, point your browser to the web administration interface. If you're already running another version of Tomato on your router, you can just go to the Administration -> Upgrade page and install a compatible Tomato firmware image/build on your router. Please make sure to mark/enable the checkbox saying "After flashing, erase all data in NVRAM memory " in order to wipe out all NVRAM settings and reset things to default values (username/password will be reset to 'admin'/'admin' and the IP address of the router should be 192.168.1.1).


![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_01_admin_upgrade.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_01_admin_upgrade.png)


NOTE: DHCP is disabled by default on Teaman-RT builds (just like on most Toastman builds), so you must configure a static IP address on your computer before proceeding. This is what I've used on my Linux machine:

```
ifconfig eth0 192.168.1.2 netmask 255.255.255.0 up
```



---



## STEP 2 - Configure WAN, LAN bridges and (primary) wireless interfaces ##

Go to the Basic -> Network page and configure your WAN/Internet settings. Next, create an additional LAN bridge (LAN1/br1):


![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_a_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_a_basic_network.png)

Hit the 'Add' button to include it on the list:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_b_basic_network.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_b_basic_network.png)

NOTE: please do remember to double-check your settings for your primary LAN (br0) and ensure DHCP is enabled.

Also, you might want to change settings and/or any security options on your primary wireless interfaces:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_c_basic_network_v2.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_02_c_basic_network_v2.png)

Once you're done, hit the 'Save' button and wait for the router to restart all services.



---



## STEP 3 - Configure VLANs ##

Go to the Advanced -> VLAN page and take a look at the current settings for VLANs and port assignments. On that page should look see something like this:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_a_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_a_advanced_vlan.png)

At this moment, you should create an extra VLAN and (re)assign at least one physical ethernet port to it.

### STEP 3a - LAN ###

Click on VID 1 (LAN) and unmark the checkbox for Port 4:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sa_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sa_advanced_vlan.png)

Once you're done, hit the 'OK' button (leave the 'Tagged' checkboxes alone for the moment - unless you really know what you're doing).

### STEP 3b - LAN1 ###

Add a new VLAN (VID 3), assign Port 4 (mark the checkbox for Port 4), and choose LAN1 in the last column:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sb_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sb_advanced_vlan.png)

### STEP 3c - Review settings and save ###

Have a good look at that page and double-check those settings before saving. Here's what it looks on mine:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sc_advanced_vlan.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_03_sc_advanced_vlan.png)

Once you hit the 'Save' button, the router will be rebooted (leave the 'Wireless' section alone for this moment).



---


## STEP 4 - Test your new VLANs and port assignments ##

Once your router is back online, you should be able to validate your new VLAN settings and port assignments (DHCP will offer IP addresses in different ranges, depending on which port you're plugging your computer). Do not skip this step: make sure DHCP is actually working as expected on all your LAN interfaces before proceeding.

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_04_device_list.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_04_device_list.png)



---



## STEP 5 - Configure your Guest wireless network ##

Go to the Advanced -> Virtual Wireless page and you'll see something like this:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_a_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_a_adv_virtual_wireless.png)

Enter your new SSID and select LAN1 on the last column, then hit 'Add' (only Access Point mode is supported at this time):

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_b_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_b_adv_virtual_wireless.png)

You'll notice the web interface will change and focus on details for this particular VIF:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_c_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_c_adv_virtual_wireless.png)

A couple of notes worth mentioning about non-primary/virtual wireless interfaces:

**as the word 'virtual' suggests, there are a few settings only relevant and meaningful for primary wireless VIFs (i.e. wireless modes, channel/frequency, etc...), unavailable on these tabs.** the MAC address of new VIFs will be shown as just a bunch of zeroes and will be (re)generated when saving settings (also, be aware even after 'saving' stuff, the MAC addresses of any non-primary VIFs could change at any moment in the future without any warning and/or apparent reason...)

Anyways - you might wish to enable some security on your guest wireless network, change the SSID and/or enable/disable SSID broadcast, etc... - take your time and do this now. Also, please notice the 'Save' button is disabled on all tabs except for the 'Overview' page (this is intentional):

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_d_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_d_adv_virtual_wireless.png)

Since the E3000 can have two physical wireless interfaces simultaneously active, you might want to add yet another wireless VIF for the second physical wireless interface (on the first column, remember choosing an 'interface' like wl1.x at this time):

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_e_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_e_adv_virtual_wireless.png)


![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_f_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_f_adv_virtual_wireless.png)


![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_g_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_g_adv_virtual_wireless.png)

Once you're done configuring any specifics on each of your wireless VIFs, get back to the Overview page, review your settings. Once you're done, you mighy wanna hit the 'Save' button and wait for your router to restart:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_h_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_05_h_adv_virtual_wireless.png)

NOTE: you might need to power-cycle your router at this stage (unplug power, wait for about a minute, then plug power back and wait for your router to boot).



---



## STEP 6 - Test and enjoy ##

Open the Status -> Overview page:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_06_a_status_overview.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_06_a_status_overview.png)

And take a look at the Status -> Device List page to check things out:

![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_06_b_device_list.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_06_b_device_list.png)



---



## STEP 7 - Optional - Define some access rules between your LAN bridges ##

Once created, LAN bridges are supposed to be isolated from each other by default (that is, as soon as WAN is up and/or the firewall service kicks in). However, if you wish to allow some devices to be accessed from your guest network (i.e. a printer?), you might wanna head on to the Advanced -> LAN Access page to setup some access rules between your networks. Here's an example:


![http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_07_lan_access.png](http://tomato-sdhc-vlan.googlecode.com/files/e3000_multissid_howto_07_lan_access.png)


These rules would allow any devices on your primary LAN to reach devices on your guest network, but not the other way around: devices on your guest network could only reach/access services running on that specific IP address.



---



# See also #

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/ExperimentalMultiSSID


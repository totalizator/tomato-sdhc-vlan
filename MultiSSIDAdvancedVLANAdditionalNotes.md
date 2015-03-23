



---



# MultiSSID and Advanced/VLAN on Teaman-ND/ND-SDHC/RT builds: Additional Notes #



---



## INTRODUCTION ##

This page assumes you've been/read through pages such as [MultiSSIDHOWTOForWRT54GL](http://code.google.com/p/tomato-sdhc-vlan/wiki/MultiSSIDHOWTOForWRT54GL), [MultiSSIDHOWTOforE3000](http://code.google.com/p/tomato-sdhc-vlan/wiki/MultiSSIDHOWTOForE3000) as well as the other pages about these topics on the wiki. This page describes what's changed on between those versions and more recent builds: a few new/extra features regarding recent MultiSSID improvements and Advanced VLAN ID mappings included since Teaman-ND/SDHC/RT v24 builds.

Take a look at the links mentioned on the "See also" section, below.



---



## MAC Addresses and CFE/defaults ##


After any kind of 'factory reset' or 'configuration restore' on the "Administration / Configuration" page in your router, you should probably check the the MAC address being used/reported for the Wireless interface by the system and cross-reference it with the 'base/LAN' MAC address on your router as in some occasions, they seem to be reset to some kind of default value hard-coded/found inside the CFE of some models.

If you see such thing (such as '00:90:4C:5F:00:2A' or something 'really different' from your LAN MAC address):

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_01_mac_address_a.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_01_mac_address_a.png)

Then you probably want to hit the 'Default' button next to the 'Wireless Interface' MAC address field:

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_02_mac_address_b.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_02_mac_address_b.png)


### Note on Models Affected ###


This has been observed only on routers like the WRT54GL, but it's possible this could happen in other devices as well.

Usually, WAN should be using the LAN MAC plus one unit (LAN + 1) and any wireless units should follow the same rule: wl0/eth1 should be LAN + 2 and wl1/eth2 should be LAN + 3 (in dual-radio/supported devices).



---



## Virtual Wireless Interfaces and MAC Addresses ##


Sometimes the (closed-source/binary) WL driver simply decides to use it's own numbering/rules for configuring/setting the MAC addresses of (virtual) wireless interfaces at startup (instead of using the values/settings we told it to use, stored on NVRAM). While this may not seem like a major issue, in some situations this could lead to problems and/or unexpected behavior - specially when there's a 'nas' daemon involved (i.e. when there's WPAx encryption, etc...).

Therefore, after creating any WLIFs (or changing any settings), saving your settings and letting the network subsystem restart, it's usually a good idea to revisit the "Advanced: Virtual Wireless Interfaces" page and check/verify for any warnings under each defined/configured WLIFs/tabs, like this:

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_03_adv_virtual_wireless.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_03_adv_virtual_wireless.png)

If there are indeed any discrepancies between the runtime configuration being used by the OS and the WL driver, you can use the "Advanced / MAC Address" page to set things right (use the BSSID/MAC address being reported by the WL driver).


### Note on Models Affected ###


This has been observed only on one of my WRT54GL routers, but it's possible this could happen in other devices as well (it's been reported to sometimes happen on the E4200v1 and other routers, but the actual/root causes for this odd behavior are still unclear).



---



## Advanced VLAN Stuff ##


There are four sections on this new/enhanced version of the "Advanced / VLAN" page:


### VLAN Settings and VID Mappings ###


In the main configuration table we have:

![http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_04_adv_vlan_a.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_04_adv_vlan_a.png)


**VLAN - the VLAN identification number - from 0 to 15 (max 16 VLANs can be defined).** VID - the VLAN ID/tag to be used by a particular VLAN. Under normal circumstances, this should match the VLAN numbering (leave it at "0" when editting).
**Port X/Tagged - member ports of a VLAN, as well as we should expect tagged and/or untagged Ethernet frames on that port.** Default - to which VLAN should be assigned to any untagged frames received by the router (with unknown/no tagging information).
**Bridge - part of a LAN or WAN interface (or "none" - manually managed).**


### VID Offset ###


Allows "shifting" the VID interval being used for your VLANs:


![http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_05_adv_vlan_b.png](http://tomato-sdhc-vlan.googlecode.com/files/multissid_adv_vlan_extras_05_adv_vlan_b.png)


From the "Notes" section:

> First 802.1Q VLAN tag to be used as base/initial tag/VID for VLAN and VID assignments. This allows using VIDs larger than 15 on (older) devices such as the Linksys WRT54GL v1.x (in contiguous blocks/ranges with up to 16 VLANs/VIDs). Set to '0' (zero) to disable this feature and VLANs will have the very same/identical value for its VID, as usual (from 0 to 15).


### Wireless ###


Allows modifying/changing assignments of existing/configured wireless interfaces to different LAN briges. This section will probably be deprecated soon - you should probably be using and/or check things on Advanced/Virtual Wireless and Basic/Network.


### Notes ###


  * VLAN - Unique identifier of a VLAN.
  * VID - EXPERIMENTAL - Allows overriding 'traditional' VLAN/VID mapping with arbitrary VIDs for each VLAN (set to '0' to use 'regular' VLAN/VID mappings instead). Warning: this hasn't been verified/tested on anything but a Cisco/Linksys E3000 and may not be supported by your particular device/model (see notes on "VID Offset" below).
  * Ports 1-4 & WAN - Which ethernet ports on the router should be members of this VLAN.
  * Tagged - Enable 802.1Q tagging of ethernet frames on a particular port/VLAN
  * Default - VLAN ID assigned to untagged frames received by the router.
  * Bridge - Determines if this VLAN ID should be treated as WAN, part of a LAN bridge or just left alone (i.e. member of a 802.1Q trunk, being managed manually via scripts, etc...).

  * VID Offset - EXPERIMENTAL - First 802.1Q VLAN tag to be used as base/initial tag/VID for VLAN and VID assignments. This allows using VIDs larger than 15 on (older) devices such as the Linksys WRT54GL v1.1 (in contiguous blocks/ranges with up to 16 VLANs/VIDs). Set to '0' (zero) to disable this feature and VLANs will have the very same/identical value for its VID, as usual (from 0 to 15).

  * Wireless - Assignments of wireless interfaces to different LAN briges. You should probably be using and/or check things on Advanced/Virtual Wireless and Basic/Network.

  * Other relevant notes/hints:
    * One VID must be assigned to WAN.
    * One VID must be selected as the default.
    * To prevent 802.1Q compatibility issues, avoid using VID "0" as 802.1Q specifies that frames with a tag of "0" do not belong to any VLAN (the tag contains only user priority information).
    * It may be also recommended to avoid using VID "1" as some vendors consider it special/reserved (for management purposes).

  * This is highly experimental and hasn't been tested in anything but a Linksys WRT54GL v1.1 running a Teaman-ND K24 build and a Cisco/Linksys E3000 running a Teaman-RT K26 build.
  * There's lots of things that could go wrong, please do think about what you're doing and take a backup before hitting the 'Save' button on this page!
  * You've been warned!



---



## See also ##

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/MultiSSIDHOWTOForWRT54GL

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/MultiSSIDHOWTOForE3000

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/ExperimentalMultiSSID

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/VLANMultiSSID

  * http://code.google.com/p/tomato-sdhc-vlan/wiki/Changelog
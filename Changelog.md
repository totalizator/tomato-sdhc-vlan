



---



# Changelogs #

## Teaman-ND-SDHC ##


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0025-SDHC

  * MultiSSID: WPAx fixes - skip WPAx/nas restart (can cause problems with WAN as Wireless Client)
  * IP Traffic: Fix JS error causing initial load delay when default nvram 'tomato' skin (Moc-RT-N)
  * Add SNMP configuration options for custom port and remote access (Moc-RT-N)
  * Add configuration options for the WAN ICMP Rate limiting (Moc-RT-N)
  * ash: fix execution of shell scripts without shebang (JYAvenard)
  * Samba security fix - "root" credential remote code execution (Toastman)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0024-SDHC

  * EXPERIMENTAL VLAN/VID mapping GUI (allows using full range/4k 802.11Q tagging on some models - see [Issue #6](https://code.google.com/p/tomato-sdhc-vlan/issues/detail?id=#6))
  * IPTraffic:
    * change NVRAM defaults to 'disabled'
    * check/validate minimum size of LAN/subnets only when cstats is enabled
  * MultiSSID:
    * check/warn about any BSSID/MAC address discrepancies on WL VIFs (Advanced: Virtual Wireless, advanced-wlanvifs.asp)
    * fix WL MAC filter to include all WL VIFs (Basic: Wireless Filter, basic-wfilter.asp)
  * PPTPD:
    * fix/config dnsmasq to answer DNS queries on pppX interfaces
    * fix PPTP when used over WAN setup using PPPoE
    * allow specify 'custom' config/settings on webUI
  * K24: fix single-line MLPPP (by Shibby - see [Issue #20](https://code.google.com/p/tomato-sdhc-vlan/issues/detail?id=#20))
  * robocfg: quick fix for segfaults on K24 builds
  * firewall: allow DMZ-style forwarding to any LAN bridges (no longer restricted to primary LAN)
  * VLAN-GUI: include E4200v1 (VLAN 802.1q support confirmed by users - see [Issue #9](https://code.google.com/p/tomato-sdhc-vlan/issues/detail?id=#9))
  * webUI/httpd: fix wl\_mitigation typo
  * webUI: fix showing IPv6 info on VLAN-GUI-enabled builds (status-overview.asp)
  * rc: syslogd custom path improvements (tries to preserve messages at syslogd restart, recreate only if it's a symlink/custom path)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0023-SDHC

  * NEW FEATURE: PPTP Server for Tomato (experimental)
  * MultiSSID: fixes saving WL VIF settings when net mode is 'n-only'
  * rc: fix dhcp options for non-VLAN builds (only pushes default GW on VLAN-GUI-enabled builds)
  * VLAN-GUI: increased max number of devices/interfaces supported from 16 to 32


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0022-SDHC

  * MultiLAN: restrict LAN Access to IPv4 addresses on firewall rules
  * VLAN-GUI: fix/load current settings when trunk\_vlan\_so is enabled (tagging)
  * MultiLAN: fix WOL when we have more than one LAN bridge (br0-br3) - thanks Shibby!
  * MultiSSID: visual/usability improvements/fixes when creating new WL VIFs


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0021-SDHC

  * MultiSSID bugfix (when saving settings for non-WPAx VIFs, i.e. open or WEP)
  * IP Traffic: when 'disabled', we now mean 'completely disabled' (i.e. no iptables rules are created, no IPv4 accounting, nada...)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0020-SDHC

  * dnsmasq: fix DNS/hostname resolution on LAN bridges with DHCP disabled
  * web UI bugfix: command output was hidden inside the 'Notes' section (Tools/System page)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0019-SDHC

  * EXPERIMENTAL Web UI for multiple virtual/guest WLANs (v2)
  * IGMPProxy: ignore non-primary addresses on upstream VIF (prevents issues when "Route Modem IP" is enabled and WAN is set to DHCP)
  * UDPxy: fixed symlink for udpxrec
  * Add route modem IP option when WAN is set to "Static IP"


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0018-SDHC

  * cstats/rstats improvement: allow 'grace period' at shutdown (avoid longer timeouts if child processes seem to be really stuck)
  * miniupnp: Fix infinite loop in SendResp\_upnphttp() (from upstream)
  * EXPERIMENTAL Web UI for multiple virtual/guest WLANs (v1)
  * MultiSSID: up to 4 WL VIFs per physical interface allowed on WebUI


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0017-SDHC

  * Static ARP: option to enable all known devices
  * IPTraffic: K24 ipt\_account module updated to v0.1.20
  * rc/firewall: reorg ipt\_account rules creation
  * IPTraffic: realtime stats gathering fixes - get rid of nested loop for realtime conntrack counting


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0016-SDHC

  * SDHC/MMC: allow setting mountpoint for sdcard via web UI
  * cstats/rstats: wait any helper processes complete at service shutdown (helps preventing history/datafile corruption)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0015-SDHC

  * Dropbear: config tuneup to reduce size
  * Allow custom syslog file path + additional log rotate options
  * Added CPU frequency, chipset type and flash size for K24 builds
  * Web UI/httpd: improved checking of WL ifaces operational state (up/down)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0014-SDHC

  * Option to show hostnames and/or IPs on IPTraffic graphics (improved readability)
  * VLAN-GUI: include E4200 port mapping
  * Bugfix for static DHCP lease times (merge from VLAN-GUI)
  * Included all LAN hostnames/IPs on /etc/hosts (for completeness)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0013-SDHC

  * VLAN-GUI: Added "Route Modem IP" option (from Tomato/MLPPP)
  * VLAN-GUI: Fix compilation error when building without VLAN-GUI enabled
  * IPTraffic: fix sortCompare functions (column sorting)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0012-SDHC

  * VLAN-GUI: include WHR-G54S port mapping
  * Added Udpxy v1.0-Chipmunk-build21 (alternative to igmpproxy/multicast)
  * Dnsmasq 2.59 update
  * dropbear 0.54 update


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0011-SDHC

  * Fix default MAC address calculation of virtual WL ifaces
  * Fix handling of DHCP IP ranges for big LAN subnets
  * Added CPU % usage to Status Overview page


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0010-SDHC
  * (...)



---



## Teaman-RT ##


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2025

  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0025-SDHC

http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2024

  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0024-SDHC
  * Updates from Toastman-RT up to http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Toastman-1.28.7497.1
  * PPP3G fixes/enhancements:
    * allow using WAN port for LAN/br0 with PPP3G as WAN
    * add more ttyUSBx devices available for selection on basic-network.asp
    * don't disable USB 3G box when storage checkbox is not checked (by Shibby)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2023

  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0023-SDHC
  * Updates from Toastman-RT up to http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Toastman-1.28.7496.2 (fix usb\_modeswitch/libusb error, improvements on USB detection/old scheme first, QoS fixes)


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2022

  * NEW FEATURE: 3G modem support (from branch tomato-shibby)
  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0022-SDHC
  * Updates from Toastman-RT up to http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Toastman-1.28.7495.2


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2021

  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0021-SDHC


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2020

  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0020-SDHC
  * Fix "Previous WAN IP" on Status Overview page


http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-RT-1.28.2019

  * For K26 builds and MIPSR2 routers (i.e. E3000)
  * Same updates as of http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Teaman-ND-1.28.0019-SDHC
  * Code merged with Toastman-1.28.7494.1 (http://repo.or.cz/w/tomato.git/shortlog/refs/tags/Toastman-1.28.7494.1)



---



## Changelog (deprecated) ##

Here's a short/brief list of features currently implemented/available on branch VLAN-GUI v8 as of today (2011-07-29):

  * up to 4 LAN bridges can be set up via web interface
  * each LAN bridge must have it's own IP address/netmask set
  * DHCP server can be enabled/configured independently for/on each LAN bridge (i.e. different IP ranges, lease times)
  * Spanning Tree Protocol can be enabled/disabled on each bridge individually
  * WLAN can be assigned to be part of any existing/configured LAN bridge, not just the primary LAN (br0)
  * when/if enabled, the web management interface on the router should be accessible from/on all LAN bridges
  * up to 16 VLANs can be created/configured on the device (with VIDs ranging from 0 to 15)
  * each VLAN can be configured/treated as: WAN, part of a LAN bridge or unassigned
  * in general, each individual/physical ethernet port on the device can be assigned as a member/participant of a single VLAN
  * on devices (known) to support tagging of ethernet frames, it's possible to configure one (physical) ethernet port as a 802.1q trunk (member of multiple VLANs)
  * static routes can be added/configured onto specific LAN bridges
  * MiniuPnPd, RIP and IGMPproxy can be configured to listen/bind only on selected/enabled interfaces
  * LAN bridges are isolated from each other by default (not accessible to each other)
  * access/communication between different LAN bridges can be configured via web interface
  * code based on TomatoUSB 1.28.8754
And there's also a (non-exhaustive) list of known issues, limitations and warnings (aka things you show consider/know about VLAN-GUI before trying it out):

  * as bridge br0 is supposed to be the 'primary' LAN interface, it cannot be deleted and must always have a IP valid address set
  * if a default gateway is set, it must be reachable via br0 (being the primary LAN interface)
  * each LAN bridge must have it's own IP address/netmask set (if a LAN bridge is created and a proper IP address/netmask pair is not set or... if two different LAN bridges are configured with similar/conflicting IP addresses and/or overlapping subnets, the outcome/results can be unpredictable)
  * although it's possible to create/configure up to 16 VLANs on devices like the WRT54GL (and possibly even more VLANs on other devices), it's usually a good idea to avoid using VID 0 to prevent 802.1q compatibility issues (as 802.1q specifies that frames tagged with VID 0 do not belong to any VLAN).
  * since a lot of code in Tomato assumes there's only one bridge/LAN (br0), we can probably/safely assume that any functionality/feature not mentioned above should be, most likely, restricted to work/function only on/with br0 (i.e. WDS/hotplugging, ondemand PPP dialing, OpenVPN-related things, etc...)
  * ipv6 support is uncertain to say the least (absolutely no testing has been done so far)

Tested partially/only tested on these models:
  * WRT54GL v1.x , WRT54G v2
  * RT-N16, E3000
  * Netgear WNR3500L



---



## Older Versions ##

Not exactly a 'proper' changelog - but close - this is a raw/partial copy of 'git log' on my local repo:

commit a27d2bf7d91eb9de2be7926272cdc919f78f2b0b
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 24 03:05:55 2011 -0300

  * New CPU frequency detection, flash size (status-overview) and updates on various GUI pages (mostly, adding notes and navigation menu layout).

commit 1a3deeb707d732921a68bc964467539aa9d460ad
Author: Augusto Bott <augusto@bott.com.br>
Date:   Wed Jun 22 07:55:37 2011 -0300

  * Corrected a bug on advanced-vlan.asp introduced on the previous commit, which was preventing the form from submitting (related to experimental settings/trunk\_vlan\_support\_override).

commit c7cb00279fae2f215f10920ab32722deb3f35a62
Author: Augusto Bott <augusto@bott.com.br>
Date:   Wed Jun 22 03:50:39 2011 -0300

  * Minor change due to how BASH was (mis)interpreting dot (aka source) on recent versions of Ubuntu

commit 323c66296ef51b87e23904b1968995f79955c0f0
Author: Augusto Bott <augusto@bott.com.br>
Date:   Mon Jun 20 03:15:42 2011 -0300

  * Instead of checking against boardtype, VLAN support for a particular router model is now checked against boardflags (BFL\_ENETVLAN, 0x0100). Trunk-based VLAN support is still checked against boardtype, but the GUI now has a checkbox available to allow overriding this check (for experimental purposes, perhaps). Physical port-ordering checks against a few additional boardtypes: WRT54GL 1.x, WL-520GU, WL-500G Premium v2, WRT54GS 2.x, WRT320N/E2000, WRT610Nv2/E3000 (not thoroughly tested). As internal switch-port-number can be different according to each router model (5 on FastE, 8 on GigE), it's now 'detected' while reading configuration from NVRAM (also not thoroughly tested, but all information found so far suggests it should work across all of them).

commit 258a7b26e75b699413f83104b7ee560d8745fc3b
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 10 07:41:05 2011 -0300

  * Minor adjustments (cosmetics/CSS, web page names)

commit fd5454d67958f6ba9836c01e7b490e2162df522a
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 10 07:00:52 2011 -0300

  * Added 'previous WAN IP' and ISP concentrator ID from TomatoRAF (Victek)

commit da9663a1dd7fb549b3839f65bfc6145cf31a9b38
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 10 06:50:33 2011 -0300

  * Correction of two minor typos regarding clkfreq (CPU frequency)

commit b5cd4981d0cf8fb5af69bd27e22a663920efed22
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 10 06:34:43 2011 -0300

  * Added IP/range BW limiter, ARP binding, CPU frequency selection from TomatoRAF (Victek)

commit 91fddd44dd7c6383bc932d352ad05629adc98592
Author: Augusto Bott <augusto@bott.com.br>
Date:   Fri Jun 10 03:41:26 2011 -0300

  * Changed TCP/UDP timeouts for high P2P load (i.e. Tomato RAF), patched Speedmod, enabled selecting PFIFO (instead of only SFQ for tc) see also http://touristinparadise.blogspot.com/2008/04/linksys-wrt54gl-routers-improving.html

commit 544c077367fe36afec7b961d6cfa5b9908f4ac0e
Author: Augusto Bott <augusto@bott.com.br>
Date:   Tue May 3 23:13:00 2011 -0300

  * First VLAN GUI version. Tested on WRT54GL v1.1 and WRT54G v2
---
title: "Installation"
date: 2023-05-22T11:13:37+04:00
draft: true
tags:
  - "gentoo"
---

# Media installation

## dd

```
dd if=<path-to-iso> of=/dev/<disk> status=progress conv=sync
```

**status=progress** is need to see progress of copying.  
**conv=sync** is need to

# Keyboard

## loadkeys

On my old laptop keyboard, `Ctrl` is broken because of that I remapped `Caps Lock` to be my `Ctrl`.

`loadkeys`'s map files use simple syntax(`man 5 keymaps`).
`keymaps 0-n` say that keycodes will be changed with this map.
`keycode keynumber = keysym keysym keysym...` say that keycode should be remapped with keysym values.

`showkey` can help you get key keynumber.

`dumpkeys` give you a full keycodes list.

# Net

## ip

**ip** --- show / manipulate routing, network devices, interfaces and tunnels.

To check ip of your machine you can use this command:

```
id addr
```

## iw

**iw** --- show / manipulate wireless devices and their configuration

**Use it if your AP doesn't use WPA**, because it simpler that wpa_supplicant.

Scan networks

<pre><code>iw <i>OBJECT</i> scan</code></pre>

_OBJECT_ it is:

- dev <interface name> --- network interface.

- phy <phy name> --- wireless hardware device (by name).

- phy#<phy index> --- wireless hardware device (by index).

- reg --- regulatory agent.

## wpa_supplicant

**wpa_supplicant** --- Wi-Fi Protected Access client and IEEE 802.1X
supplicant

Fast connection:

```
wpa_supplicant -i <INTERFACE> -c <(wpa_passphrase <SSID> <PASSPHRASE>) -B
```

For instanse:
`<INTERFACE>` could be wlp3s0.  
(You can check name of your with `ip addr`. Usually starts with `w`)

`-i` specify the interface.  
`-c` specify the config for the interface.  
`-B` run in the daemon mode.

Or you can create configuration file.
The conifg file may be necessary if your access point uses WPA2 or WPA3 with PMF.

Example config to connect to a WPA2 AP with PMF enabled:

```
network={
ieee80211w=2 # Enable IEEE 802.10w standard
ssid=<YOUR-SSID>
key_mgmt=WPA-PSK WPA-PSK-SHA256
psk=<YOUR-GENERATED-PSK>
}
```

> [Good configuration file](https://raw.githubusercontent.com/vanhoefm/hostap-wpa3/master/wpa_supplicant/wpa_supplicant.conf)

# Disks

## GPT and MBR

Usually the disk block diveces on which the OS will be installed are divided into **smaller**,
**more manageable** block devices --- **partitions**.

There are two standard technologies:

- The **MBR**(DOS disklabel) is used for the legacy **BIOS boot**.
- The **GPT** is used for the **UEFI**

```
GUID Partition Table (GPT)
Master boot record (MBR) or DOS boot sector
```

> If your hardware is pre-2010 you should use MBR,
> otherwise GPT is the best choice.

## Filesystems

#### btrfs

btrfs --- is a cool fs, which has a lot of features and also speed.

#### ext4

ext4 --- is a stable and good fs.

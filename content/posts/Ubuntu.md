+++
title = 'Ubuntu 20.04 und WLAN auf einem MacBook Pro 8,1 (early 2011)'
date = 2021-03-14T10:39:20+01:00
draft = false
+++

2011 kaufte ich mir ein MacBook Pro. Mein zweites Produkt von Apple, nach einem iPod nano. Seitdem funktioniert es, abgesehen vom Einbau einer SSD und mehr RAM, ohne Eingriffe hervorragend und bekam nie ein Clean Install. Immer nur Updates auf neue Versionen von OS X/macOS. Mit macOS 10.13 war 2017 allerdings Schluss. Einige Zeit gab (gibt?) es zwar noch Updates, aber ich hatte ohnehin keinen großen Bedarf an macOS mehr – also installierte ich Ubuntu 20.04.

Dabei gab es keine Überraschungen, einzig eine Verbindung zum WLAN war nicht möglich, da Ubuntu die Karte (eine Broadcom BCM4331) nicht kannte. Hier hilft die Installation der Broadcom Station Treiber:

```
sudo apt install broadcom-sta-dkms
```


Außerdem wurde beim Boot die Meldung

```
Failed to Set MokListRT: Invalid Parameter
Could not create mokListRT: Invalid Parameter
Importing MOK states has failed: import_mok_state() failed: Invalid Parameter
Continuing boot since secure mode is disabled.
```


angezeigt. Los wird man sie so:

```
cd /boot/efi/EFI/ubuntu
cp grubx64.efi shimx64.efi
```


Reboot – läuft.
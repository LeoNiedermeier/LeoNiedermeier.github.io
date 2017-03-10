---
title: Setup VirtualBox for development
tags: [misc]
category: misc
summary: "Setup a VirtualBox for development with java, eclipse etc."
---

# General

## VirtualBox

<div class="danger" title="Dynamically allocated image" markdown="1">
Using a **dynamically allocated image** does **NOT** mean that the file system size of the guest system grows dynamically. It means that the image file will be grow on demand. Therefore select a appropriate size of the guest file system. 

> This will initially be very small and not occupy any space for unused virtual disk sectors, but will grow every time a disk sector is written  
(https://www.virtualbox.org/manual/ch05.html) 

</div>

## Installing Lubuntu

<http://lubuntu.net/>

<div class="success" title="Partition" markdown="1">
We do not use the default partitioning. If the swap partition is left of the root partition, it is easier to grow the root partition. Resizing the size of a VirtualBox disk needs two steps

1. Resize the VirtualBox disk in the host
2. Resize the partitions in the guest

See for instance <http://derekmolloy.ie/resize-a-virtualbox-disk/>
</div>

![Installation Tye](setup_virtual_box_for_development/lubuntu_install_1.png  "Installation Type")

![Partition Create](setup_virtual_box_for_development/lubuntu_install_2.png  "Partition Create")

![Swap Area](setup_virtual_box_for_development/lubuntu_install_3.png  "Swap Area")

![Root](setup_virtual_box_for_development/lubuntu_install_4.png  "Root")

![Partition](setup_virtual_box_for_development/lubuntu_install_5.png  "Partition")


## Installing Guest Additions
 
 Required packages:
 
* make
* gcc

# Tweaks

## Set a Theme

{: .success title="Themes"}
A Windows 10 like theme:
<http://b00merang.weebly.com/windows-10-transformation-pack.html>

Download the files and copy the theme to `~/themes` and the icons to `~/.icons`

## Set Tooltip Color of Eclipse

Per default the tooltips in eclipse are black. This is not very readable. In order to change this, do the following:

Create a `eclipse.sh` file

~~~bash
#!/bin/bash
export UBUNTU_MENUPROXY=0
export SWT_GTK3=0
/home/lubuntu/bin/eclipse/eclipse
~~~

Depending on the theme the ToolTips background color should be customized:

![Customize Look and Feel - Color](setup_virtual_box_for_development/customizeLookAndFeel.png)

Select a color like:

![Customize Look and Feel - Color](setup_virtual_box_for_development/pickAColor.png)

## Add Eclipse to Desktop

Create a `~/.local/share/applications/eclipse.desktop` file:

~~~
[Desktop Entry]
Name=Eclipse
Type=Application
Exec=/home/lubuntu/bin/eclipse/eclipse.sh
Terminal=false
Icon=/home/lubuntu/bin/eclipse/icon.xpm
Comment=Integrated Development Environment
NoDisplay=false
Categories=Development;IDE;
Name[en]=Eclipse
~~~

The `Exec` path is to the `eclipse.sh` file mentioned above.

## Change Key Shortcut F11

Eclipse binds the key F11 to "Debug the selected resource or active editor". Lubuntu binds F11 to "Toggle Fullscreen". In order to change comment out the `action` associated with F11 in the file `~/.config/openbox/lubuntu-rc.xml`

~~~
<keybind key="F11">
  <!-- <action name="ToggleFullscreen"/> -->
</keybind>
~~~

Then reconfigure the openbox window manager by executing the following shell command

~~~bash
openbox --reconfigure
~~~

see <http://askubuntu.com/questions/354664/how-do-i-actually-disable-f11-fullscreen-on-lubuntu-13-04>

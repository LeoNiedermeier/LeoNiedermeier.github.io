---
title: Setup VirtualBox for development
tags: [misc]
category: misc
summary: "Setup a VirtualBox for development with java, eclipse etc."
---

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

# References
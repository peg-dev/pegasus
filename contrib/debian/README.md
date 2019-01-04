
Debian
====================
This directory contains files used to package pegasusd/pegasus-qt
for Debian-based Linux systems. If you compile pegasusd/pegasus-qt yourself, there are some useful files here.

## pegasus: URI support ##


pegasus-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install pegasus-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your pegasusqt binary to `/usr/bin`
and the `../../share/pixmaps/pegasus128.png` to `/usr/share/pixmaps`

pegasus-qt.protocol (KDE)


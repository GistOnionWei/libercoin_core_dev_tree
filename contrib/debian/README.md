
Debian
====================
This directory contains files used to package libercoind/libercoin-qt
for Debian-based Linux systems. If you compile libercoind/libercoin-qt yourself, there are some useful files here.

## libercoin: URI support ##


libercoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install libercoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your libercoin-qt binary to `/usr/bin`
and the `../../share/pixmaps/libercoin128.png` to `/usr/share/pixmaps`

libercoin-qt.protocol (KDE)


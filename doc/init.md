Sample init scripts and service configuration for libercoind
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/libercoind.service:    systemd service unit configuration
    contrib/init/libercoind.openrc:     OpenRC compatible SysV style init script
    contrib/init/libercoind.openrcconf: OpenRC conf.d file
    contrib/init/libercoind.conf:       Upstart service configuration file
    contrib/init/libercoind.init:       CentOS compatible SysV style init script

Service User
---------------------------------

All three Linux startup configurations assume the existence of a "libercoin" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes libercoind will be set up for the current user.

Configuration
---------------------------------

At a bare minimum, libercoind requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, libercoind will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that libercoind and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If libercoind is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running libercoind without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/libercoin.conf`.

Paths
---------------------------------

### Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/libercoind`  
Configuration file:  `/etc/libercoin/libercoin.conf`  
Data directory:      `/var/lib/libercoind`  
PID file:            `/var/run/libercoind/libercoind.pid` (OpenRC and Upstart) or `/var/lib/libercoind/libercoind.pid` (systemd)  
Lock file:           `/var/lock/subsys/libercoind` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the libercoin user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
libercoin user and group.  Access to libercoin-cli and other libercoind rpc clients
can then be controlled by group membership.

### Mac OS X

Binary:              `/usr/local/bin/libercoind`  
Configuration file:  `~/Library/Application Support/Libercoin/libercoin.conf`  
Data directory:      `~/Library/Application Support/Libercoin`  
Lock file:           `~/Library/Application Support/Libercoin/.lock`  

Installing Service Configuration
-----------------------------------

### systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start libercoind` and to enable for system startup run
`systemctl enable libercoind`

### OpenRC

Rename libercoind.openrc to libercoind and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/libercoind start` and configure it to run on startup with
`rc-update add libercoind`

### Upstart (for Debian/Ubuntu based distributions)

Drop libercoind.conf in /etc/init.  Test by running `service libercoind start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

### CentOS

Copy libercoind.init to /etc/init.d/libercoind. Test by running `service libercoind start`.

Using this script, you can adjust the path and flags to the libercoind program by
setting the libercoind and FLAGS environment variables in the file
/etc/sysconfig/libercoind. You can also use the DAEMONOPTS environment variable here.

### Mac OS X

Copy org.libercoin.libercoind.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.libercoin.libercoind.plist`.

This Launch Agent will cause libercoind to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run libercoind as the current user.
You will need to modify org.libercoin.libercoind.plist if you intend to use it as a
Launch Daemon with a dedicated libercoin user.

Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.

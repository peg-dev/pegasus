# Sample init scripts and service configuration for pegasusd

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/pegasusd.service:    systemd service unit configuration
    contrib/init/pegasusd.openrc:     OpenRC compatible SysV style init script
    contrib/init/pegasusd.openrcconf: OpenRC conf.d file
    contrib/init/pegasusd.conf:       Upstart service configuration file
    contrib/init/pegasusd.init:       CentOS compatible SysV style init script

# Service User

All three startup configurations assume the existence of a "pegasus" user
and group.  They must be created before attempting to use these scripts.

# Configuration

At a bare minimum, pegasusd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, pegasusd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that pegasusd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If pegasusd is run with `-daemon` flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

```bash
bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'
```

Once you have a password in hand, set `rpcpassword=` in `/etc/pegasus/pegasus.conf`

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/pegasus.conf`.

# Paths

All three configurations assume several paths that might need to be adjusted.
```
Binary:              /usr/bin/pegasusd
Configuration file:  /etc/pegasus/pegasus.conf
Data directory:      /var/lib/pegasusd
PID file:            /var/run/pegasusd/pegasusd.pid (OpenRC and Upstart)
                     /var/lib/pegasusd/pegasusd.pid (systemd)
```
The configuration file, PID directory (if applicable) and data directory
should all be owned by the pegasus user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
pegasus user and group.  Access to pegasus-cli and other pegasusd rpc clients
can then be controlled by group membership.

# Installing Service Configuration

## systemd

Installing this .service file consists on just copying it to
`/usr/lib/systemd/system` directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run "systemctl start pegasusd" and to enable for system startup run
`systemctl enable pegasusd`

## OpenRC

Rename pegasusd.openrc to pegasusd and drop it in `/etc/init.d`.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/pegasusd start` and configure it to run on startup with
`rc-update add pegasusd`

## Upstart (for Debian/Ubuntu based distributions)

Drop pegasusd.conf in `/etc/init`.  Test by running "service pegasusd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

## CentOS

Copy pegasusd.init to `/etc/init.d/pegasusd`. Test by running "service pegasusd start".

Using this script, you can adjust the path and flags to the pegasusd program by
setting the PEGASUSD and FLAGS environment variables in the file
`/etc/sysconfig/pegasusd`. You can also use the DAEMONOPTS environment variable here.

# Auto-respawn

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.

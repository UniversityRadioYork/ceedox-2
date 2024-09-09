# Cold Starting URY’s Servers

Occasionally, it is necessary to start up all of the servers, at the same time. This is usually after a power cut.

If possible (read: if the UPS actually works), it is best to shut down the servers during a power outage to prevent any corruption of data.

This document assumes that URY is not a smoking crater, and that the FM transmitter is still powered on (it should be, the Pi is powered by a USB battery pack).

## Order of starting

Regardless of if the servers how the servers are powered off, the following order should be followed for booting them back up

1. uryfw0 - critical to allow for off-site assistance via ssh
2. stratfordbackup0
3. steve
4. ury (Thunderhorn)
5. Logger Red
6. Logger Blue
7. Selector Pi
8. dolby
9. evergiven bsod - Get Xymon back up, it make life so much easier

N.B. Some servers may have started themselves after a power cut. Login to each server in the above order to get each one working.

Please ensure each server has finished starting before beginning the next one, or at least, that all of the services or mounts required by other servers have been dealt with.

### Rational behind the boot order:

The order recommended above is most likely the quickest way to get all of the servers up and running again, and is based on the dependencies that servers have.

Practically all of them mount stratford backup0, so there is little point in having ury, bsod or dolby up without it. We also need at least one logger up before we start broadcasting again.

Jukebox (on dolby) and BAPS (in the studios) require Steve’s filestore, so there is no point having Dolby up without Steve (we might as well rely on silencio until we have Jukebox).

Broadcasting before all of the servers are restarted is probably a bad idea (other than bsod, though that does make monitoring difficult).

### Notes on starting the servers

All of the servers have the same root user password. This password will be available in a lockbox in the tx cupboard, as it is required if one of the servers boots into single user mode.

#### uryfw0

Firewall0 is our main way of connecting to the outside world. Boot it first in order to allow the rest of the Computing Team to access the rest of the network. It is one of the servers that seems to start itself, and actually does start, rather than hanging because it has either a corrupt drive or is missing a connection to another server.

\
However, upon booting the server, immediately run:

```
sudo /usr/local/fw0wall/fw0wall
```

This command actually runs the firewall. It is usually a good idea to stop anyone having access to absolutely everything (apparently).

#### Stratford

Stratford is our backup server and should start up relatively painlessly. It’s possible the pools don’t mount automatically on startup. To remedy this you can run the following as root: \
 \
`zpool import filestore`

```
zpool import musicstore
zpool import backup
```

#### Steve

Make sure the database is running after booting steve. Its a postgres database. It was working for me so idk the command to start it if it isn’t.

#### Thunderhorn

Freebsd the shit needs to mount everything on boot, like the musicstore from Steve.

Bonus fun facts: it was shitting cos we yeeted autoviz so sbeve wouldn't start nfs so blundershorn wouldn't mount the musicstore and some bollocks mount from dolby. The shit.

#### Loggers

service start loggerng

Check in /home/logger/logs/continuous on red and /logs/continuous in blue that it's actually making them

#### Dolby

/usr/local/etc/liquidsoap/scripts

startaudio.sh

#### Selector pi (tbf, this should be before dolby)

Oh it just didn't turn on. Off and on the power, startselctor.sh

#### Working on it

##### Studios

Check all channels to desk work

Check baps works

#### Switching FM

-   ssh to the fm transmitter
-   screen -ls
-   screen -r pid
-   Close running program
-   ./fm_test or recv
-   Ctl a, d

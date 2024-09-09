# Common Tasks

## Switching To/From Term Time Mode

On dolby, run fm jb to put jukebox on the FM feed. The auto-selector will deal with the news. When we go back, run fm pgm to put us back.

This should also switch the DAB feed.

## Putting FM on Local Audio Loop for Breaking Station Link

On the library PC, open the screen session on the `ury` user, `screen -r`. Quit the running script. Then run `fm_test.sh` for the local audio loop, and `fm_recv.sh` for the station feed. When going back to the station feed, dolby may have killed it whilst it didn’t have a receiver, so you’ll have to sudo `systemctl restart fm_streamer` on dolby. If out of term time, also run a `fm pgm && fm jb` to get everything to catch up.

## Restarting the Station -> Transmitter Link

Actually, just see above.

## Restarting RDS

Open [Portainer](https://docker.ury.org.uk), go to the swarm services, and scale radiotext to 0 and back up to 1.

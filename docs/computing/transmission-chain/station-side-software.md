# Software Station-Side

A lot is at [https://github.com/UniversityRadioYork/jukebox](https://github.com/UniversityRadioYork/jukebox).

## Audio Router

Once it comes out of the studio selector, _which we won’t be talking about here,_ it can get routed to FM or DAB as a program feed. They also accept a clean jukebox feed, or a clean autonews feed (which we use out of term time). DAB also accepts a fourth “alternate” input, that we could use one day, but nothing’s really set up to actually put that source out.

The audio feeds are piped into the streamer scripts in /usr/local/etc/liquidsoap/scripts. These are remotely controlled by the fm_sel Python script, running as the fm_sel service, and it lives in /opt/fm_sel. This exposes a HTTP port for control, and talks to the FFMPEG streamer scripts through ZMQ.

See more details at [https://github.com/UniversityRadioYork/fm_sel](https://github.com/UniversityRadioYork/fm_sel).

[TODO finish]

## Streaming FM

FM is streamed from /usr/local/etc/liquidsoap/scripts/fm_streamer_new.sh on dolby (this is run as systemctl fm_streamer). After routing the correct source, it is sent over an RTP feed (as raw audio data) to the PC in the library.

**Note:** this uses a custom compiled version of FFMPEG, with extra features the default insteall doesn’t have. Updating servers will happily break this, so if you need to recompile it, please see [https://gist.github.com/michael-grace/d078920d15d3d5379e53bf6366e37a2d](https://gist.github.com/michael-grace/d078920d15d3d5379e53bf6366e37a2d).

## Streaming DAB

## Radiotext FM

## DLS/Slideshow for DAB

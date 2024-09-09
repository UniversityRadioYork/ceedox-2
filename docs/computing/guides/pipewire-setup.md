# Pipewire Setup

## Or: How Studio PC Pipewire works

### Alternatively, linux audio is a mess

PulseAudio in studio blue had a Moment™ (it was getting SIGKILLed at startup, launching with -v made it still fail with no useful info -vv made it suddenly start working again), so we decided to finally test moving to pipewire

## Installing pipewire+wireplumber (copy-pasted from kory’s bash history on the computing account)

```
sudo add-apt-repository ppa:pipewire-debian/pipewire-upstream
sudo add-apt-repository ppa:pipewire-debian/wireplumber-upstream
sudo apt install gstreamer1.0-pipewire libpipewire-0.3-{0,dev,modules} libspa-0.2-{bluetooth,dev,jack,modules} pipewire{,-{audio-client-libraries,pulse,bin,jack,alsa,v4l2,libcamera,locales,tests}}
sudo apt-get install libpipewire-module-x11-bell
sudo apt-get install wireplumber{,-doc} gir1.2-wp-0.4 libwireplumber-0.4-{0,dev}
systemctl --user --now disable pulseaudio.{socket,service}
systemctl --user mask pulseaudio
systemctl --user --now disable pulseaudio.{socket,service}
systemctl --user mask pulseaudio
systemctl --now disable pulseaudio.{socket,service}
```

(Alternatively, the guide I followed: https://pipewire-debian.github.io/pipewire-debian/)

PipeWire initialisation is handled by the script `/usr/local/bin/ury-pipewire.sh`, which is run by the ury-pipewire systemd user service (`/usr/lib/systemd/user/ury-pipewire.service`). BAPS depends on `pipewire+pipewire-pulse`, `ury-pipewire` depends on `pipewire+pipewire-pulse+wireplumber` and is set to run before BAPS. ury-pipewire can probably be replaced by either a pipewire or wireplumber config, I just need to get round to it.

## Current systemd service dependencies:

![image](/assets/pipewire.png)
Colour legend:
Black = Requires
Dark blue = Requisite
Dark grey = Wants
Red = Conflicts
Green = After

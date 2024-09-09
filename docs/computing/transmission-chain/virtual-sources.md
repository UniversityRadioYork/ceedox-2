# Virtual Sources and Audio Routing

As mentioned previously, Liquidsoap is our main software that handles audio routing, selection and playout. The source code running on the audio server (currently Dolby) is [here](https///github.com/UniversityRadioYork/jukebox).

Liquidsoap and other bits generate the following stereo sources:

- Jukebox

- OB-Line (via Icecast)

- WebStudio (via [ShittyServer](https///github.com/UniversityRadioYork/WebStudio/blob/master/shittyserver.py))

- Auto news (a cron job that plays pre-mixed jingle, patches in IRN 1, selects next show source),

- Sine wave (of 1350 Hz of course)

- Off Air loop

- Campus playout (when it's alive)

Jackd is used as a intermediate routing layer. Each hardware input and output are there, as well as routes patching between different liquidsoap inputs and outputs.

Liquidsoap lives in `/usr/local/etc/liquidsoap` on Dolby. The `scripts` directory has helpful scripts to start the entire virtual audio stack (`startAudio.sh`) and make various jack routing patches.

You can see the virtual routing in [jackConnect.sh](https///github.com/UniversityRadioYork/jukebox/blob/master/scripts/jackConnect.sh).

## Hardware Interconnects

Currently URY gets audio inputs from both digital and analogue sources via a Focusrite Scarlett 18i20 (gen 1). All inputs below are stereo pairs unless otherwise noted.

### Digital Inputs

8 Channels via ADAT from Dante
2 more channels could be aquired via SPDIF

- Studio Red

- Studio Blue

- Office (Yamaha Mixer)

- Satelite IRN 1 (Mono)

- Satelite IRN 2 (Mono)

### Analogue Inputs

(Upto 8 channels)

- AM Receiver (Stereo because reciever does have two outputs)

- FM Receiver

- TX Compressor Output (to live streams/FM)

### Digital Outputs

8 channels via ADAT to Dante
2 more channels could be aquired via SPDIF

- Selector output (for studio "Online" monitoring)

- AM Receiver passthrough (Mono, for studio "AM" monitoring)

- FM Receiver passthrough (Mono due to lack of channels, office monitoring)

- OB-Line

- Webstudio/autonews mixed

### Analogue Outputs

(Upto 10 outputs)

- Selector output (for Logging "online" and final TX compressor)

- AM TX output (2x mono downmix)

- Headphones for AM monitoring (Unconfirmed if still working)

- Headphones for FM monitoring (Unconfirmed if still working)

# Equipment in the Cupboard

The equipment rack contains an uninterruptible power supply at the bottom. This is (well, not yet, will be), monitored by MuxNet.

## FM

There’s a computer (see below), which outputs audio on USB into an interface box (we call it the kermitbox). Then that audio goes into the transmitter via two ¼” jack - XLRs. The FM transmitter is the top, blue one.

**Note:** currently one of these XLRs is wired backwards, so it inverts the phase in the cable. To compensate, currently the software is outputting one of the channels with reversed phase.

The feeder run then goes to the back of the cupboard, into the utility void space, out to the roof, along the cable housing and to the antenna. From the platform looking towards Derwent/Heslington, the FM antenna is the one on the right.

### RDS

Digital data broadcast alongside the audio feed on FM, to display on the screens of radios that support it. The data is added to the broadcast signal in the FM transmitter. It comes from the computer over the USB-Serial cable, and then through the dodgy wiring adapter. This is required, because the transmitter expects a non-standard pin layout.

RDS has two flavours: static data (which includes the station name and ident code, assigned by Ofcom), and dynamic data (we only use the Radiotext feature for Now Playing data).

If the dynamic data feed fails, then either the transmitter keeps radiating the last seen data, or defaults to the default static text (“By students, for students..”). In either case, it’ll need kicking to reset it. If the By Students, For Students bit has each word capitalised, then it’s coming from our software. If only the word By is capitalised, it’s the default one set in the transmitter.

## DAB

The DAB transmitter is below the FM one. This is connected to the internet (via the computer, see Networking below) to receive the stream from the multiplex. It is monitored by MuxNet.

The RF from that goes into the filter (the black lump of metal with protruding bits) below the transmitter. This does magic. (It filters the RF).

Then, it goes along the feeders, following the same route, to the DAB antenna, on the left (closer to the STEM centre). The other feeder cable is the return feed from the GPS antenna, which is the little antenna on the same pole. This is for GPS clocking.

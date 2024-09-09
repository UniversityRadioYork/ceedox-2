# Dante

## History

As part of URY's Beat the Buzz campaign in the mid 2010's, URY was able to fund the switchover to digital for inter-room audio distribution.

Prior to this, all audio was sent analogue via jack patch bays between the studios, hub, office and stores. These patches are still present for emergency and interesting use cases.

## Network

The Dante network has been dedicated it's own IP subnet:
`10.64.160.32/28`. Sadly this has become rather full.

See [Netbox](https://netbox.ury.org.uk/ipam/ip-ranges/) for a full IP address allocation breakdown

All devices need to be patched onto the VLAN on the main URY switch to function.

Several PCs in the station have extra USB or PCIe network ports to be able to use Dante Controller to configure the Dante network, namely the Studio Guest PCs and the production PC (since they run windows at present).

## Devices

You have several Dante devices at your disposal:

-   Focusrite Rednet 2 per studio

-   Focusrite Rednet 3 in stores, used as an interface between computing virtual audio routing and hardware recorders/tuners

-   Focusrite Rednet 4 (8 channel input stagebox) in a portable rack case.

-   Focusrite Rednet AM2 (2 channel output) in the office for monitoring AM/FM/studio

-   Yamaha 01V96VCM mixing desk with Dante expansion card.

-   Behringer X32 Rack with X-Dante card

-   Studios / Production PC with Dante Via [LINK TO LICENSES]

-   [Unknown status] Ferrofish Verto 32 (replacement for Rednet 3 above)

## Clocking Info

Because Dante sends audio digitally, all Dante devices on the network must share a syncronised clock to allow data to flow between them.

Here are some facts to know about URY's specific setup:

-   All devices on the Dante network MUST operate on the same clock frequency and should all sync to a single master clock.

-   The Yamaha 01V96VCM and Dante card only support 48KHz. This enforces that the Dante network as a whole must be 48KHz.

-   The clock master should always be the Dante device in stores (Rednet 3) which connects to the Computing sound card/audio intereface (Focusrite Scarlett 18i20 version 1)

-   The Dante device in stores (Rednet 3) should take it's clock source externally from either the ADAT or word clock (BNC connector) from the Computing audio interface.

Those last 2 points require some further explanation:
Most Dante devices on the network input and output analogue signals. These are not referenced by the clock, signals coming in won't be `out of sync` with the clock of the Dante device. This makes them suitable for being clock slaves.
This is in contrast to devices such as the Rednet 3 in stores. This device inputs and outputs ADAT digital signals over fibre-optic only. These signals are referenced to the Dante master clock, the other device (the Computing audio interface, a Scarlett 18i20) needs to send and recieve data syncronised to this same clock also, otherwise crackles, clicks and dropouts occur.

The decision was made for the Scarlett to be the master clock source because this is synced to the software clock of the virtual audio processing. URY can operate a skeleton service without physical studios as long as the audio interface is connected to the audio server, without Dante. As such, the Scarlett clock is the most important and shouldn't be interfered with by Dante hardware issues/swaps.

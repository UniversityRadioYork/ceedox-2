# Equipment in the Station

## The Hub

The Hub (the cupboard behind the station front door) contains some useful pieces of the broadcast chain puzzle!

### Networking

Everything that requires networking in the station and stores goes via The Hub. There are multiple switches here:

-   URY's main switch, currently a 3COM 48 port unit. This has many VLANs accociated with different services, including a Dante VLAN/subnet.

-   A smaller 8-port overflow Dante switch.

-   An ITS managed HP switch for our university/internet uplink. This is the one with the fibre going into it.

Each room / area of the station / stores is labeled in the patchbay at the bottom. Count from left to right to find the port you are after. All ports around the station are labled consistently (and in numerical order) with the labels here.

One row of the patchbay is just a passthrough to other equipment in The Hub rack.

### The Selector (Hardware)

This is a silver 2U rack box with 4 buttons and LEDs. The buttons represent Studio 1 (Red), 2 (Blue), 3 (Jukebox) and 4 (Outside Broadcast).

This box actually supports 8 sources internally and is remote controlled via a 9 pin D-type serial port. Instructions on the protocol are printed inside the box.

It connects to the selector buttons via the patch bay with Cat5 cables. WARNING: Be very careful, these are ~9v lines with custom pinouts, they are not ethernet friendly and should be very carefully patched.

It also connects via serial (and a USB converter) to the "Selector" Raspberry Pi in the black rack unit named "Galifrey".

This box is considered the primary device in the broadcast selection chain. If it is not connected, many software-controlled source selections will not work.

### Dante

In the rack is a small POE (Power Over Ethernet) device called a Dante AVIO. This device provides two channels of digital audio as input (on one XLR connector) and two channels as output, using the AES3 digital audio format.

This is used for:

-   2 channels of input from the Satelite reciever. Channel 1 is typically IRN 1 (Sky news radio, hourly news bulletins and random sports updates) and channel 2 is IRN 2, which carries live coverage from events. Usually IRN1 is duplicated across both channels sent to the studios, while they are available as separate feeds via online streams or custom Dante routing.

-   2 channels output to the Blackmagic Design ATEM Television video mixer. This is typically "selector" output, the soonest audio tap after the virtual audio selector to reduce video delay required to sync audio and video [LINK TO VISUALISATION]. It could also be sent direct studio feeds, but this will encure significantly less delay, but won't change with selector output going to air.

# Source Selection

URY's source selection system has changed a bit over the last 10 years or so, slowly being more computing-based.

## The Selector

### In the Hub

The physical selector was once the only part of the source selection system. Located in the Hub in magestic brushed silver is the 2009 iteration of the unit.

It supports 8 sources. Originally these were physical inputs on the rear of the unit, but now lay unused. The box is now just a hardware controller for the Liquidsoap software stack.

Also on the back are:

- A serial DB9 (for the software control)

- 2 RJ45 jacks for the studio selector controls in each studio

- A mains input

On the front:

- A lock switch (Here / Studio options)

- 3 buttons (select Studio 1 (red), Studio 2 (blue), Jukebox)

- (Unused) Headphone socket for listening.

The lock switch prevents the studios from being selected from inside the studios (Here mode), as an effort to prevent selections out of term time/OBIT. Historically the Chief Engineer would lock the selector during these times.

The Firmware also allows for a software lock which takes precedence over the physical buttons. The firmware and serial comms guide is inside the box.

## In the Studios

Inside each studio there are 3 buttons, again to select Studio 1 (red), Studio 2 (blue) and Jukebox. Other sources can be selected via MyRadio/SIS.

Also, less apparent, is some further functionality, power detection. Inside the MattchBox of each studio is a relay which is powered by the studio switched power ring. This informs the selector that the studio is powered on.

This means that the selector will disable switching to a studio that is powered off and will switch back to Jukebox (Source 3) if power is lost during a show.

Lastly, the bulb power of the corresponding studio button will also illuminate the On Air lights on the hall door and in-studio stalk via a mains relay in a box near each studio door.

[MORE TECH DETAILS ABOUT THE SELECTOR BUTTONS, MATT S has pinouts for the cat5 and voltages]

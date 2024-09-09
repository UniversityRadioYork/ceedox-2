# Dolby

-   IF IT SHITS ITSELF YOU WILL GO TO **DEAD AIR** WITHOUT ANY WARNING! (except Xymon)

    -   Panic.

    -   Get back on air. Somehow. Do a TX OB if you need to.

-   Liquidsoap

    -   Choose a box to temporarily host all URY audio - preferably a Debian box because setting up liquidsoap on a FreeBSD is all the pain. I recommend Steve.

    -   You will need the music store mounted at /music, if you’re using Steve this isn’t a problem because it _is_ the musicstore

    -   Physically move the sound card USB to it, shouldn’t need any drivers

    -   Install Liquidsoap from OPAM (already installed on Steve) and JACK (might already be installed on Steve)

    -   Git clone [https://github.com/UniversityRadioYork/jukebox.git](https://github.com/UniversityRadioYork/jukebox.git)

    -   Edit the template config files - you can probably skip the Icecast parts until you’re back on air on AM, it’s more important (although you will need to comment out the appropriate lines in the liquidsoap files otherwise it will refuse to run)

    -   You may need to edit jackConnect with the appropriate device names

    -   `cd scripts && sudo ./startAudio.sh`

-   SelectorListener
    -   TODO
-   Icecast
    -   TODO

# Virtual Switching

The real audio switching these days is done in Liquidsoap, a software-defined audio processing system. This is ultimately found in [sel.liq](https://github.com/UniversityRadioYork/jukebox/blob/master/sel.liq).

The process of switching at its most complicated is:

1.  A source is selected in MyRadio via SIS (Studio Information System) [API Func](https://github.com/UniversityRadioYork/MyRadio/blob/master/src/Classes/ServiceAPI/MyRadio_Selector.php)
2.  MyRadio makes a [Telnet connection](https://github.com/UniversityRadioYork/MyRadio/blob/master/src/Classes/ServiceAPI/MyRadio_Selector.php#L164) with a command to [Selector Listener](https://github.com/UniversityRadioYork/selectorlistener), a piece of software running on a raspberry Pi connected to the serial of the physical Selector
3.  The physical selector makes a decision on if the current source is allowed to be selected (based on power and lock status) and selects it.
4.  SelectorListener writes a database record to record its change, this changes the [MyRadio API](https://github.com/UniversityRadioYork/MyRadio/blob/8977a6c8b34aa9663343c182c1d7798a63dbff9d/src/Classes/ServiceAPI/MyRadio_Selector.php#L250)/Timelord display etc (**be careful to update the configuration if the database server is changed!**)
5.  SelectorListener makes a [Telnet connection](https://github.com/UniversityRadioYork/selectorlistener/blob/master/main.c#L200) to Liquidsoap to actually perform the source selection.

Note that if you're selecting from the selector buttons, step 1 and 2 are skipped. Also note that this therefore means that the physical selector is considered the master of the selector output, so things get very borked / unresponsive if it's not present.

At a pinch, you can telnet liquidsoap directly and force a switch.

One last note is that if Liquidsoap is restarted, it will switch to a default source (typically Off-Air). The MyRadio API will likely be incorrectly reporting the selector state until the physical selector is told to change to the same source that liquidsoap is outputing, before selecting back to whatever source is required.

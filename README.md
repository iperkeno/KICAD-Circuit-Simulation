# KICAD - Circuit Simulation
Kicad 5.0 has a new and improved interface with SPICE ([ngspice](http://ngspice.sourceforge.net/ngspice-eeschema.html) specifically).



    * set up a schematic for simulation,
    * create and apply models,


SPICE uses models to describe the behavior of electronic components. KiCad implicitly assigns models to passive components, such as resistors and capacitors, however, models for semiconductor devices, such as diodes and transistors, need to be explicitly assigned.

    * Modified from LTspice standard library
    .model 2N2222 NPN (IS=1E-14 VAF=100
    + BF=200 IKF=0.3 XTB=1.5 BR=3
    + CJC=8E-12 CJE=25E-12 TR=100E-9 TF=400E-12
    + ITF=1 VTF=2 XTF=3 RB=10 RC=.3 RE=.2)

you can add the model directlu to the schematic as graphic text or save in a separate file lib.
lib file can contain several models and subcircuits.

assign the model to the component.

## Basic NGSPICE syntax

comment: *


### SPICE control statements

transient: 
```tran 12n 1m```

## Best Prctice
* For named nets, use global labels instead of local labels.
  The reason for this is that in the netlists, global identifiers will be used as-is but local labels get text prepended to the name—which makes it hard for you to remember/guess what the full identifier is.


## Run a circuit simulation
`Select Tools > Simulator` from main menu and you will see the Spice Simulator window appear.

Let’s take a look at the simulation settings. Click the Settings icon (gear) within the toolbar to see the Simulation Settings window. The Transient tab should already be selected and populated with the control statement data it obtained from the text within the schematic. Here you can see the correlation of 1u to Time step and 1m to Final time. This is the place I mentioned earlier where you can enter your SPICE control statements in a more user friendly way, but your entries will not be saved between simulations. The other tabs provide for different simulation control statements. If you click the Custom tab, you will see the exact control statement retrieved from the schematic. We don’t want to change anything here, so just click Cancel when done.

The big moment arrives. Click the green arrow button (Run/Stop Simulation) in the toolbar to run the simulation. A blank Plot1 waveform viewer will appear at the top and the simulation output will be shown at the bottom with the following contents.

## View circuit waveforms and determine certain values along the curves.
In the previous section, we determined the circuit values from the SPICE simulation output text. A simpler way to see the values is to use the KiCad waveform viewer. Since we ran a transient analysis with .tran 1u 1m, the time frame for the waveforms will cover from 0 seconds (when the circuit turned on) up to 1 ms.

To view a signal, such as a voltage or current, click the Add Signals icon in the toolbar and select a signal you want to view in the popup window. Let’s start with choosing the current flowing through resistor R1. Click on I(R1) and then the OK button. The waveform will be shown in the waveform viewer on the left and the I(R1) signal will be listed in the Signals list on the right. The current will be about 4.19 mA.

If you see a negative current through a resistor, you can change either the orientation of the resistor by 180 degrees in the schematic or use the Alternate node sequence option like we did earlier for the transistor.

To remove a signal from the viewer, double click the signal name in the Signals list.

Let’s next look at the transistor’s collector voltage by adding the V(/Vc) signal like we did earlier for I(R1). The value will be around 57.1 mV. Play around and look at some of the other signals as well.

Although we are seeing the expected values in the waveform viewer, the waveforms themselves are not very interesting. Let’s change it up a little by adding a 100 mV ripple to the input voltage. Close the Spice Simulator window. Change the value of the Vin voltage source from 5 to sin(5 100m 10k). This means we are applying a sine wave voltage with a DC offset of 5 V, an amplitude of 100 mV, and a frequency of 10 KHz.

You can also edit the SPICE model for the voltage source using the Spice Model Editor like we did earlier for the transistor. This time, however, select the Source tab instead of the Model tab. Under the Transient analysis section, select the Sinusoidal tab and you will see the sine wave source data we entered previously as a value. The other tabs provide other SPICE based voltage source types available. Model data entered here will override the Value field, but will not be visible on the schematic.

Run the simulation again and the output text should be identical to the previous simulation since the ripple voltage we applied is about the 5 V DC offset.

View the V(Vin) signal this time and we should see a sinusoidal waveform that oscillates between 4.9 and 5.1 V.

To determine a value along the sine wave, right-click on V(Vin) in the Signals list and select Show Cursor from the contextual menu. A dashed axis will appear in the waveform viewer with V(Vin) also showing up in the Cursors list. Click and hold around the origin of the axis and you can “ride” the waveform watching the Time and Voltage/Current values in the Cursors list change. Release the click when you are at an interesting point along the curve. To remove a cursor, right-click on the signal in the Signals list and this time select Hide Cursor.

Remove the V(Vin) waveform and view the I(R1) signal this time. You will see the current oscillating between 4.09 and 4.29 mA.

Again, play around and view some of the other signals.
## Web References
[]()

[Quick Guide to Using KiCad for SPICE Simulation](https://mithatkonar.com/wiki/doku.php/kicad/kicad_spice_quick_guide)(Mithat Konar)
written for KiCad 4. 

[Performing A Circuit Simulation In KiCad](https://www.woolseyworkshop.com/2019/07/01/performing-a-circuit-simulation-in-kicad/)  (July 1, 2019 by John Woolsey)

[Spice and Kicad 5.0](https://thestumbler.io/projs/modelrr/03-spice-and-kicad.html) (Chris Lott)

[]()

[]()

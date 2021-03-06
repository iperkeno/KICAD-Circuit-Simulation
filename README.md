# KICAD - Circuit Simulation
Kicad 5.0 has a new and improved interface with SPICE ([ngspice](http://ngspice.sourceforge.net/ngspice-eeschema.html) specifically).
Eeschema provides an embedded electrical circuit simulator using ngspice as the simulation engine.



## Simulation with external NGSPICE

### set up a schematic for simulation
To perform a simulation with external Ngspice is necessary to generate the net file.

1. From menu `Tool/Generate Netlist File...`
2. Select `Spice` tab;
3. Press button `Generate Netlist`
4. Select the netlist name `<net-name>.cir` and save file      

### Run External NGSPICE

     gnome-terminal -e "ngspice  absolute/path/to/file/<net-name>.cir" 

## Device Libs
Ngspice-31 reads PSPICE device libs. These are often provided by the semiconductor device manufacturers for design support. Internally ngspice translates the PSPICE syntax to ngspice before simulating. No more manual tweaking of the library description is required. 

## create and apply models,


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
in line comment: $

### SPICE control statements

transient: 
```tran 12n 1m```

```
  * wait with the following commands until simulation has finished
  set controlswait
  * save the name of the current plot into a variable
  set prevplot = $curplot
  * do an operating point evaluation
  op
  * print the data
  echo Data from $curplot
  print all
  * go back to the previous plot
  setplot $prevplot
```


## Best Practice
* For named nets, use global labels instead of local labels.
  The reason for this is that in the netlists, global identifiers will be used as-is but local labels get text prepended to the name—which makes it hard for you to remember/guess what the full identifier is.


### Netlist name precautions

Many software tools that use netlists do not accept spaces in the component names, pins, nets or other informations. Avoid using spaces in labels, or names and value fields of components or their pins to ensure maximum compatibility.

In the same way, special characters other than letters and numbers can cause problems. Note that this limitation is not related to Eeschema, but to the netlist formats that can then become untranslatable to software that uses netlist files.

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

## Simulating multi-gate devices
[reference](https://frdmtoplay.com/simulating-multi-gate-devices-in-kicad-with-ngspice/)

The 74HC00 is a quad 2-input NAND gate. KiCAD splits the IC into five discrete components: four individual gates (Units A-D) and one power unit (Unit E).

The experimental ngspice model assumes that each gate maps to one instance of the SPICE model. 
Each instance of the 74HC00 SPICE model requires five connections: two inputs, an output, VCC, and ground.

      .SUBCKT 74HC00  in1 in2 out  NVCC NVGND 

This model works well if the schematic contains a single gate. Placing one of the NAND gates and the power unit exposes five pins in eeschema and the circuit simulates properly. Note that the NAND gate and the power unit need to be part of the same IC (U1A and U1E). Multiple pairs of NAND and power units could be added to the schematic and used with the default ngspice 74HC00 model if they were treated as discrete components (U1A, U2A, U3A, etc).

Simple schematic

However, in practice, all four NAND gates of the 74HC00 are going to be used. When multiple NAND gates (U1A, U1B, etc) from the same IC are placed in the schematic the ngspice model fails. KiCAD assumes that each IC maps to one SPICE model. Placing all four NAND gates of a 74HC00 into the model exposes 14 pins and KiCAD expects a SPICE model with 14 parameters.

**ngspice Model**

The normal ngspice 74HC00 model can be expanded into a quad gate model by creating a new component, the 74HC00x4. The new component maps the original 14 pins of the 74HC00 to four discrete five pin models.

      .SUBCKT 74HC00x4 1A 1B 1Y 2A 2B 2Y GND 3Y 3A 3B 4Y 4A 4B VCC
      XU1 1A 1B 1Y VCC GND 74HC00  
      XU2 2A 2B 2Y VCC GND 74HC00  
      XU3 3A 3B 3Y VCC GND 74HC00  
      XU4 4A 4B 4Y VCC GND 74HC00  
      .ENDS

**TI Model**

TI provides detailed 74HC models which can also be modified to work with KiCAD by merging four individual gates into one larger chip.

      .SUBCKT SN74HC00 1A 1B 1Y 2A 2B 2Y AGND 3Y 3A 3B 4Y 4A 4B VCC
      XU1 1Y 1A 1B VCC AGND LOGIC_GATE_2PIN_OD_LVC_2i_NAND_PP_CMOS  
      XU2 2Y 2A 2B VCC AGND LOGIC_GATE_2PIN_OD_LVC_2i_NAND_PP_CMOS  
      XU3 3Y 3A 3B VCC AGND LOGIC_GATE_2PIN_OD_LVC_2i_NAND_PP_CMOS  
      XU4 4Y 4A 4B VCC AGND LOGIC_GATE_2PIN_OD_LVC_2i_NAND_PP_CMOS  
      .ENDS

### Hierarchical Sheets

SPICE models also work within KiCAD's hierarchical sheets allowing complex logic gates to be built and reused in schematics. A XOR gate can be simulated implementing it with four NAND gates using one quad 74HC00 SPICE model.



## Web References
[Tutorial: ngspice simulation in KiCad/Eeschema](http://ngspice.sourceforge.net/ngspice-eeschema.html#digi)

[Quick Guide to Using KiCad for SPICE Simulation](https://mithatkonar.com/wiki/doku.php/kicad/kicad_spice_quick_guide)(Mithat Konar)
written for KiCad 4. 

[Performing A Circuit Simulation In KiCad](https://www.woolseyworkshop.com/2019/07/01/performing-a-circuit-simulation-in-kicad/)  (July 1, 2019 by John Woolsey)

[Spice and Kicad 5.0](https://thestumbler.io/projs/modelrr/03-spice-and-kicad.html) (Chris Lott)

[]()

[]()

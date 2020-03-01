# KICAD - Circuit Simulation
Kicad 5.0 has a new and improved interface with SPICE ([ngspice](http://ngspice.sourceforge.net/ngspice-eeschema.html) specifically).

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

transient: 
```tran 12n 1m```

## Best Prctice
* For named nets, use global labels instead of local labels.
  The reason for this is that in the netlists, global identifiers will be used as-is but local labels get text prepended to the name—which makes it hard for you to remember/guess what the full identifier is.



## Web References
[]()

[Quick Guide to Using KiCad for SPICE Simulation](https://mithatkonar.com/wiki/doku.php/kicad/kicad_spice_quick_guide)(Mithat Konar)
written for KiCad 4. 

[Performing A Circuit Simulation In KiCad](https://www.woolseyworkshop.com/2019/07/01/performing-a-circuit-simulation-in-kicad/)  (July 1, 2019 by John Woolsey)

[Spice and Kicad 5.0](https://thestumbler.io/projs/modelrr/03-spice-and-kicad.html) (Chris Lott)

[]()

[]()

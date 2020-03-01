# KICAD - Circuit Simulation
Kicad 5.0 has a new and improved interface with SPICE ([ngspice](http://ngspice.sourceforge.net/ngspice-eeschema.html) specifically).



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

[]()

[]()

[]()

[]()

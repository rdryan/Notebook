## USB Study Note 2 ##

### High-speed (480 Mb/s) Driver Characteristics ###

* differential characteristic impedance (Z0) of 90 Ohm +/- 15%.
* a common mode impedance (Zcm) of 30 Ohm +/- 30%.
* the high-speed capable transceiver opereating in full-speed or low-speed mode must have a driver impedance (Zhsdrv) of 45 Ohm +/- 10%.
* On downstream facing ports. Rpd resistors (15 kOhm +/- 5%) must be connected from D+ and D- to ground.
* a high-speed capable transceiver transitions to high-speed mode, the **high-speed idle state** is achieved by **driving SE0** with the _low-/full-speed_ drivers at each end of the link. D+ pull-up resistor disconnect in upstream facing transceiver.
* one line at **(17.78 mA * 22.5 Ohm = ) 400 mV** voltage, the other line remains at ground voltage. and vice versa. (_To signal a J, the current is directed into the D+ line, and to signal a K, the current is directed into the D- line_).


### Full-speed (12 Mb/s) Driver Characteristics ###

* differential Z0: 90 Ohm +/- 15%.
* common mode Zcm: 30 Ohm +/- 30%.
* full-speed (not part of high-speed caable transceiver) driver impedance Zdrv: 28 Ohm ~ 44 Ohm.
* full-speed (part of high-speed capable transceiver) driver impedance Zhsdrv: 40.5 Ohm ~ 49.5 Ohm


### Low-speed (1.5 Mb/s) Driver Characteristics ###

* a single-ended capacitance of no less than 200 pF and no more than 450 pF on the D+/D- lines.













 
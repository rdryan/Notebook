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


### Reset ###

* SE0 more then 10 ms.

### Suspend ###

* Idle state on lines for more then 3.0 ms.

### Resume ###

* Device is resumed when any non-idle signaling is received.


2015-09-11

### Data Encoding/Decoding

* The USB employs NRZI data encoding when tranmitting packets.    
(**NRZI** stands for _Non-Return to Zero, Inverted_)    

* Zero's in the NRZI data stream are represented by transitions while 1s are represented by the absence of a transition.

* NRZI **encoder** and **decoder** design can refer to [this artical](http://www.oguchi-rd.com/technology/nrzi.pdf).

* Transitions in the data stream permit the decoder to maintain synchronization with the incoming data, thereby eliminating 
the need for a separate clock signals.

	**Note** however that a long string of consecutive 1s results in no transitions, causing the receiver to eventually lose synchronization. The solution is to employ **_bit stuffing_**.


### Bit Stuffing

* Bit stuffing forces transitions into the NRZI data stream in the event that six consecutive 1s are transmitted.    
* The transmitter of NRZI data is responsible for inserting a 0 (stuffed bit) into the NRZI stream.   
* The receiver must be designed to expect an automatic transmition following six consecutive 1s and discard the 0 bit that immediately follows the sixth consecutive 1.
* Bit stuffing is enabled beginning with the Sync Pattern.
* The data "one" that ends the Sync Pattern is counted as the first one in a sequence.
* Bit stuffing by the transmitter is always enforced, except during high-speed EOP.












 
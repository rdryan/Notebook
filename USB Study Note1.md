## USB Study Note 1 ##

### USB transfer -> transaction -> packets ###

* One USB transfer contains many transactions:
> SETUP Transaction.   
> DATA(IN/OUT) Transaction.  
> ACK Transaction.

* One transaction contains many packets:
> SETUP Packet    
> DATA Packet    
> ACK Packet    


### USB Packet format ###

* Token packets format     
PID + ADDR + ENDPOINT + CRC5    
(the PID is token pid)

* DATA packet format    
PID + DATA Payload + CRC16   
(the PID is data pid)

* Handshake packet format     
PID

There are four token pid:   

	OUT Token, IN Token, SOF Token, SETUP Token

There are four data pid:

	DATA0, DATA1, DATA2, MDATA

There are for Handshake pid:

	ACK, NAK, STALL, NYET

There are other special pid:

	PRE, ERR, SPLIT, PING


### Sync Field ###
All packets begin with a SYNC field. 8 bits for full/low_speed, 32 bits for hight-speed.

> **SYNC Pattern**
>> 3 KJ pairs + 2K for **full/low-speed**    
>> 15 KJ pairs + 2K for **high-speed**    

### EOP (End Of Packet) ###

> **EOP Pattern**   
>> 2 SE0 for **full/low-speed**  

>> For **high-speed** packets other than SOF's, the transmitted EOP delimiter is required to be an NRZ byte of 01111111 without bit stuffing.    

>> For **high-speed** SOF's, the transmitted EOP delimiter is required to be 5 NRZ bytes without bit stuffing, consisting of 01111111 11111111 11111111 11111111 11111111.   
(**The extra EOP length is of no significance to a receiver, it is used for disconnect detection**.)


2015-09-08

### Connect and Disconnect Signaling ###

**Disconnect**    
   	
* low-/full-speed, the pull-down resistors present at host or hub will cause both D+ and D- to be pulled low (like an SE0). A disconnect condition is indicated if the host or hub is not driving the data lines and an SE0 persists on a downstream facing port for more then TDDIS.

* high-speed, by sensing the doubling in differential signal amplitude across the D+ and D- lines that can occur when the device terminations are removed. 

	* Vdiff >= 625mV: actiate the Disconnection Enveloper Detector 	
	* Vdiff <= 525mV: never activate the Disconnection Enveloper Detector
	* _To assure that this additive effect occurs and is of sufficient duration to be detected, the EOP at the end of a high-speed SOF is lengthened to a continuous string of 40 bits without any transition._


**Connect**    
A connect condition will be detected when the hub detects that on of the data lines is pulled above its VIH threshold for more than TDCNN.



### Byte/Bit Ordering ###
Bits are sent LSB first, MSB last.   
Multiple byte fieldsare in little-endian order. LSB to MSB.


### CRCs ###

**Token CRCS**

The five-bit CRC:

	G(X)=X^5+X^2+1

The polynomial is 00101B. five-bit residual is 01100B.


**Data CRCS**

The 16-bit CRC:

	G(X)=X^16+X^15+X^2+1

The polynomial is 10000000000001101B. 16-bit  residual is 1000000000001101B.


2015-09-10

-----

### USB Connector Termination Data ###

| Contact Number | Signal Name | Typical Wriring Color|   
|----------------|-------------|----------------------|   
|       1        |  VBUS       |    Red               |   
|       2        |  D-         |    White             |   
|       3        |  D+         |    Green             |   
|       4        |  GND        |    Black             |   
|     Shell      |  Shield     |    Drain Wire        |   

-----




	












 
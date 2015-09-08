## USB Study Note 1 ##

### USB transfer -> transaction -> packets ###

+ One USB transfer contains many transactions:
> SETUP Transaction.   
> DATA(IN/OUT) Transaction.  
> ACK Transaction.

+ One transaction contains many packets:
> SETUP Packet    
> DATA Packet    
> ACK Packet    


### USB Packet format ###

* Token packets format *    
PID + ADDR + ENDPOINT + CRC5    
(the PID is token pid)

* DATA packet format *   
PID + DATA Payload + CRC16   
(the PID is data pid)

* Handshake packet format *    
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

> ** EOP Pattern**   
>> 2 SE0 for **full/low-speed**  

>> For **high-speed** packets other than SOF's, the transmitted EOP delimiter is required to be an NRZ byte of 01111111 without bit stuffing.    

>> For **high-speed** SOF's, the transmitted EOP delimiter is required to be 5 NRZ bytes without bit stuffing, consisting of 01111111 11111111 11111111 11111111 11111111.   
(**The extra EOP length is of no significance to a receiver, it is used for disconnect detection**.)


2015-09-08











 
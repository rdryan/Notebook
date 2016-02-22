## USB LTSSM ##

### Link Training and Status Status Machine (LTSSM) ###
Link Training and Status State Machine (LTSSM) is a state machine defined for link connectivity and the link power management. 

LTSSM consists of **12** different link states that can be characterized based on their functionalities:  

1. Four operational link states: **U0**, **U1**, **U2**, and **U3**  
2. Four link states: **Rx.Detect**, **Polling**, **Recovery**, and **Hot Reset**  
3. Two other link states: **Loopback** and **Compliance**   
4. Two more link states: **eSS.Inactive** and **eSS.Disabled**  


**Rx.Detect**: Represents the initial power-on link state where a port is attempting to determine determine if its Enhanced SuperSpeed link partner is present. Upon detecting the presence of an Enhanced SuperSpeed link partner, the link training process will be started.  

**Polling**: is a link state that is defined for the two link partners to have their Enhanced SuperSpeed transmitters and receivers trained, synchronized, and ready for packet transfer.  

**Recovery**: is a link state defined for retraining the link when the two link partners exit from a low power link state, or when a link partner has detected that the link is not operating in U0 properly and the link needs to be retrained, or when a link partner decides to changed the mode of link operation.

**Hot Reset**: is a state defined to allow a downstream port to reset its upstream port.

 

# Protocol Layer #


## Packet Types ##
There are four packet types: 

* Link Management Packets (LMP)  
* Transaction Packets (TP)
* Data Packets (DP)
* Isochronous Timestamp Packets (ITP)

All packets consist of a 14-byte header, followed by a 2-byte Link Control Word at the end of the packet (16 bytes total).   
All headers have a Type field that is used by the receiving entity to determine how to process the packet.   
All hearders include a 2-byte CRC (CRC-16).  


## Packet Formats ##

**Type** Field is a 5-bit field:

	00000b:	Link Management Packet
	00100b: Transaction Packet
	01000b: Data Packet Header
	01100b: Isochronous Timestamp Packet
	All other value are Reserved

All header packets have a **16-bit CRC** field.

**Link Control Word**  

	CRC-5, DF, DL, Hub Depth, Reserved, Header Seq

## Link Management Packet (LMP) ##

Packets that have the **Type** filed set to *Link Management Packet* are referred to as LMPs. These packets are used to manage a single link. They carry no addressing information and as such are not routable.

**Subtype Field**

	0000b: Reserved
	0001b: Set Link Function
	0010b: U2 Inactivity Timeout
	0011b: Vendor Device Test
	0100b: Port Capability
	0101b: Port Configuration
	0110b: Port Configuration Response
	All other value are Reserved

## Transaction Packet (TP) ##

Transaction Packets (TPs) traverse the direct path between the host and a device. TPs are used to control data flow and manage the end-to-end conection.

**Subtype Field**

	0000b: Reserved
	0001b: ACK
	0010b: NRDY
	0011b: ERDY
	0100b: STATUS
	0101b: STALL
	0110b: DEV_NOTIFICATION
	0111b: PING
	1000b: PING_RESPONSE
	1001b - 1111b Reserved


## Data Packet (DP) ##

This packet can be sent by either the host or a device. All data packets are comprised of a Data Packet Header and a Data Packet Payload.


## Isochronous Timestamp Packet (ITP) ##

The Isochronous Timestamp Packet (ITP) shall be multicast on all links in U0 that have completed Port Configuration.
      
ITPs are used to deliver timestamps from the host to all active devices. ITPs carry no addressing or routing information and are multicast by hubs to all of their downstream ports with links in the U0 state and that have completed Port Configureation. A device shall not respond to an ITP. ITPs are used to provide host timing information to devices for synchronization. 

**Note that:**
 
* Any device or hub may receive an ITP. 
* The host shall transmit an ITP on a root port link if and only if the link is already in U0. Only the host shall initiate an ITP transmission. 
* The host shall not bring a root port link to U0 for the purpose of transmitting an ITP. 
* The host shall transmitt an ITP in every bus interval within tTimestramp Window from a but interval boundary if the root  port link is in U0. 
* The host shall begin transmitting ITPs within tIsochronous TimestampStart from when the host root port's link enters U0 from the polling state. 
* An ITP may be transmitted in between packets in a burst. If a device receives an ITP with the delayed flag (DL) set in the link control word, the timestamp value may be significantly inaccurate and may be ignored by the device.




-----

[Go to USB Study Note2](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note2.md)   
[Go to USB Study Note3](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note3.md)   


	












 
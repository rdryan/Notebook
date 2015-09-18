## USB Study Note 3 ##


### UTMI vs. UTMI+

* The UTMI spec is used only for USB2.0 peripherals, it can not be used to develop USB2.0 host or OTG peripherals.
* The UTMI+ spec is to extend the UTMI spec to standardize the interface for USB2.0 hosts and USB2.0 OTG peripherals.


### UTMI Serial Interface Signals

* **FSDATAEXT**   
    USB 1.1 Transmit Data. This controller signal sets the USB to either a J or K state. This signal is valid only if FSXCVROWNER is set to 1'b1, TXENABLEN is set to 1'b0, and FSSE0EXT is set to 1'b0.

* **FSSE0EXT**    
    USB 1.0 SE0 Generation. This controller signal sets the USB to an SE0 state. This signal is valid only if FSXCVROWNER is set to 1'b1 and TXENABLEN is set to 1'b0.

* **TXENABLEN**    
    USB 1.0 Data Enable. This controller signal enables the FSDATAEXT and FSSE0EXT inputs. TXENABLEN is valid only when the FSXCVROWNER signal is set to 1'b1.

* **FSXCVROWNER**  
    UTMI+/Serial Interface Select. This controller signal enables the UTMI+ or serial interface.

* **FSLSRCV**   
    Differential Data Receive Indicator. This controller signal indicates the state of differential data on the USB during normal operation.

* **FSVPLUS**   
    Signle-Ended D+ Indicator. This controller signal indicates the state of the D+ line during normal operation.

* **FSVMINUS**   
    Signle-Ended D- Indicator. This controller signal indicates the state of the D- line during normal operation.


### UTMI+ Parallel Interface Signals

* DATAIN, DATAINH, TXREADY, TXVALID, TXVALIDH

* DATAOUT, DATAOUTH, RXERROR, RXACTIVE, RXVALID, RXVALIDH


2015-09-14


------

### USB Transfer Types and Scheduling

There are 4 transfer types have been defined by the USB spec:

* Interrupt transfer
* Bulk transfer
* Isochronous transfer
* Control transfer


### Max Package Size (in Bytes)

|Type           | Low Speed | Full Speed    | High Speed |        
|---------------|-----------|---------------|------------|     
|Control        |   8       |   8/16/32/64  |   64       |    
|Isochronous    |   NA      |   1023        |   1024     |     
|Interrupt      |   8       |   64          |   1024     |    
|Bulk           |   NA      |   64          |   512      |    


### Communication Pipes

USB spec classifies a communications pipe as either a stream or a message pipe:

* Streaming pipes    
    Iso, Interrupt and bulk endpoint. USB imposes no particular format for data. Data delivered to or from these endpoint may have specific structures or formats (class or vendor-specific).

* Message pipes    
    Control endpoint. has specific strcture defined by USB. Communication requires a specific structure and sequence, and the data patterns sent to the endpoint define requests or commands that are issued to a device. These requests specify that the device must take some action.

**_Note_**   

The communication targeting endpoint zero (the default control endpoint) that every device must implement. it is **bidirectional** to allow messages to be passed in both directions.

The other transfer pipes are always **unidirectional**. So two endpoints needed, one for reads and one for writes.

------

### Special Considerations for Isochronous Transfers

Support for isochronous data movement between the host and a device is one of the system capabilities supported by the USB. Delivering isochronous data reliably over the USB requires careful attention to detail.

* USB Clock Model
* USB (micro)frame Clock-to-function Clock Synchronization Option
* SOF Tracking
* Data Prebuffering
* Error Handling
* Buffering for Rate Matching

(_Above topic can be found in usb2.0 spec page65 ~ page83._)


------

### NAK Limiting *via* Ping Flow Control

PING and NYET are only used in high-speed.

Following an **OUT transaction** that has been delivered to a bulk or control endpoint. If the endpoint return a NAK handshake for inability processing the data it received. Much of the bandwidth is wasted.

The *PING* protocol solves this problem by providing a way for the host to ask and for the device to answer.

(All bulk and control endpoints at high-speed must support the PING protocol. However, the setup stage of a control transfer **must** always return ACK)

The endpoint either responds to the *PING* with a *NAK* or an *ACK* handshake.

* A *NAK* handshake indicates that the endpoint does not have space for a *wMaxPacketSize* data payload. The host controller will retry the *PING* at some future time to query the endpoint again.

* An *ACK* handshake indicates the endpoint has space for a *wMaxPacketSize* data payload. The host controller must generate an *OUT* transaction with a DATA phase as the next transaction to the endpoint.
	>* If the endpoint responds to the OUT/DATA transaction with an **ACK** handshake, this means the endpoint accepted the data successfully and has room for **another** *wMaxPacketSize* data payload.
	
	>* If the endpoint responds to the OUT/DATA transaction with a **NYET** handshake, this means that the endpoint accepted the data **but** does not have room for another *wMaxPacketSize* data payload.
 

### Bulk Transactions

* When host **receives** bulk data, it issues an **IN token**. The function endpoint responds by returning either a data packet or, should it be unable to return data, a _NAK_ or _STALL_ handshake.
	>* _NAK_ indicates indicates that the function is temporarily unable to return data
	
	>* _STALL_ indicates that the endpoint is permanently halted and requires USB System Software intervention.
	
	>* If host receives a valid data, it responds with an _ACK_ handshake.
	
	>* If host _detects an error_ while receiving data, it returns **no handshake** packet to the function.

* When host **transmits** bulk data, it issues an **OUT token packet** followed by a data packet (or PING token in high-speed mode). If the data is received without error by the function, it will return _ACK_ or _NAK_ or _STALL_ or no handshake is returned.
	>* _ACK_ indicates that data packet was received without errors, and informs host that it may send next packet in the sequence.

	>* _NAK_ indicates that data packet was received without erros **but** that host should **resend** the data because the function was in a temporary condition preventing it from accepting that data (e.g., buffer full).

	>* If the endpoint was halted, _STALL_ is returned to indicate that the host should not retry the transmission because there is an error condition on the function.

	>* If the data packet was received _with a CRC or bit stuff error_, **no handshake** is returned.
	

### Control Transfers

Control transfers have **Two** or **Three** transaction stages:  
  
* Setup and Status.
* Setup, Data and Status.

**SETUP transactions** are similar in format to an OUT except the PID. A SETUP always uses a **_DATA0_** PID for the _data field_ of the SETUP transaction.

* SETUP (Token) + DATA0 + **ACK**    (_transaction succeed_)
* SETUP (Token) + DATA0	with **no handshake**    (_transaction corrupted_)

(_The function receiving a SETUP must accept the SETUP data and respond with ACK. If the data is corrupted, discard the data and return **no handshake**._)

The _**Data stage**_ consists one or more IN or OUT transactions, follows the rules as bulk transfers. All the transactions must in the **same** direction. All the data carry the maximum packet size, except the last one.

The _**Status stage**_ transactions follow the same protocol sequence as **bulk transactions**. It also include PING protocol (high-speed). A Status stage is delineated by a change in direction of data flow from the previous stage and always use a **DATA1 PID**.

**Status Stage Responses**

| Status Response      | Control Write Transfer   | Control Read Transfer        |    
|----------------------|--------------------------|------------------------------|    
|                      | (sent during data phase) | (sent during hanshake phase) |      
| Function completes   | Zero-length data packet  | ACK handshake                |    
| Function has an error| STALL handshake          | STALL handshake              |    
| Function is busy     | NAK handshake            | NAK handshake                |    


### Interrupt Transactions

Interrupt transactions consist of **IN** or **OUT** transfers.

* when receipt of an IN token, a function may return data, NAK, or STALL. The host, in response to receipt of data packet, issues ACK handshake or returns no handshake.
* When receipt of an OUT token and DATA, a function may return ACK, NAK, STALL or no handshake return.
* Initialized to use **DATA0** PID, behaves the same as Bulk transactions.

### Isochronous Tranactions
Isochronous transactions have a **token** and **data** phase, but no handshake phase. It do not support a handshake phase or retry capbility.

The host issuse either an **IN** or an **OUT** token followed by the **data** phase.

* A full-speed device only send **DATA0** PIDs in data packet.
* A high-speed device with at most 1 transaction per microframe must only send **DATA0** PIDs.
* A high-speed device with high-bandwith endpoints must be able to accept and/or send **DATA0**, **DATA1**, **DATA2**, or **MDATA** PIDs in data packets.
* Full-speed isochronous transactions do not support toggle sequencing.
* High-speed isochronous transactions with a single transaction per microframe do not support toggle sequencing.
* High bandwidth, high-speed isochronous transctions support data PID sequencing.

2015-09-18


------


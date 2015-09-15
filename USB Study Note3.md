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





 

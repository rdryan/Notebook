## USB Study Note 3 ##


### UTMI vs. UTMI+

* The UTMI spec is used only for USB2.0 peripherals, it can not be used to develop USB2.0 host or OTG peripherals.
* The UTMI+ spec is to extend the UTMI spec to standardize the interface for USB2.0 hosts and USB2.0 OTG peripherals.


### UTMI Serial Interface Signals

* FSDATAEXT   
	USB 1.1 Transmit Data. This controller signal sets the USB to either a J or K state. This signal is valid only if FSXCVROWNER is set to 1'b1, TXENABLEN is set to 1'b0, and FSSE0EXT is set to 1'b0.

* FSSE0EXT    
	USB 1.0 SE0 Generation. This controller signal sets the USB to an SE0 state. This signal is valid only if FSXCVROWNER is set to 1'b1 and TXENABLEN is set to 1'b0.

* TXENABLEN    
	USB 1.0 Data Enable. This controller signal enables the FSDATAEXT and FSSE0EXT inputs. TXENABLEN is valid only when the FSXCVROWNER signal is set to 1'b1.

* FSXCVROWNER
	UTMI+/Serial Interface Select. This controller signal enables the UTMI+ or serial interface.

* FSLSRCV
	Differential Data Receive Indicator. This controller signal indicates the state of differential data on the USB during normal operation.








 
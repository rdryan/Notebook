## USB Type-C ##

### Three Critical Challenges of USB Type-C Implementation ###
1. The first challenge is supporting two SuperSpeed datapaths matching one or the other orientation of the USB Type-C connector.

2. The second challenge is partitioning the SoC and system design to support multiple variants of product-specific hardware for USB Type-C. Designers must navigate the specification's requirements for precision analog circuitry plus high voltage/high current switches that can be external discrete componets, external dedicated USB Type-C controller chips, integrated in a power management IC, or integrated in the SoC.

3. The third challenge is partitioning additional USB Type-C management software that can execute on the main process, internal microcontroller, microcontroller in a power management IC, or external dedicated USB Type-C chip.

### Solution: ###
for 1:  
> use external datapath switch  
> use two PHY or two port  
> use internal, on-die switches  
> use USB-C PHY

 



-----

[Go to USB Study Note2](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note2.md)   
[Go to USB Study Note3](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note3.md)   


	












 
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

 



-----

[Go to USB Study Note2](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note2.md)   
[Go to USB Study Note3](https://github.com/rdryan/Notebook/blob/master/USB%20Study%20Note3.md)   


	












 
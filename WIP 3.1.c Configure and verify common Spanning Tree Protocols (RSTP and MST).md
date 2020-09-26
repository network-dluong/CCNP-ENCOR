## Spanning Tree Protocol (STP)  
* Enables switches to become aware of other switches through the advertisement and receipt of bridge protocol data units (BPDUs)  
* Builds Layer 2 loop-free topology by blocking traffic on redundant ports  
* 802.1D Port States:  
  * **Disabled** - shut down  
  * **Blocking** - switch port is enabled but not forwarding any traffic to ensure no loops, and does not modify the MAC address table  
  * **Listening** - switch port transitioned from blocking state and now sends or receives BPDUs, but cannot forward any other network traffic  
  * **Learning** - switch port modifies MAC address table with any network traffic it receives, but does not forward any other network traffic besides BPDUs  
  * **Forwarding** - switch port can forward all network traffic and updates MAC address table  
  * **Broken** - switch has detected a configuration or operation problem on a port, and port discards packets as problem exists  
  
* 802.1D Port Types:  


## EtherChannel Bundle  
* Physical links can be aggregated into a logical link (port channel) called member interfaces  
  * Advantage - reduction in topology changes when member link line protocol goes up or down  
  * Disadvantage – no health integrity check. If physical medium degrades and keeps line protocol in an up state, it will reflect that link as viable for transferring data, which may not be accurate and would result in sporadic loss  

## Port Aggregation Protocol (PAgP)  
* Cisco proprietary, advertises message with multicast MAC address 0100:0CCC:CCCC and protocol code 0x0104  
  * **Auto**:  
    * Interface does not initiate an EtherChannel to be established and does not transmit PAgP packets  
    * If PAgP packets are received from remote switch, the interface responds and establishes PAgP adjacency  
    * If both devices are PAgP auto, an adjacency does not form  
  * **Desirable**:  
    * Interface tries to establish an EtherChannel and transmits PAgP packets  
    * Active PAgP interfaces can establish PAgP adjacency only if remote interface is configured to auto or desirable  
* PAgP ports operate in silent mode by default, which allows port to establish an EtherChannel with a device that is not PAgP capable and rarely sends packets  
* To display additional information about PAgP neighbor:  
> **show pagp neighbor**  
* To show PAgP counters:  
> **show pagp counters**  
> **clear pagp counters**  

## Link Aggregation Control Protocol (LACP)  
* Open industry standard, advertises messages with multicast MAC address 0180:C200:0002  
  * **Passive**:  
    * Interface does not initiate EtherChannel to be established and does not transmit LACP packets  
    * If LACP packet is received from remote switch, the interface responds and establishes LACP adjacency  
    * If both devices are LACP passive, an adjacency does not form  
  * **Active**:  
    * Interfaces tries to establish an EtherChannel and transmit LACP packets  
    * Active LACP interfaces can establish adjacency only if remote interface is configured to active or passive  
* To display additional information about LACP neighbor:  
> **show lacp neighbor [detail]**  
* To display local LACP system ID:  
> **show lacp (system-id)**  
* LACP counters – incremental. Common problems could be related to a physical link, or it might have to do with an incomplete or incompatible configuration  
  * To show LACP counters:  
> **show lacp counters**  

## EtherChannel Configuration  
* Configure range of interfaces:  
> **interface range (*int-range-id*)**  
* **Static EtherChannel**:  
> **channel-group (*etherchannel-id*) mode on**  
* **LACP EtherChannel**:  
> **channel-group (*etherchannel-id*) mode {active | passive}**  
* **PAgP EtherChannel**:  
> **channel-group (*etherchannel-id*) mode {auto | desirable | non-silent}**  
* **Non-silent** – requires port to receive PAgP packets before adding it to the EtherChannel, recommended when connecting PAgP-compliant switches together  
* To verify Port-Channel status:  
> **show etherchannel summary**  
* To view logical interface:  
> **show interface port-channel (*port-channel-id*)**  

* **LACP Fast**:  
  * Link can be identified and removed in 3 seconds compared to 90 seconds from LACP standard  
> **lacp rate fast**  

* **Minimum number of port-channel member interfaces**:  
  * Does not need to be configured on both devices to work properly, but is recommended  
  > **lacp min-links (*min-links*)**  
  
* **Maximum number of port-channel member interfaces**:  
  * Ensure that the active member interface count proceeds with power of two to accomodate load-balancing hashes
  > **lacp max-bundle (*max-links*)**  
  
* **LACP System Priority**:  
  * Identifies which switch is the master switch for a port channel  
  * Master switch is responsible for choosing which member interfaces are active when there are more than the maximum number of member interfaces associated with a port-channel interface  
  * Switch with lower system priority is preferred  
  > **lacp system-priority (*priority*)**  
  
* **LACP Interface Priority**:  
  * Enables master switch to choose which member interfaces are active when there are more member interfaces than the maximum number of member interfaces for a port-channel  
  * Port with lower port priority is preferred  
  > **lacp port-priority (*priority*)**  
  
  
## Troubleshooting EtherChannel Bundles  
* Following configuration settings must match on member interfaces:  
  * Port type - every port in the interface must be configured to be Layer 2 switch port or Layer 3 routed port  
  * Port mode - all Layer 2 port channels must be configured as either access port or trunk port (cannot mix)  
  * Native VLAN - member interfaces on Layer 2 trunk port channel muct be configured with same native VLAN  
  > **switchport trunk native vlan (*vlan-id*)**  
  * Allowed VLAN - member interfaces on Layer 2 trunk port channel must be configured to support the same VLANs  
  > **switchport trunk allowed (*vlan-id*)**  
  * Speed - duplex must be the same  
  * MTU - all Layer 3


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
  * **Root port (RP)** - network port that connects to the root bridge or an upstream switch (only one RP per VLAN)  
  * **Designated port (DP)** - network port that receives and forwards BPDU frames to other switches, and provides connectivity to downstream devices and switches (only one active DP on a link)  
  * **Blocking port** - network port that is not forwarding traffic because of STP calculations  
  
* STP Terminology:  
  * **Root bridge** - all ports in forwarding state and are DPs  
  * **Bridge protocol data unit (BPDU)** - network packets used for network switches to identify a hierarchy and notify changes in topology (destination MAC address 01:80:c2:00:00:00)  
    * Configuration BPDU - used to identify root bridge, RPs, DPs, and blocking ports  
    * Topology change notification (TCN) BPDU - used to communicate changes in Layer 2 topology to other switches  
  * **Root path cost** - combined cost for a specific path toward root switch  
  * **System priority** - 4-bit value that indicates preference for a switch to be root bridge (default value of 32,768)  
  * **System ID extension** - 12-bit value that indicates VLAN that the BPDU correlates to. System priority + system ID extension (VLAN ID) = part of switch's identification of root bridge  
  * **Root bridge identifier** - combination of root bridge system MAC address, system ID extension (VLAN ID), and system priority of root bridge  
  * **Local bridge identifer** - combination of local switch's bridge system MAC address, system ID extension (VLAN ID), and system priority of root bridge  
  * **Max age** - maximum length of time that passes before a bridge port saves its BPDU information (dfeault value is 20 seconds but can be changed)  
  > **spanning-tree vlan (*vlan-id*) max-age (*maxage*)**  
  * **Hello time** - time that a BPDU is advertised out of port (default value is 2 seconds but can be changed from 1 to 10)  
  > **spanning-tree vlan (*vlan-id*) hello-time (*1-10*)**  
  * **Forward delay** - amount of time that port stays in listening and learning state (default value is 15 seconds but can be changed from 15 to 30)  
  > **spanning-tree vlan (*vlan-id*) forward-time (*15-30*)**  
  
  
  ## STP Path Cost  
  * **Short mode** - 16-bit value with reference value of 20 Gbps (default)  
  * **Long mode** - 32-bit value with reference value of 20 Tbps  
  > **spanning-tree pathcost method long**  
  
  | **Link Speed** | **Short-Mode STP Cost** | **Long-Mode STP Cost** |
  | --- | --- | --- |
  | 10 Mbps | 100 | 2.000.000 |
  | 100 Mbps | 19 | 200,000 |
  | 1 Gbps | 4 | 20,000 |
  | 10 Gbps | 2 | 2,000 |
  | 20 Gbps | 1 | 1,000 |
  | 100 Gbps | 1 | 200 |
  | 1 Tbps | 1 | 20 |
  | 10 Tbps | 1 | 2 |
  
  
## Root Bridge Election  
* If neighbor's configuration BPDU is inferioer, the switch ignores that BPDU  
* If neighbor's configuration BPDU is preferred, the switch updates its BPDUs to include the new root bridge identifier with a new root path cost that correlates to the total path cost to reach the new root bridge  
* STP deems a switch more preferable if priority in the bridge identifier is lower than other switch's  
* If priority is the same, then switch prefers BPDU with lower system MAC address  
> **show spanning-tree root**  


## Locating Root Ports  
* The Root Port (RP) is selected through the following steps (the next step is used in the event of a tie):  
1. The interface associated to lowest path cost is more preferred  
2. The interface associated to the lowest system priority of the advertising switch is preferred  
3. The interface associated to the lowest system MAC address of the advertising switch is preferred  
4. When multiple links are associated to the same switch, the lowest port priority from the advertising switch is preferred  
5. When multiple links are associated to the same switch, the lower port number from the advertising switch is preferred  


## Locating Blocked Designated Switch Ports  
* To prevent forwarding loop, the following steps will calculate which ports should be blocked between two non-root switches:  
1. The interface is a DP and must not be considered an RP  
2. The switch with lower path cost to root bridge forwards packets, and the one with the higher path cost blocks packets. In case of a tie, go to next step  
3. The system priority of the local switch is compared to the system priority of remote switch. Local port is moved to blocking state if remote system priority is lower than that of local switch. In case of a tie, go to next step  
4. The system MAC address of local switch is compared to system priority of remote switch. Local designated port is moved to blocking state if remote system MAC address is lower than that of local switch  
* To locate a port's STP state:  
> **show spanning-tree** **[vlan (*vlan id*)]**  

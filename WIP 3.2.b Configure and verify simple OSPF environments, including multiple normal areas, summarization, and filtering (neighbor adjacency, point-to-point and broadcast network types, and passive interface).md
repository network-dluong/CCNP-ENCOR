## OSPF Configuration  
* To define OSPF process:  
> **router ospf (*process-id*)**  
* Network statement uses wildcard mask:  
> **network (*ip-address*) (*wildcard-mask*) area (*area-id*)**  
* Interface-specific configuration:  
> **ip ospf (*process-id*) area (*area-id*) [secondaries none]**  
* To statically set the Router ID:  
> **router-id (*router-id*)**  
* To restart OSPF process on router so it can use new RID:  
> **clear ip ospf process**  
* To configure passive interface:  
> **passive (*int-id*)**  
> **no passive (*int-id*)**  
* To configure all interfaces as passive:  
> **passive interface default**  
* To verify OSPF:  
> **show ip ospf interface [brief | (*int-id*)]**  
* To verify OSPF Neighbors:  
> **show ip ospf neighbor [detail]**  
* To verify OSPF routes:  
> **show ip route ospf**  
* To advertise default rote into OSPF domain:  
> **default-information originate** **[always]** **[metric (*metric-value*)]** **[metric-type (*type-value*)]**  
* **always** - advertises default route even if it does not exist in RIB


## Requirements for Neighbor Adjacency  
* RIDs must be unique between devices  
* Interfaces must share a common subnet  
* MTUs on the interfaces must match. OSPF does not support fragmentation  
* Area ID must match for the segment  
* DR enablement must match for the segment  
* OSPF hello and dead timers must match for the segment  
* Authentication type and credentials must match for the segment  
* Area type flags must match for the segment  


## Common OSPF Optimizations  
* Link Costs = Reference Bandwidth / Interface Bandwidth  
 * Default reference bandwidth is 100 Mbps  
 
| **Interface Type** | **OSPF Cost** |
| --- | --- |
| T1 | 64 |
| Ethernet | 10 |
| FastEthernet | 1 |
| GigabitEthernet | 1 |
| 10 GigabitEthernet | 1 |  
* To change reference bandwidth (needs to be changed on all OSPF routers):  
> **auto-cost reference-bandwidth (*bandwidth-mbps*)**  
> **ip ospf cost (*1-65535*)**  
* To modify Hello Timer, Dead Interval Timer, and verify Timers:  
> **ip ospf hello-interval (*1-65535*)**  
> **ip ospf dead-interval (*1-65535*)**  
> **show ip ospf interface**  


## OSPF Network Types  
* **Broadcast**:  
 * Hello: 10, Wait: 40, Dead: 40  
 * Non-Broadcast - Hello: 30, Wait: 120, Dead: 120
 * OSPF network type is set to broadcast by default for Ethernet interfaces  
 * DR is required because multiple nodes can exist on a segment and LSA flooding needs to be controlled  
 * To override configured setting and statically set an interface as OSPF broadcast network type:  
 > **ip ospf network broadcast**  
 
* **Point-to-Point Networks**:  
 * Hello: 10, Wait: 40, Dead: 40  
 * Point-to-Multipoint - Hello: 30, Wait: 120, Dead: 120
 * Network circuit that only allows two devices to communicate and do not use ARP  
 * OSPF is set to P2P by default for serial interfaces (HDLC or PPP encapsulation), GRE tunnels, and P2P Frame Relay sub-interfaces  
 * Forms OSPF adjacency quickly because DR election is bypassed and there is no wait timer  
 * To set interface as OSPF P2P:  
 > **ip ospf network point-to-point  
 
 * **Loopback Networks**:  
 * Enabled by default and can only be used on loopback interfaces  
 * IP address is always advertised with /32 prefix length even if configured otherwise  
  
  
 ## OSPF Areas  
 * Logical grouping of routers (or router interfaces) that is set at the interface level  
 * Area 0 is the backbone  
 * **Area Border Routers (ABR)** are OSPF routers connected to Area 0 and another OSPF Area  
   * Responsible for advertising routes from one area and injecting them into another OSPF area  
   * Will always need to participate in Area 0  
 
 * **OSPF Route Types**:  
  * Intra-area routes - network routes that are learned from other OSPF routers  
  * Inter-area routes - network routes that are learned from other OSPF routers from a different area using an ABR  
  
  
  ## Link-State Announcements  
  

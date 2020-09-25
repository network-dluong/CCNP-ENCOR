## Distance Vector Algorithms  
* **Distance** – route metric to reach network  
* **Vector** – interface or direction to reach network  
* When best paths are determined, they are installed into routing table and advertised to each neighbor router  
* Does not have a complete map of the whole network; neighbor router knows how to reach the destination network and how far the neighbor router is from destination network  
* Does not account for link speeds or other factors  
* Requires less CPU and memory, and can run on low-end routers  
* Road sign at intersection indicating destination is 2 miles to the west; drivers blindly trust and follow this information without knowing if there is a better way  


## Enhanced Distance Vector Algorithms  
* EIGRP advertises network information to neighbors with enhanced features:  
  * Offers rapid convergence time for changes in network topology  
  * Sends updates when there is change in network. Does not send full routing table updates in a periodic fashion like distance vector protocols  
  * Uses hellos and forms neighbor relationships like link-state protocols  
  * Uses bandwidth, delay, reliability, load, and maximum transmission unit (MTU) size instead of hop count  
  * Has option to load balance traffic across equal or unequal-cost paths  


## Link-State Algorithms  
* Advertises link state and link metric for each of its connected links and directly connected routers to every router in network  
* OSPF advertisements are link-state advertisements (LSAs), and IS-IS use link-state packets (LSPs)  
  * When router receives advertisement from neighbor, it stores the information in link-state database (LSDB) and advertises the information to each neighbor  
  * Allows routers in network to have synchronized and identical map of network  
  * Every router runs Dijkstra shortest path first (SPF) algorithm to calculate best shortest loop-free paths  
* Requires more CPU and memory than distance vector vectors, but less prone to routing loops  
* Compared to GPS navigation system, which has a complete map and can make the best decision on which way is the shortest and best path to destination  


## Path Vector Algorithm  
* BGP, like distance vector protocol but instead of looking at the distance to determine the best loop-free path, it looks at various BGP path attributes  
 * Autonomous system path (AS_Path), multi-exit discriminator (MED), origin, next hop, local preference, atomic aggregate, and aggregator  
 * Keeps record of each AS that the advertisement traverses  
 

## Path Selection  
* Router identifies path a packet should take by looking at prefix length in the FIB  
* FIB is programmed through the routing table (RIB)  
* RIB is composed of routes presented from the routing protocol processes  
 * **Prefix length** – represents number of leading binary bits in subnet mask that are in the on position  
 * **Administrative distance** – rating of trustworthiness of routing information source. If router learns about a route to a destination from more than one protocol with all routes having same prefix length, then AD is compared  
 * **Metrics** – unit of measure used by routing protocol in best-path calculation  


| **Routing Protocol** | **Default Administrative Distance** |
|	 --- 	|	 --- 	|
| Connected | 0 |
| Static | 1 |
| EIGRP summary route | 5 |
|External BGP (eBGP) | 20 |
|EIGRP (Internal) | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP (external) | 170 |
| Internal BGP (iBGP) | 200 |


## EIGRP
| **Term** | **Definition** |
| --- | --- |
| Successor Route | The route with the lowest path metric to reach a destination |
| Successor | The first next-hop router for the successor route |
| Feasible Distance (FD) | The metric value for the lowest-metric path to reach a destination |
| Reported Distance (RD) | The distance reported by a router to reach a prefix. The reported distance value is the feasible distance for the advertising router |
| Feasibility Condition | For route to be considered a backup route, the RD received for that route must be less than the FD calculated locally. This logic guarantees a loop-free path |
| Feasible Successor | A route that satisfies the feasibility condition and is maintained as a backup route. It ensures that the backup route is loop-free |  

* EIGRP contains a topology table which is different from a true distance vector routing table. The table contains:
 * Network prefix
 * EIGRP neighbors that have advertised that prefix
 * Metrics from each neighbor
 * Values used for calculating the metric
* Default EIGRP hello timer is 5 seconds (60 seconds for T1 or lower)
* Default hold time value is 3 timex hello timer interval


## EIGRP Neighbors  
| **Type** | **Packet Name** | **Function** |
| --- | --- | --- |
| 1 | Hello | Used for discovery of EIGRP neighbors and for detecting when a neighbor is no longer available |
| 2 | Request | Used to get specific information from one or more neighbors |
| 3 | Update | Used to transmit routing and reachability information with other EIGRP neighbors |
| 4 | Query | Sent out to search for another path during convergence |
| 5 | Reply | Sent in response to a query packet |  


## EIGRP Load Balancing  
* Variance value - feasible distance (FD) multiplied by EIGRP variance multiplier  
* Variance multiplier - divide feasible successor metric by the successor route metric (whole number, round up any remainder)  


## EIGRP Convergence  
* When EIGRP detects lost successor, the feasible successor becomes the new successor route  
* If a feasible successor is not available for a prefix, DUAL must perform a new route calculation (route state changes from passive (P) to active (A))  
* Router receiving a query packet:  
  * Might reply to query that the router does not have a route to the prefix  
  * If query did not come from successor for that route, it detects the delay set for infinity but ignore it because it did not come from successor  
 if query came from successor for the route, the receiving router detects the delay set for infinity, sets prefix as active, and sends query packet to all downstream EIGRP neigbors  
 
 
## EIGRP Route Summarization  
* Summarizes network prefixes on an interface basis, summary aggregate is configured for the inteface  
* Prefixes within summary aggregate are suppressed, and summary aggregate prefix is advertised in lieu of the original prefixes  
* Summary aggregate prefix is not advertised until a prefix matches it  
* Summarization creates a query boundary and shrinks the routing table and the query domain when a route goes active during convergence  


## OSPF  
* provides scalability for the routing table by using multiple OSPF areas within routing domain  
* Area 0 is the backbone, which all other areas must connect  
* Non-backbone areas advertise routes into backbone, and backbone then advertises routes into other non-backbone areas  
* All routers in the same area has identical area LSDBs  
* OSPF process numbers are locally significant and do not have to match among routers  

| **Type** | **Packet Name** | **Functional Overview** |
| --- | --- | --- |
| 1 | Hello | For discovering and maintaining neighbors. Packets are sent periodically on all OSPF interfaces |
| 2 | Database Description (DBD or DDP) | For summarizing database contents. Packets are exchanged when an OSPF adjacency is first formed, and are used to desribe contents of LSDB |
| 3 | Link-State Request (LSR) | For database downloads. When router thinks that part of its LSDB is stale, it may request a portion of neighbor's database |
| 4 | Link-State Update (LSU) | For database updates. This is an explicit LSA for a specific network link and sent in direct response to an LSR |
| 5 | Link-State Ack | For flooding acknowledgements. Sent in response to the flooding of LSAs |  
* Router ID - 32-bit number that uniquely identifies an OSPF router, must be unique for each process in a domain  
* Neighbor states - Down, Attempt, Init, 2-Way, ExStart, Exchange, Loading, Full  


## OSPF Designated Router and Backup Designated Router  
 * Number of adjacencies: n(n - 1)/2  
 * Designated Router (DR):  
   * Router on the broadcast segment  
   * Reduces number of OSPF adjacencies by forming full OSPF adjacency with DR only  
   * Responsible for flooding updates to all OSPF routers on segment as updates occur  
 * Back Designated Router (BDR):  
   * Becomes new DR when original DR fails  
   * BDR also forms full OSPF adjacencies with OSPF routers on segment  
   * DROTHER routers do not establish full adjacency with other DROTHER routers
1. All OSPF routers (DR, BDR, DROTHER) on segment form full OSPF adjacencies with DR and BDR  
2. As OSPF router learns of a new route, it sends updated LSA to ALLDRouters address, which only DR and BDR receive and process  
3. DR sends unicast acknowledgement to router that sent the initial LSA update  
4. DR floods LSA to all routers on segment via the ALLSPFRouters address  


## OSPF Configuration  
* to define OSPF process:  
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
> **default-information originate [always] **[metric (*metric-value*)]** **[metric-type (*type-value*)]**  
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

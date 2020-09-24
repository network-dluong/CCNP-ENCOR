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


## EIGRP Neighbors  
| **Type** | **Packet Name** | **Function** |
| --- | --- | --- |
| 1 | Hello | Used for discovery of EIGRP neighbors and for detecting when a neighbor is no longer available |
| 2 | Request | Used to get specific information from one or more neighbors |
| 3 | Update | Used to transmit routing and reachability information with other EIGRP neighbors |
| 4 | Query | Sent out to search for another path during convergence |
| 5 | Reply | Sent in response to a query packet |  


## Path Metric Calculation  

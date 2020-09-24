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
•	Advertises link state and link metric for each of its connected links and directly connected routers to every router in network  
•	OSPF advertisements are link-state advertisements (LSAs), and IS-IS use link-state packets (LSPs)  
  * When router receives advertisement from neighbor, it stores the information in link-state database (LSDB) and advertises the information to each neighbor  
  * Allows routers in network to have synchronized and identical map of network  
  * Every router runs Dijkstra shortest path first (SPF) algorithm to calculate best shortest loop-free paths  
* Requires more CPU and memory than distance vector vectors, but less prone to routing loops  
* Compared to GPS navigation system, which has a complete map and can make the best decision on which way is the shortest and best path to destination  




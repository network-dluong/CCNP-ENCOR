## Border Gateway Protocol (BGP)  
* **Autonomous System (AS)**:  
  * A collection of routers under a single organization's control, using one or more IGPs and common metrics to route packets within the AS (IGP is not required in AS)  
  * ASNs 64,512 - 65,535 are private in the 16-bit range, ASNs 4,200,000,000 - 4,294,967,294 are private in the 32-bit range  
  * IANA requires the following items when requesting a public ASN:  
     * Proof of a publicly allocated network range  
     * Proof that Internet connectivity is provided through multiple connections  
     * Need for a unique routing policy from providers  

* **Path Attributes**:  
* Well-known mandatory - must be included with every prefix advertisement  
* Well-known discretionary - may or may not be included with a prefix advertisement  
* Optional transitive - do not have to be recognized by all BGP implementations. Can be set to be transitive and stay with the route advertisement from AS to AS  
* Optional non-transitive - cannot be shared from AS to AS  

* **Loop Prevention**:  
  * AS_Path is a well-known mandatory attribute and includes a complete list of all ASNs that the prefix advertisement has traversed from its source AS  
  * AS_Path is used as a loop-prevention mechanism  
  * IF a BGP router receives a prefix advertisement with its AS listed in the AS_Path, it discard the prefix because the router thinks it forms a loop  
  
* **Address Families**:  
  * Every address family maintains a separate database and configuration for each protocol (address family + sub-address family) in BGP  
  * Allows for routing policy in one address family to be different from another address family  

* **Inter-Router Communication**:  
  * BGP neighbors do not use hello packets. Neighbors are defined by IP address  
  * Uses TCP port 179  
  * BGP neighbors in the same network use ARP table to lcate the IP address of the peer  
  * BGP is similar to a control plane routing protocol or as an application because it allows for the exchange of routes with a peer that is multiple hops away  
  * BGP routers do not have to be in the data plane (path) to exchange prefixes, but all routers need to know all the routes that will be forwarded through them  

* **Internal BGP (iBGP)**:  
  * 

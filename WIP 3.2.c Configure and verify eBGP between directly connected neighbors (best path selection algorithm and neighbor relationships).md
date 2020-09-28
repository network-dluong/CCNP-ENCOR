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
  * Sessions established with an iBGP router in the same AS or participating in the same BGP confederation  
  * Prefixes are assigned an AD of 200 upon installation in the router's FIB  
  * Redistributing BGP table into an IGP is not a viable solution:  
    * Scalability - IGPs cannot scale to the level of routes of the Internet, which has over 780,000 IPv4 network prefixes  
    * Custom routing - link-state protocols and distance vector protocols use metric as the primary method for route selection. IGP always use this routing pattern for path selection. BGP uses multiple steps to identify best path and allows for BGP path attributes to manipulate the path for a specific prefix (NLRI)  
    * Path attributes - All BGP path attributes cannot be maintained within IGP protocols. Only BGP is capable of maintaining the path attribute as the prefix advertised from one edge of the AS to the other  
    * SPs provide transit connectivity. Enterprise organizations are consumers and should not privode transit connectivity between ASs across the Internet  

* **External BGP (eBGP)**:  
  * Peerings are the core component of BGP on the Internet. It involves the exchange of network prefixes between ASs  
  * TTL on eBGP packets is set to 1 by default (iBGP packets is set to 255, which allows for multi-hop sessions). They drop in transit if a mult-hop BGP session is attempted  
  * Advertising router modifies BGP next-hop address to the IP address sourcing the BGP connection  
  * Advertising router prepends its ASN to existing AS_Path variable  
  * Receiving router verifies the AS_Path variable does not contain an ASN that matches the local routers. BGP discard NLRI if it fails the loop prevention check  
  
* **BGP Messages**:  

| **Type** | **Name** | **Functional Overview** |
| --- | --- | --- |
| 1 | OPEN | Sets up and establishes BGP adjacency |
| 2 | UPDATE | Advertises, updates, or withdraws routes |
| 3 | NOTIFICATION | Indicates an error condition to a BGP neighbor |
| 4 | KEEPALIVE | Ensures that BGP neighbors are still alive |

* **BGP Neighbor States**:  
  * Idle - BGP tries to initiate a TCP connection to the peer. If an error causes it to go back to Idle state for a second time, the ConnectRetryTimer is set to 60 seconds and must go to zero before connection can be initiated again. Every consecutive failed attempt doubles the timer  
  * Connect - BGP initiates TCP connection. If initiation is completed, the prcoess reset the Timer and sends Open message to the neighbor, then changes to OpenSent state. If Timer depletes before it is complete, a new TCP connection is attempted, the Timer is reset, and state is moved to Active. The neighbor with the higher IP address manages the connection, using a dynamic source port and destination port 179  
  > **show tcp brief**  
  * Active - BGP starts new three-way handshake. If connection is established, an Open message is sent, hold timer is set to 4 minutes, and state moves to OpenSent. If it fails, state moves back to Connect state, and Timer is reset  
  * OpenSent - BGP versions much match. The source IP address must match IP address configured for the neighbor. The AS number must match what is configured for the neighbor. BGP RIDs must be unique. Security parameters must be set appropriately  
  * OpenConfirm - BGP waits for KEEPALIVE or NOTIFICATION message. If KEEPALIVE received, it moves to Established. If hold timer expires, a stop event occurs, or if NOTIFICATION received, state goes to Idle  
  * Established - BGP session is established. BGP neighbors exchange routes using UPDATE messages. Hold timer is reset when UPDATE and KEEPALIVE is received. If hold timer expires, an error is detected, and BGP moves neighbor back to Idle  
  

## Basic BGP Configuration  


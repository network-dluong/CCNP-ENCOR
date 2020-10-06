# Border Gateway Protocol (BGP)  
* **Autonomous System (AS)**:  
  * A collection of routers under a single organization's control, using one or more IGPs and common metrics to route packets within the AS (IGP is not required in AS)  
  * ASNs 64,512 - 65,535 are private in the 16-bit range, ASNs 4,200,000,000 - 4,294,967,294 are private in the 32-bit range  
  * IANA requires the following items when requesting a public ASN:  
     * Proof of a publicly allocated network range  
     * Proof that Internet connectivity is provided through multiple connections  
     * Need for a unique routing policy from providers  

* **Path Attributes**:  
  * **Well-known mandatory** - must be included with every prefix advertisement  
  * **Well-known discretionary** - may or may not be included with a prefix advertisement  
  * **Optional transitive** - do not have to be recognized by all BGP implementations. Can be set to be transitive and stay with the route advertisement from AS to AS  
  * **Optional non-transitive** - cannot be shared from AS to AS  

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
  * **Idle** - BGP tries to initiate a TCP connection to the peer. If an error causes it to go back to Idle state for a second time, the ConnectRetryTimer is set to 60 seconds and must go to zero before connection can be initiated again. Every consecutive failed attempt doubles the timer  
  * **Connect** - BGP initiates TCP connection. If initiation is completed, the prcoess reset the Timer and sends Open message to the neighbor, then changes to OpenSent state. If Timer depletes before it is complete, a new TCP connection is attempted, the Timer is reset, and state is moved to Active. The neighbor with the higher IP address manages the connection, using a dynamic source port and destination port 179  
  > **show tcp brief**  
  * **Active** - BGP starts new three-way handshake. If connection is established, an Open message is sent, hold timer is set to 4 minutes, and state moves to OpenSent. If it fails, state moves back to Connect state, and Timer is reset  
  * **OpenSent** - BGP versions much match. The source IP address must match IP address configured for the neighbor. The AS number must match what is configured for the neighbor. BGP RIDs must be unique. Security parameters must be set appropriately  
  * **OpenConfirm** - BGP waits for KEEPALIVE or NOTIFICATION message. If KEEPALIVE received, it moves to Established. If hold timer expires, a stop event occurs, or if NOTIFICATION received, state goes to Idle  
  * **Established** - BGP session is established. BGP neighbors exchange routes using UPDATE messages. Hold timer is reset when UPDATE and KEEPALIVE is received. If hold timer expires, an error is detected, and BGP moves neighbor back to Idle  
  

## Basic BGP Configuration  
* **BGP session parameters** - provides settings that involve establishing communication to the remote BGP neighbor (settings include ASN of BGP peer, authentication, and keepalive timers)  
* **Address family initialization** - initialized under BGP router configuration mode. Network advertisements and summarization occur within the address family  
* **Activate the address family on the BGP peer** - one address family for a neighbor must be activated in order for a session to initiate. The router's IP address is added to neighbor table, and BGP attempts to establish a BGP session or accepts a BGP session initiated from peer router  
1. Initialize BGP routing process with global command:  
> **router bgp (*as-number*)**  
2. (Optional) Statically define BGP router ID (RID). The dynamic RID allocation logic uses highest IP address of the any *up* loopback interfaces. If there is no *up* loopback interface, then the highest IP address of any active *up* interfaces becomes the RID. Any IPv4 address can be used, including those not configured on the router. When RID changes, all BGP sessions reset and need to be reestablished:  
> **bgp router-id (*router-id*)**  
3. Identify BGP neighbor's IP address and ASN. When a BGP packet is received, the router correlates the source IP address of the packet to the IP address configured to that neighbor. If BGP packet source does not match an entry in neighbor table, the packet cannot associate to neighbor and is discarded:  
> **neighbor (*ip-address*) remote-as (*as-number*)**  
* (Optional) IPv4 address family is activated by default. The following steps are optional. To disable the automatic activation:  
> **no bgp default ip4-unicast**  
4. Initialize address family with BGP router. Examples of *afi* values are IPv4 and IPv6, and *safi* values are unicast and multicast:  
> **address-family (*afi*) (*safi*)**  
5. Activate address family for BGP neighbor:  
> **neighbor (*ip-address*) activate**  

* **Verification of BGP Sessions**:  
  * To show IPv4 BGP unicast summary and other essential peeriong information:  
  > **show bgp (*afi*) (*safi*) summary**  
  > **show bgp (*afi*) (*safi*) neighbors (*ip-address*)**  
  
* **Prefix Advertisement**:  
  * LOC-RIB table - BGP network statements identify specific network prefixes to be installed into BGP table  
  * Connected network - next-hop BGP attribute is set to 0.0.0.0, BGP origin attribute is set to i (IGP), and BGP weight is set to 32,768  
  * Static route or routing protocol - next-hop BGP attribute is set to next-hop IP address in the RIB, BGP origin attribute is set to i (IGP), BGP weight is set to 32,768, and MED is set to the IGP metric  
  * To advertise IPv4 networks (optional **route-map** provides method of setting specific BGP PAs when the prefix installs into Loc-RIB table):  
  > **network (*network*) mask (*subnet-mask*) [route-map (*route-map-name*)]**  
  * All routes in Loc-RIB table use the following process for advertisement to BGP peers:  
 1. Pass a validity check. Veryify that NRLI is valid and next-hop address is resolvable in the global RIB. If NRLI fails, it remains but does not process further  
 2. Process outbound neighbor route policies. If a route was not denied by outbound policies after processing, the route is maintained in Adj-RIB-Out table for later reference  
 3. Advertise NLRI to BGP peers. If NLRI's next-hop PA is 0.0.0.0, then next-hop address is changed to the IP address of BGP session  
  
* **Receiving and Viewing Routes**:  
  * Adj-RIB-In - contains NLRIs in original form (before inbound route policies are processed). Table is purged after all route policies are processed to save memory  
  * Loc-RIB - contains all NLRIs that originated locally or received from other BGP peers. After NLRIs pass validity and next-hop reachability check, the BGP best-path algorithm selects best NLRI for a specific prefix. LOC-RIB table is used for presenting routes to the IP routing table  
  * Adj-RIB-Out - contains NLRIs after outbound route policies have been processed 
  * To display contents of BGP database on router:  
  > **show bgp (*afi*) (*safi*)**  
  * BGP performs the following route processing steps:  
 1. Store route in Adj-RIB-In table in original state and apply the inbound route policy based on neighbor on which the route was received  
 2. Update Loc_RIB with latest entry, and the Adj-RIB-In table is cleared to save memory  
 3. Pass validity check to verify that the route is valid and next-hop address is resolvable in global RIB. If the route fails, it remains in Loc-RIB table but is not processed further  
 4. Identify BGP best path and pass only the best path and its pat attributes to step 5  
 5. Install best-path route into global RIB, process outbound route policy, store non-discarded routes in Adj-RIB-Out table, and advertise to BGP peers  

  * To display all the paths for a specific route and BGP path attributes:  
  > **show bgp (*afi*) (*safi*) (*network*)**  
  * To display contents of Adj-RIB-Out table for a neighbor:  
  > **show bgp (*afi*) (*safi*) neighbor (*ip-address*) advertised routes**
  * To verify the exchange of NLRIs between nodes:  
  > **show bgp ipv4 unicast summary**  
  * To display BGP routes in global RIB:  
  > **show ip route bgp**  
  
## Route Summarization  
* Conserves router resources and accelerates best-path calculation by reducing size of table  
* Provides stability by hiding route flaps from downstream routers, reducing routing churn  
* **Static** - create a static route to Null0 for summary network prefix and then advertise the prefix with a **network** statement. Downfall is that the summary route is always advertised, even if networks are not available  
* **Dynamic** - Configure an aggregation network prefix. When routes match the aggregate network prefix enter the BGP table, then the aggregate prefix created. The originating router sets next hop to Null0 as a discard route for the aggregated prefix for loop prevention  
* **Aggregate Address**:  
  * To enable dynamic route summarization:  
  > **aggregate-address (*network*) (*subnet-mask*)** **[summary-only]** **[as-set]**  
  * **aggregate-address** command advertises the aggregated route in addition to the original component network prefixes  
  * **summary-only** suppresses component network prefixes in the summarized network range  
* **Atomic Aggregate**:  
  * Acts like new BGP routes with shorter prefix length  
  * Indicates that a loss of path information has occurred  
* **Route Aggregation with AS_SET**:  
  * **as-set** - only counts as one hop, even if multiple ASs are listed  
  
## Multiprotocol BGP for IPv6  
* MP-BGP enables BGP to carry NLRI for multiple protocols (IPv4, IPv6, and MPLS L3VPNs)  
* All the same underlying IPv4 path vector routing protocol features and rules apply to IPv6  
* **IPv6 Configuration**:  
  * Routers with only IPv6 addressing must statically define the BGP RID to allow sessions to form  
  * Unique global unicast addressing is recommended for BGP peering to avoid operational complexity  
  * To disable IPv4 unicast (enabled by default):  
  > **no bgp default ipv4-unicast**  
  * To display IPv6 information, summary, and path attributes:  
  > **show bgp ipv6 unicast neighbors (*ip-address*) [detail]**  
  > **show bgp ipv6 unicast summary**  
  > **show bgp ipv6 unicast (*prefix/prefix-length*)**  
  
* **IPv6 Summarization**:  
  * Same process for IPv6 applies to IPv6 except the configuration:  
  > **aggregate-address (*prefix/prefix-length*) [summary-only]** **[as-set]**  
  

# Advanced BGP  
## BGP Multihoming  
* Provides redundancy by adding a second circuit and establishing a second BGP session across the peering link  
* Adding more SPs means traffic can select an optimal path between devices due to the BGP best-path algorithm  
* **Scenario 1** - R1 connects to R2 with the same SP. This accounts for link failures. Failure on either router or within SP1's network results in a network failure  
* **Scenario 2** - R1 connects to R2 and R3 with the same SP. This account for link failures. Failure on R1 or within SP1's network resultes in a network failure  
* **Scenario 3** - R1 connects to R2 and R3 with different SPs. This accounts for link failures and failures in either SP's network, and it can optimize routing traffic. Faulre on R1 results in a network failure  
* **Scenario 4** - R1 and R2 form iBGP session together. R3 connects to SP1, and R4 connects to SP2. This accounts for link failures and failures in either SP's network, and it can optimize routing traffic  

* **Internet Transit Routing**:  
  * If an enterprise uses BGP to connect with more than one SP, it runs the risk of its AS becoming a transit AS  
  * Transit routing can be avoided by applying outbound BGP route policies that only allow for local BGP routes to be advertised to other ASs  

* **Branch Transit Routing**:  
  * Proper network design takes traffic patterns into account to prevent suboptimal or routing loops  
  * When network is properly designed, traffic between sites use the preferred SP network in both directions, which simplifies troubleshooting when the traffic flow is symmetric (same path in both directions) versus asymmetric (different path for each direction)  
  * Path is *deterministic* when the flow between sites is pre-determined and predictable  
  * Unplanned transit connectivity can give the following issues:  
    * Transit router's circuit can become oversaturated because they were sized for that site's traffic and not the traffic crossing through them  
    * Routing patterns can become unpredictable and nondeterministic  


## Conditional Matching  
### Standard ACLs:  
1. Define ACL:
> **ip access-list standard {*acl-number* | *acl-name*}**  
2. Configure specific ACE entry:  
> **[*sequence*] {permit | deny} (*source*) (*source-wildcard*)**  
  
### Extended ACLs:  
1. Define ACL:  
> **ip access-list extended {*acl-number* | *acl-name*}**  
2. Configure specific ACE entry:  
> **[*sequence*] {permit | deny} (*protocol*) (*source*) (*source-wildcard*) (*destination*) (*destination-wildcard*)**  

### BGP Network Selection  

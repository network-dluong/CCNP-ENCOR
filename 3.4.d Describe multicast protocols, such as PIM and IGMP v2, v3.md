## Multicast Protocols  
* Multicast traffic provides one-to-many communication, where only one data packet is sent on a link as needed and then replicated between links as the data forks on a network device along the multicast distribution tree (MDT)  
* Data packets – stream, special destination IP address – group address  
* Recipient devices of multicast stream – receivers  


## Multicast Addressing  
* IP Class D 224.0.0.0/4 (224.0.0.0 – 239.255.255.255)  
  * Local network control block (224.0.0.0/24) – used for protocol control traffic that is not forwarded out a broadcast domain  
  * Internetwork control block (224.0.1.0/24) – used for protocol control traffic that may be forwarded through the Internet  
  * Source Specific Multicast (SSM) block (232.0.0.0/8) – default ranged used my SSM, which is a PIM extension. SSM forwards traffic to receivers from only those multicast sources for which receivers have explicitly expressed interest, one-to-many  
  * GLOP block (233.0.0.0/8) – globally scoped statistically assigned addresses  
  * Administratively scoped block (239.0.0.0/8) – limited to a local group or organization  

| **IP Multicast Address** | **Description** |  
| --- | --- |  
| 224.0.0.0 | Base address (reserved) |  
| 224.0.0.1 | All hosts in this subnet (all-hosts group) |  
| 224.0.0.2 | All routers in this subnet |  
| 224.0.0.5 | All OSPF routers (AllSPFrouters) |  
| 224.0.0.6 | All OSPF DRs (AllDRouters) |  
| 224.0.0.9 | All RIPv2 routers |  
| 224.0.0.10 | All EIGRP routers |  
| 224.0.0.13 | All PIM routers |  
| 224.0.0.18 | VRRP |  
| 224.0.0.22 | IGMPv3 |  
| 224.0.0.102 | HSRPv2 and GLBP |  
| 224.0.1.1 | NTP |  
| 224.0.1.39 | Cisco-RP-Announce (Auto-RP) |  
| 224.0.1.40 | Cisco-RP-Discovery (Auto-RP) |  


## Layer 2 Multicast Addresses  
* First 24 bits of multicast MAC address start with 01:00:5E  
  * Low-order bit of first byte is the *individual/group bit* (I/G) bit, or *unicast/multicast* bit  
  * 32 (2^5) multicast IP addresses that are not universally unique and could correspond to a single MAC address  
  
  
## Internet Group Management Protocol (IGMP)  
* Protocol that receivers use to join multicast groups and start receiving traffic from those groups  
* Must be supported by receivers and the router interfaces facing the receivers  


## IGMPv2  
* Type – describes five different types of IGMP messages used by routers and receivers:  
  * Version 2 membership report (0x16) – IGMP join. Used by receivers to join multicast group or respond to local router’s membership query message  
  * Version 1 membership report (0x12) – used by receivers for backwards compatibility with IGMPv1  
  * Version 2 leave group (0x17) – used by receivers to indicate they want to stop receiving multicast traffic for a group they joined  
  * General membership query (0x11) – periodically sent to all-hosts group address 224.0.0.1 to see if there are any receivers in the attached subnet. Sets group address field to 0.0.0.0  
  * Group specific query (0x11) – sent in response to a leave group message to group address. Group address is the destination IP address of the IP packet and the group address field  
  * Max response time – set only in general and group-specific membership query messages (0x11). Specifies maximum allowed time before sending a responding report in one-tenth of a second. For all other messages, it is set to 0x00 by sender and ignored by receivers  
  * Checksum – 16-bit 1s complement of the 1s complement sum of the IGMP message, used by TCP/IP  
  * Group address – set to 0.0.0.0 in general query messages and to the group address in group-specific messages. Membership report messages carry address of the group being reported in this field, and group leave messages carry the address of the group being left in this field  

* Receiver sends unsolicited membership report (IGMP join) to local router for the group it wants to join  
  * Local router sends request toward the source using PIM join message  
  * When local router receives multicast stream, it forwards down to subnet where receiver is located  
  
* Router periodically sends general membership query messages into subnet, to all-hosts group address 224.0.0.1, to see if any members are in the attached subnet  
  * General query message contains max response time field set to 10 seconds by default  
  * Receivers set internal random timer between 0 and 10 seconds  
  * When timer expires, receivers send membership reports for each group they belong to  

* When receiver wants to leave group, it sends leave group message to all-routers group address 224.0.0.2  
  * When leave group message is received by router, it follows with a specific membership query to the group multicast address to determine if there are any receivers interested in the group remaining in the subnet. If none, the router removes IGMP state for group  

* If more than one router in LAN segment, an IGMP querier election determines which router will be the querier  
  * IGMPv2 routers send general membership query message with interface address as source IP address to 224.0.0.1 multicast  
  * When IGMPv2 router receives message, it checks source IP address and compares to its own interface IP address  
Router with lowest interface IP address in LAN subnet is elected as IGMP querier  

* If querier router stops sending membership queries, a new querier election takes place  
  * A non-querier router waits twice the query interval (default 60 seconds), and if it has heard no queries from IGMP querier, it triggers IGMP querier election  

## IGMPv3  
* Adds support for multicast source filtering, which gives receivers capability to pick the source they wish to accept multicast traffic from
* Supports all IGMPv2’s IGMP message types and backwards compatible with IGMPv2
 * Include mode – receiver announces membership to multicast group address and provides a list of source addresses from which it wants to receive traffic
 * Exclude mode – receiver announces membership to multicast group address and provides list of source addresses from which it does not want to receive traffic. Receiver receives traffic only from sources whose IP addresses are not listed on exclude list

## IGMP Snooping
* Examines IGMP joins sent by receivers and maintains table of interfaces to IGMP joins
* When switch receives multicast frame for a multicast group, it forwards packet only out the ports where IGMP joins were received for that specific multicast group

## Protocol Independent Multicast (PIM)
* Multicast routing protocol that routes multicast traffic between network segments
* Can use any of the unicast routing protocols to identify path between source and receivers

## PIM Distribution Trees



> [*Back*](https://github.com/network-dluong/CCNP-ENCOR/tree/3.0-Infrastructure)  

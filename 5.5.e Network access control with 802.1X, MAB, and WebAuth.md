### 802.1x  
* Standard for port-based network access control (PNAC) that provides an authentication mechanism for LANs and WLANs  
  * Extensible Authentication Protocol (EAP) - provides an encapsulated transport for authentication parameters  
  * EAP method (EAP type) - different authentication methods can be used with EAP  
  * EAP over LAN (EAPoL) - transport of EAP messages over IEEE 802 wired and wireless networks  
  * RADIUS protocol – AAA protocol used by EAP  
* Supplicant, Authenticator, and Authentication server  
* EAP identity exchange and authentication occur between Supplicant and Authentication server:  
  * Authenticator notices a port coming up and starts authentication process by sending periodic EAP-request/identify frames. Supplicant can also initiate by sending an EAPoL-start message to authenticator  
  * Authenticator relays EAP messages between supplicant and authentication server  
  * If successful, the server returns a RADIUS access-accept message with encapsulated EAP-success message and opens the port  
* EAP-MD5, EAP-TLS (uses PKI), PEAP (EAP-MSCHAPv2 or PEAPv0, EAP-GTC or PEAPv1, EAP-TLS, EAP-FAST, EAP-TTLS)  
* EAP Chaining – supports machine and user authentication inside a single outer TLS tunnel  


### MAC Authentication Bypass (MAB)  
* An access control technique that enables port-based access control using MAC address of an endpoint, and typically used as a fallback mechanism to 802.1x  
  * Switch initiates authentication by sending an EAPoL identity request message to the endpoint every 30 seconds by default. After three timeouts, endpoint does not have supplicant and authenticates via MAB  
  * Switch begins MAB by opening port to accept a single packet and learns source MAC address of endpoint. Packets sent before port has fallen back to MAB are discarded and cannot be used to learn MAC address. It crafts a RADIUS access-request message using endpoint’s MAC address and performs authentication  
  * RADIUS server sends the RADIUS response (access-accept) to authenticator which allows endpoint to access the network  


### Web Authentication (WebAuth)  
* Used as a fallback authentication mechanism for 802.1x. Switch attempts MAB first  
* Endpoints are presented with a web portal requesting username and password. Results are sent from switch to RADIUS server  
  * Local Web Authentication (LWA)  
    * Switch (or wireless controller) redirects web traffic to a locally hosted web portal running in the switch where end user can enter username and password  
    * Switch sends RADIUS access-request message with login credentials to RADIUS server  
    * Web portals are not customizable on Cisco switches. No native support for advanced services  
    * Does not support VLAN assignment and change of authorization (CoA); supports ACL  
  * Central Web Authentication with Cisco ISE (CWA)  
    * Supports CoA, dACL and VLAN authorization options, and all advanced services  
    * WebAuth and guest VLAN functions remain mutually exclusive  
    * Endpoint entering network does not have configured supplicant or is misconfigured  
    * Switch performs MAB, sending RADIUS access-request to Cisco ISE  
    * ISE sends RADIUS result, including URL redirection, to centralized portal on the ISE server  
    * Endpoint is assigned an IP address, DNS server, and default gateway using DHCP  
    * End user opens browser and enters credentials  
    * ISE sends re-authentication CoA to the switch  
    * Switch sends new MAB request with same session ID to ISE, and ISE sends final authorization result to the switch for the end user  


### Enhanced Flexible Authentication (FlexAuth or Access Session Manager)  
* Allows multiple authentication methods concurrently so endpoints can be authenticated and brought online quickly  
  * Cisco Identity-Based Networking Services (IBNS) 2.0  
    * Integrated solution that offers authentication, access control, and user policy enforcement with a common end-to-end access policy that applies to both wired and wireless networks  
    * Combination of: Enhanced FlexAuth, Cisco Common Classification Policy Language (C3PL), and Cisco ISE  
    
    
    

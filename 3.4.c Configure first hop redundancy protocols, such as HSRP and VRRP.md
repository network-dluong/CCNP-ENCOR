### FHRP (First-Hop Redundancy Protocol)  
* Object tracking – users can track specific objects in network and take necessary action when any object’s state change affects network traffic:  
> **Track (*object-number*) ip route (*route/prefix-length*) reachability**  
> **show track (*object-number*)**  


### HSRP (Hot Standby Router Protocol)  
1. Define HSRP instance:  
> **standby (*id*) ip (*vip-address*)**  
2. (Optional) Configure HSRP router preemption:  
> **standby (*id*) preempt**  
3. (Optional) Define HSRP priority, value between 0 and 255:  
> **standby (*id*) priority (*#*)**  
4. (Optional) Define MAC Address:  
> **standby (*id*) mac-address (*mac-address*)**  
5. (Optional) Define HSRP timers (intervals of 1 to 254 seconds, or 15 to 999 milliseconds):  
> **standby (*id*) timers {(*seconds*) | msec (*milliseconds*)}**  
6. (Optional) Establish HSRP authentication:  
> **standby (*id*) authentication {(*text-password*) | text (*text-password*) | md5 {key-chain (*chain*) | key-string (*string*)}}**  

> **Show standby (*int-id*) brief**  


### VRRP (Virtual Router Redundancy Protocol)  
### Legacy VRRP:  
1. Define VRRP instance:  
> **vrrp (*id*) ip (*vip-address*)**  
2. (Optional) Define VRRP priority (0 to 255):  
> **vrrp (*id*) priority (*#*)**  
3. (Optional) Enable object tracking so priority is decremented when object is false:  
> **vrrp (*id*) track (*object-id*) decrement (*#*)**  
4. (Optional) Establish VRRP authentication:  
> **vrrp (*id*) authentication {(*text-password*) | text (*password*) | md5 {key-chain (*chain*) | key-string (*string*)}}**  


### Hierarchical VRRP:  
1. Enable VRRPv3:  
> **fhrp version vrrp v3**  
2. Define VRRP instance:  
> **vrrp (*id*) address-family [ipv4 | ipv6]**  
3. (Optional) Change VRRP to Version 2(V2 and V3 not compatible):  
> **vrrpv2**  
4. Define gateway VIP:  
> **address (*ip-address*)**  
5. (Optional) Define VRRP priority between 0 and 255:  
> **priority (*#*)**  
6. (Optional) enable object tracking:  
> **track (*id*) decrement (*value*)**  


### GLBP (Global Load Balancing Protocol)  
1. Define GLBP instance:  
> **glbp (*id*) ip (*vip-address*)**  
2. (Optional) configure GLBP preemption:  
> **glbp (*id*) preempt**  
3. (Optional) Define GLBP priority between 0 and 255:  
> **glbp (*id*) priority (*#*)**  
4. (Optional) Define GLBP timers:  
> **glbp (*id*) timers {(*hello-seconds*) | msec (*hello-milliseconds*)} {(*hold-seconds*) | msec (*hold-milliseconds*)}**  
5. (Optional) establish GLBP authentication:  
> **glbp (*id*) authentication {text (*password*) | md5 {key-chain (*chain*) | key-string (*string*)}}**  
6. Round robin configured by default and can be changed (can be Weighted, Host dependent):  
> **glbp (*id*) load-balancing {host-dependent | round-robin | weighted}**  


> [*Back*](https://github.com/network-dluong/CCNP-ENCOR/tree/3.0-Infrastructure)  

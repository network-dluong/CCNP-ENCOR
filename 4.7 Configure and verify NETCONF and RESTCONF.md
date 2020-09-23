## NETCONF  
* IETF standard protocol that uses YANG data models to communicate with various devices on the network  
* Runs over SSH, TLS, and SOAP (Simple Object Access Protocol)  
* Can distinguish between configuration data and operational data  
* Uses paths to describe resources, whereas SNMP uses object identifiers (OIDs)  
  * Collects status of specific fields  
  * Changes configuration of specific fields  
  * Takes administrative actions  
  * Sends event notifications  
  * Backs up and restores configurations  
  * Tests configurations before finalizing transaction  
* Exchanges information called capabilities when TCP connection is made. Capabilities tell client what device it’s connected to can do  
  * **get** - requests running configuration and state information of device  
  * **get-config** - requests some or all configuration from datastore  
  * **edit-config** - edits configuration datastore by using CRUD operations  
  * **copy-config** - copies configuration to another datastore  
  * **delete-config** - deletes configuration  


## RESTCONF  
* Used to programmatically interface with data defined in YANG models while also using datastore concepts defined in NETCONF  
* Provides RESTful API experience while leveraging device abstraction capabilities provided by NETCONF  
* Supports the following HTTP methods and CRUD operations:  
  * **GET**  
  * **POST**  
  * **PUT**  
  * **DELETE**  
  * **OPTIONS**  
* Requests and responses can use either JSON or XML structured data formats  


> *[Back](https://github.com/network-dluong/CCNP-ENCOR/tree/4.0-Network-Assurance)*  

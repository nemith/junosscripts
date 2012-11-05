#show-zone-for-ip.slax

This script is useful on SRX platforms where you need to idenify the zone for a paticular IP address using the routing table and matching that up with interfaces.  Supports virtual routers

##Changelog 

v1.1:
* Fixed issue when used on a clustered SRX
* Increaed the IP column for IPv6 valuestoa
* Optimized lookup the zone for an interface (remoted the loop)
* Use a single RPC connection
* Added some progress information for debugging

v1.1.1:
* Fixed stupid bug in matching a zone interface

v1.2:
* Allow for the specified IP address to be a local IP address on  the firewall

##Example

	bbennett@SRX210> op show-zone-for-ip ip 172.16.1.40    
	  Matching Route                 NH Interface    Route Table     Security Zone
	  172.16.1.0/24                  vlan.1001       inet.0          TRUST

	bbennett@SRX210> op show-zone-for-ip ip 8.8.8.8        
	  Matching Route                 NH Interface    Route Table     Security Zone
	  0.0.0.0/0                      fe-0/0/7.0      inet.0          UNTRUST   
	  0.0.0.0/0                      lt-0/0/0.2      test.inet.0     RI-TRUST
	  0.0.0.0/0                      lt-0/0/0.3      test.inet.0     RI-TRUST
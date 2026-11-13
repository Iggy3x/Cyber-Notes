# Home Lab Glossary

**AD** — Active Directory. Directory service for domain users, groups, computers and other objects.  
**AD DS** — Active Directory Domain Services. Windows Server role providing AD domain functionality.  
**DC** — Domain Controller. Hosts AD DS and authenticates domain users.  
**DNS** — Domain Name System. Resolves names and supports AD service discovery.  
**DHCP** — Dynamic Host Configuration Protocol. Assigns IP configuration to clients. In this lab DHCP Server runs on `MM-DC01`.  
**GPO** — Group Policy Object. Centralized Windows configuration/policy.  
**OU** — Organizational Unit. AD container used for organization, GPO links and delegation.  
**RSAT** — Remote Server Administration Tools. Admin consoles installed on `MM-WS01`.  
**LAPS** — Local Administrator Password Solution. Centrally manages local admin passwords.  
**WinRM** — Windows Remote Management. Transport used by PowerShell Remoting.  
**SMB** — Server Message Block. Windows network file-sharing protocol.  
**NTFS** — Windows filesystem; NTFS ACLs control file/folder permissions.  
**ACL** — Access Control List. Collection of permission entries.  
**ACE** — Access Control Entry. One permission entry inside an ACL.  
**UNC** — Universal Naming Convention, e.g. `\\server\share`.  
**FQDN** — Fully Qualified Domain Name, e.g. `WS-02.ad.malunas.test`.  
**UPN** — User Principal Name, e.g. `user@ad.malunas.test`.  
**SID** — Security Identifier used internally by Windows security.  
**GC** — Global Catalog. AD search/catalog role on a DC.  
**CIDR** — Subnet notation; `/24` = `255.255.255.0`.  
**A record** — DNS name -> IPv4 address.  
**PTR record** — DNS IP address -> hostname.  
**Forward Lookup Zone** — stores normal hostname-to-IP DNS records.  
**Reverse Lookup Zone** — stores IP-to-hostname PTR records.  
**Lease** — time-limited DHCP assignment of an IP to a client.  
**Network ID** — identifies the subnet itself; in this lab `10.10.10.0/24`.

# 01 install domain controller 


1. use `sconfig` to:
    - change our hostname 
    - change our ip address to static 
    - change our dns server to our own ip address 


2. install the active directory windows feature 
```shell
Install-WindowsFeature -AD-Domain-Services -IncludeManagementTools
```

3. make sure you change the dns server on the dc from 127.0.0.1 to its own ip address

```shell
Set-DNSClientServerAddress -InterfaceIndex <II> -ServerAddress <ip>
```
once this is done it's officialy installed and ready to go 

4. in the video we added a workstation vm to the domain

    - the first step is changing the dns server from the nic's default to the domain controllers ip
```shell
Set-DNSClientServerAddress -InterfaceIndex <II> -ServerAddress <DC ip>
```
this ensures that we can access the domain ldap service on the domain controller 
5. then what we do is join the workstation to the domain
    - this can be done multiples ways but the most common ones include going to settings > accounts > access work or school, and then following the intuitive process that they show. or you can do it the powershell way (much cooler imo)
```shell
Add-Computer -DomainName <domainname> -Credential <domainname>\username -restart -force
``` 
this is what initiates the join process
the join process:
so the computer must use the dns server on the DC to query the ip and port for: "_ldap._tcp.dc._msdcs.corp.local"
the ldap service is basically used for retriving data about the active directory such as user group info. 
from that it will return a list of services on the domain for which to use a domain controller offering ldap through tcp. 
usually this is port 389 or the ldap port on the dc 
then it will query "_kerberos._tcp.dc._msdcs.corp.local" for the authentication to the domain - pretty simple
from there it uses the kerberos service returned from the endpoint to do the actual authentication for the join
you must have access to a domain admin account to authenticate.
then the process of kerberos authentication happens and this is well documented so i won't regurgitate it here 







    

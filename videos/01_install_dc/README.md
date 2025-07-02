# 01 install domain controller 


1. use `sconfig` to:
    - change our hostname 
    - change our ip address to static 
    - change our dns server to our own ip address 


2. install the active directory windows feature 
```shell
Install-WindowsFeature -AD-Domain-Services -IncludeManagementTools
```
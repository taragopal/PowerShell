The *list-mDNS.ps1* Script
===========================

list-mDNS.ps1 


Parameters
----------
```powershell


[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Script Content
--------------
```powershell
Get-NetUDPEndpoint -localPort 5353 | Select-Object LocalAddress,LocalPort,OwningProcess,@{ Name="Process"; Expression={((Get-Process -Id $_.OwningProcess).Name )} }
exit 0 # success
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:38)*

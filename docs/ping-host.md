The *ping-host.ps1* Script
===========================

This PowerShell script pings the given host.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/ping-host.ps1 [[-hostname] <String>] [<CommonParameters>]

-hostname <String>
    Specifies the hostname or IP address to ping (x.com by default)
    
    Required?                    false
    Position?                    1
    Default value                x.com
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./ping-host.ps1 x.com
✅ 'x.com' is online (20ms to IP 104.244.42.1)

```

Notes
-----
Author: Markus Fleschutz | License: CC0

Related Links
-------------
https://github.com/fleschutz/PowerShell

Script Content
--------------
```powershell
<#
.SYNOPSIS
	Pings a host
.DESCRIPTION
	This PowerShell script pings the given host.
.PARAMETER hostname
	Specifies the hostname or IP address to ping (x.com by default)
.EXAMPLE
	PS> ./ping-host.ps1 x.com
	✅ 'x.com' is online (20ms to IP 104.244.42.1)
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

param([string]$hostname = "x.com")

try {
	$remoteHosts = $hostname.Split(",")
	$tasks = $remoteHosts | foreach { (New-Object Net.NetworkInformation.Ping).SendPingAsync($_,5000) }
	[Threading.Tasks.Task]::WaitAll($tasks)
	foreach($ping in $tasks.Result) {
		if ($ping.Status -eq "Success") {
			Write-Output "✅ '$hostname' is online ($($ping.RoundtripTime / 2)ms to IP $($ping.Address))"
			exit 0 # success
		} else {
			Write-Output "⚠️ No reply from '$hostname' (IP $($ping.Address)) - check the connection or maybe the host is down."
			exit 1
		}
	}
	Write-Output "⚠️ No reply from '$hostname' - check the connection or maybe the host is down."
	exit 1
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:40)*

The *ping-internet.ps1* Script
===========================

This PowerShell script measures the ping roundtrip times from the local computer to remote ones (10 Internet servers by default).

Parameters
----------
```powershell
/Repos/PowerShell/scripts/ping-internet.ps1 [[-hosts] <String>] [<CommonParameters>]

-hosts <String>
    Specifies the hosts to ping, seperated by commata (10 Internet servers by default)
    
    Required?                    false
    Position?                    1
    Default value                bing.com,cnn.com,dropbox.com,github.com,google.com,ibm.com,live.com,meta.com,x.com,youtube.com
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
PS> ./ping-internet.ps1
✅ Internet ping: 12ms (9...18ms range)

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
	Pings remote hosts to measure the latency 
.DESCRIPTION
	This PowerShell script measures the ping roundtrip times from the local computer to remote ones (10 Internet servers by default).
.PARAMETER hosts
	Specifies the hosts to ping, seperated by commata (10 Internet servers by default)
.EXAMPLE
	PS> ./ping-internet.ps1
	✅ Internet ping: 12ms (9...18ms range)
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

param([string]$hosts = "bing.com,cnn.com,dropbox.com,github.com,google.com,ibm.com,live.com,meta.com,x.com,youtube.com")

try {
	$hostsArray = $hosts.Split(",")
	$tasks = $hostsArray | foreach { (New-Object Net.NetworkInformation.Ping).SendPingAsync($_,1000) }
	[int]$min = 9999999
	[int]$max = [int]$avg = [int]$success = 0
	[int]$total = $hostsArray.Count
	[Threading.Tasks.Task]::WaitAll($tasks)
	foreach($ping in $tasks.Result) {
		if ($ping.Status -ne "Success") { continue }
		$success++
		[int]$latency = $ping.RoundtripTime
		$avg += $latency
		if ($latency -lt $min) { $min = $latency }
		if ($latency -gt $max) { $max = $latency }
	}
	[int]$loss = $total - $success
	if ($success -eq 0) {
		Write-Host "⚠️ Internet offline (100% ping loss)"
	} elseif ($loss -gt 0) {
		[float]$speed = [math]::round([float]$avg / [float]$success, 1)
		Write-Host "✅ Online with $loss/$total ping loss and $($min)...$($max)ms latency - $($speed)ms average"
	} else {
		[float]$speed = [math]::round([float]$avg / [float]$success, 1)
		Write-Host "✅ Internet ping: $($speed)ms ($min...$($max)ms range)"
	}
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:40)*

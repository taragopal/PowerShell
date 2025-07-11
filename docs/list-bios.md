The *list-bios.ps1* Script
===========================

This PowerShell script lists the BIOS details.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/list-bios.ps1 [<CommonParameters>]

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./list-bios.ps1



SMBIOSBIOSVersion : F6
Manufacturer      : American Megatrends Inc.
...

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
	Lists BIOS details
.DESCRIPTION
	This PowerShell script lists the BIOS details.
.EXAMPLE
	PS> ./list-bios.ps1

	SMBIOSBIOSVersion : F6
	Manufacturer      : American Megatrends Inc.
	...
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

try {
	Get-CimInstance -ClassName Win32_BIOS
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:37)*

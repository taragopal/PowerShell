The *open-google-maps.ps1* Script
===========================

This PowerShell script launches the Web browser with the Google Maps website.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/open-google-maps.ps1 [<CommonParameters>]

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./open-google-maps

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
	Opens Google Maps
.DESCRIPTION
	This PowerShell script launches the Web browser with the Google Maps website.
.EXAMPLE
	PS> ./open-google-maps
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

& "$PSScriptRoot/open-default-browser.ps1" "https://www.google.com/maps"
exit 0 # success
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:39)*

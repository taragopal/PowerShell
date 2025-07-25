The *open-fire-place.ps1* Script
===========================

This PowerShell script launches the Web browser with a fire place website.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/open-fire-place.ps1 [<CommonParameters>]

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./open-fire-place

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
	Opens a fire place website
.DESCRIPTION
	This PowerShell script launches the Web browser with a fire place website.
.EXAMPLE
	PS> ./open-fire-place
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

& "$PSScriptRoot/open-default-browser.ps1" "https://freefireplaces.com"
exit 0 # success
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:39)*

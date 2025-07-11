The *open-d-drive.ps1* Script
===========================

This PowerShell script launches the File Explorer with the D: drive folder.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/open-d-drive.ps1 [<CommonParameters>]

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./open-d-drive

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
	Opens the D: drive folder
.DESCRIPTION
	This PowerShell script launches the File Explorer with the D: drive folder.
.EXAMPLE
	PS> ./open-d-drive
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

& "$PSScriptRoot/open-file-explorer.ps1" "D:"
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:39)*

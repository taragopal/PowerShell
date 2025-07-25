The *enable-god-mode.ps1* Script
===========================

This PowerShell script enables the god mode in Windows. It adds a new icon to the desktop.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/enable-god-mode.ps1 [<CommonParameters>]

[<CommonParameters>]
    This script supports the common parameters: Verbose, Debug, ErrorAction, ErrorVariable, WarningAction, 
    WarningVariable, OutBuffer, PipelineVariable, and OutVariable.
```

Example
-------
```powershell
PS> ./enable-god-mode.ps1
✔ God mode enabled - just double-click the new desktop icon.

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
	Enables the god mode
.DESCRIPTION
	This PowerShell script enables the god mode in Windows. It adds a new icon to the desktop.
.EXAMPLE
	PS> ./enable-god-mode.ps1
	✔ God mode enabled - just double-click the new desktop icon.
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

try {
	$GodModeSplat = @{
		Path = "$HOME\Desktop"
		Name = "GodMode.{ED7BA470-8E54-465E-825C-99712043E01C}"
		ItemType = 'Directory'
	}
	$null = New-Item @GodModeSplat
	"✅ God mode enabled - just double-click the new desktop icon."
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:36)*

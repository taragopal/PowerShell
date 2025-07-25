The *list-pins.ps1* Script
===========================

This PowerShell script lists random PIN's.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/list-pins.ps1 [[-PinLength] <Int32>] [[-Columns] <Int32>] [[-Rows] <Int32>] [<CommonParameters>]

-PinLength <Int32>
    Specifies the PIN length
    
    Required?                    false
    Position?                    1
    Default value                5
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

-Columns <Int32>
    Specifies the number of columns
    
    Required?                    false
    Position?                    2
    Default value                12
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

-Rows <Int32>
    Specifies the number of rows
    
    Required?                    false
    Position?                    3
    Default value                30
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
PS> ./list-pins.ps1

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
	Lists random PIN's
.DESCRIPTION
	This PowerShell script lists random PIN's.
.PARAMETER PinLength
	Specifies the PIN length
.PARAMETER Columns
	Specifies the number of columns
.PARAMETER Rows
	Specifies the number of rows
.EXAMPLE
	PS> ./list-pins.ps1
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

param([int]$PinLength = 5, [int]$Columns = 12, [int]$Rows = 30)

try {
	write-output ""
	$Generator = New-Object System.Random
	for ($j = 0; $j -lt $Rows; $j++) {
		$Line = ""
		for ($k = 0; $k -lt $Columns; $k++) {
			for ($i = 0; $i -lt $PinLength; $i++) {
				$Line += [char]$Generator.next(48,57)
			}
			$Line += "   "
		}
		write-output $Line
	}
	write-output ""
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:38)*

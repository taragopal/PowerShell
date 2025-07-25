The *write-pi.ps1* Script
===========================

This PowerShell script calculates and writes the digits of the mathematical constant PI.

Parameters
----------
```powershell
/Repos/PowerShell/scripts/write-pi.ps1 [[-digits] <Int32>] [<CommonParameters>]

-digits <Int32>
    Specifies the number of digits to list (1000 by default)
    
    Required?                    false
    Position?                    1
    Default value                1000
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
PS> ./write-pi.ps1
3.141592653589793238462643383279502884197169399375105820974944592307816406286208998628034825342...

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
	Writes PI
.DESCRIPTION
	This PowerShell script calculates and writes the digits of the mathematical constant PI.
.PARAMETER digits
	Specifies the number of digits to list (1000 by default)
.EXAMPLE
	PS> ./write-pi.ps1
	3.141592653589793238462643383279502884197169399375105820974944592307816406286208998628034825342...
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

param([int]$digits = 1000)

function Write-Pi ( $digits ) {
	$Big = [bigint[]](0..10)
 
	$ndigits = 0
 
 	$q = $t = $k = $Big[1]
 	$r =           $Big[0]
	$l = $n =      $Big[3]

	# calculate first digit
	$nr = ( $Big[2] * $q + $r ) * $l
	$nn = ( $q * ( $Big[7] * $k + $Big[2] ) + $r * $l ) / ( $t * $l )
	$q *= $k
	$t *= $l
	$l += $Big[2]
	$k = $k + $Big[1]
	$n = $nn
	$r = $nr
 
 	Write-Host "$($n)." -noNewline
 	$ndigits++
 
 	$nr = $Big[10] * ( $r - $n * $t )
 	$n = ( ( $Big[10] * ( 3 * $q + $r ) ) / $t ) - 10 * $n
 	$q *= $Big[10]
 	$r = $nr
 
 	while ($ndigits -lt $digits) {
		if ($Big[4] * $q + $r - $t -lt $n * $t) {
			Write-Host "$n" -noNewline
			$ndigits++
			$nr = $Big[10] * ( $r - $n * $t )
			$n = ( ( $Big[10] * ( 3 * $q + $r ) ) / $t ) - 10 * $n
			$q *= $Big[10]
			$r = $nr
		} else {
			$nr = ( $Big[2] * $q + $r ) * $l
			$nn = ( $q * ( $Big[7] * $k + $Big[2] ) + $r * $l ) / ( $t * $l )
			$q *= $k
			$t *= $l
			$l += $Big[2]
			$k = $k + $Big[1]
			$n = $nn
			$r = $nr
		}
      }
	Write-Host "...  ($digits digits)"
}

try {
	Write-Pi $digits
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:42)*

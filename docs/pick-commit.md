The *pick-commit.ps1* Script
===========================

Cherry-picks a Git commit into one or more branches (branch names need to be separated by spaces)
NOTE: in case of merge conflicts the script stops immediately!

Parameters
----------
```powershell
/Repos/PowerShell/scripts/pick-commit.ps1 [[-CommitID] <String>] [[-CommitMessage] <String>] [[-Branches] <String>] [[-RepoDir] <String>] [<CommonParameters>]

-CommitID <String>
    Specifies the commit ID
    
    Required?                    false
    Position?                    1
    Default value                
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

-CommitMessage <String>
    Specifies the commit message to use
    
    Required?                    false
    Position?                    2
    Default value                
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

-Branches <String>
    Specifies the list of branches, separated by spaces
    
    Required?                    false
    Position?                    3
    Default value                
    Accept pipeline input?       false
    Aliases                      
    Accept wildcard characters?  false

-RepoDir <String>
    Specifies the path to the Git repository
    
    Required?                    false
    Position?                    4
    Default value                "$PWD"
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
PS> ./pick-commit 93849f889 "Fix typo" "v1 v2 v3"

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
	Cherry-picks a Git commit into one or more branches
.DESCRIPTION
	Cherry-picks a Git commit into one or more branches (branch names need to be separated by spaces)
	NOTE: in case of merge conflicts the script stops immediately! 
.PARAMETER CommitID
	Specifies the commit ID
.PARAMETER CommitMessage
	Specifies the commit message to use
.PARAMETER Branches
	Specifies the list of branches, separated by spaces
.PARAMETER RepoDir
	Specifies the path to the Git repository
.EXAMPLE
	PS> ./pick-commit 93849f889 "Fix typo" "v1 v2 v3"
.LINK
	https://github.com/fleschutz/PowerShell
.NOTES
	Author: Markus Fleschutz | License: CC0
#>

param([string]$CommitID = "", [string]$CommitMessage = "", [string]$Branches = "", [string]$RepoDir = "$PWD")

try {
	if (-not(Test-Path "$RepoDir" -pathType container)) { throw "Can't access directory: $RepoDir" }
	Set-Location "$RepoDir"

	if ($CommitID -eq "") { $CommitID = read-host "Enter the Git commit id to cherry-pick" }
	if ($CommitMessage -eq "") { $CommitMessage = read-host "Enter the commit message to use" }
	if ($Branches -eq "") { $Branches = read-host "Enter the branches (separated by spaces)" }
	
	$StopWatch = [system.diagnostics.stopwatch]::startNew()

	$BranchArray = $Branches.Split(" ")
	$NumBranches = $BranchArray.Count
	foreach($Branch in $BranchArray) {

		"🍒 Switching to branch $Branch ..."
		& git checkout --recurse-submodules --force $Branch
		if ($lastExitCode -ne 0) { throw "'git checkout $Branch' failed" }

		"🍒 Updating submodules..."
		& git submodule update --init --recursive
		if ($lastExitCode -ne 0) { throw "'git submodule update' failed" }

		"🍒 Cleaning the repository from untracked files..."
		& git clean -fdx -f
		if ($lastExitCode -ne 0) { throw "'git clean -fdx -f' failed" }
			
		& git submodule foreach --recursive git clean -fdx -f
		if ($lastExitCode -ne 0) { throw "'git clean -fdx -f' in submodules failed" }

		"🍒 Pulling latest updates..."
		& git pull --recurse-submodules 
		if ($lastExitCode -ne 0) { throw "'git pull' failed" }

		"🍒 Checking the status..."
		$Result = (git status)
		if ($lastExitCode -ne 0) { throw "'git status' failed" }
		if ("$Result" -notmatch "nothing to commit, working tree clean") { throw "Branch is NOT clean: $Result" }

		"🍒 Cherry picking..."
		& git cherry-pick --no-commit "$CommitID"
		if ($lastExitCode -ne 0) { throw "'git cherry-pick $CommitID' failed" }

		"🍒 Committing..."
		& git commit -m "$CommitMessage"
		if ($lastExitCode -ne 0) { throw "'git commit' failed" }

		"🍒 Pushing..."
		& git push
		if ($lastExitCode -ne 0) { throw "'git push' failed" }
	}
	[int]$Elapsed = $StopWatch.Elapsed.TotalSeconds
	"✅ cherry picked $CommitID into $NumBranches branches in $Elapsed sec"
	exit 0 # success
} catch {
	"⚠️ Error in line $($_.InvocationInfo.ScriptLineNumber): $($Error[0])"
	exit 1
}
```

*(page generated by convert-ps2md.ps1 as of 06/22/2025 10:37:40)*

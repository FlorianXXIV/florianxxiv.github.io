---
layout: default
title: "PowerShell"
---

## Custom PowerShell prompt

This is a prompt for PowerShell, for it to work you'll need a monospaced nerd font like "JetBrains Mono Nerd Font"
I think it looks nice, it's moddeld after the "bobthefish" theme of the fish shell, sadly without the additional functionality.

```PowerShell
function prompt {
	$identity = [Security.Principal.WindowsIdentity]::GetCurrent()
    $principal = [Security.Principal.WindowsPrincipal] $identity
    $adminRole = [Security.Principal.WindowsBuiltInRole]::Administrator
	
	$prefix = $(if (Test-Path variable:/PSDebugContext) { '🐛:' }
                elseif ($principal.IsInRole($adminRole)) { '🛡:' }
                else { "$($PSStyle.Background.FromRgb(0x2f2f2f))$($PSStyle.Foreground.FromRgb(0x6084f3))" + $env:USERNAME + "$($PSStyle.Foreground.Yellow)" + '@' + "$($PSStyle.Foreground.White)"})

	$drive = $(Get-Location).Drive
    $winPath = $(Get-Location).Path.Split('\')
	if ($(Get-Location).Path -like $HOME + '*') {
		$path = "~"
		$i = 3
	} else {
		$path = '/'+$drive.ToString()
		$i = 1
	}
	for($i; $i -lt $winPath.Length; $i++) {
		if ($i -eq $winPath.Length-1) {
			$path += '/' + $winPath[$i]
		} else {
			$path += '/' + $winPath[$i][0]
		}
	}

	$body = $path
	
				
	$suffix = " $($PSStyle.Background.Black)$($PSStyle.Foreground.FromRgb(0x2f2f2f)) $($PSStyle.Reset)"
	
    $prefix + $body + $suffix
}
```
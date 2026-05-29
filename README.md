# search-win
Scripts for searching text.

## Installation
    git clone https://github.com/devubu/search-win.git ~/Tools/Custom/PowerShell/search-win

    New-Item -ItemType Directory -Force -Path $HOME/bin
    Invoke-WebRequest 'https://github.com/junegunn/fzf/releases/download/v0.72.0/fzf-0.72.0-windows_amd64.zip' -OutFile $HOME\bin\fzf-0.72.0-windows_amd64.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\fzf-0.72.0-windows_amd64.zip", "$HOME\bin")
    Remove-Item "$HOME\bin\fzf-0.72.0-windows_amd64.zip"

    Invoke-WebRequest 'https://github.com/BurntSushi/ripgrep/releases/download/15.1.0/ripgrep-15.1.0-x86_64-pc-windows-msvc.zip' -OutFile $HOME\bin\ripgrep-15.1.0-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\ripgrep-15.1.0-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\ripgrep-15.1.0-x86_64-pc-windows-msvc\rg.exe" "$HOME\bin\rg.exe"
    Remove-Item "$HOME\bin\ripgrep-15.1.0-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\ripgrep-15.1.0-x86_64-pc-windows-msvc" -Recurse

    Invoke-WebRequest 'https://github.com/sharkdp/fd/releases/download/v10.4.2/fd-v10.4.2-x86_64-pc-windows-msvc.zip' -OutFile $HOME\bin\fd-v10.4.2-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\fd-v10.4.2-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\fd-v10.4.2-x86_64-pc-windows-msvc\fd.exe" "$HOME\bin\fd.exe"
    Remove-Item "$HOME\bin\fd-v10.4.2-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\fd-v10.4.2-x86_64-pc-windows-msvc" -Recurse

    Invoke-WebRequest 'https://github.com/sharkdp/bat/releases/download/v0.26.1/bat-v0.26.1-x86_64-pc-windows-msvc.zip' -OutFile $HOME\bin\bat-v0.26.1-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\bat-v0.26.1-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\bat-v0.26.1-x86_64-pc-windows-msvc\bat.exe" "$HOME\bin\bat.exe"
    Remove-Item "$HOME\bin\bat-v0.26.1-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\bat-v0.26.1-x86_64-pc-windows-msvc" -Recurse

## Check if you have a profile 
    $PROFILE

## Create a backup of the profile
    Copy-Item -Path $PROFILE -Destination "$PROFILE.bak" -Force

## If the profile doesn’t exist, create the file
    New-Item -ItemType File -Force -Path $PROFILE

### Open the profile
    notepad $PROFILE

## Add the following to the profile

    function cbat {
        bat --paging=never @Args
    }
    
    function bfzf {
        fzf --preview="bat --color=always {}"
    }
    
    function cdfzf {
        param([string]$Path = ".")
    
        $selected_dir = fd -a -u -t d . $Path | fzf
    
        if ($selected_dir) {
            Set-Location $selected_dir
        }
    }
    
    function ffzf {
        param([string]$Path = ".")
    
        fd -a -u -t f . $Path | fzf --preview='bat --color=always {}'
    }
    
    function dfzf {
        param([string]$Path = ".")
    
        fd -a -u -t d . $Path | fzf --preview 'dir /a --color=always {}' --preview-window='~4,+{2}+4/3,<80(up),wrap'
    }
    
    function copy {
        Set-Clipboard
    }
    
    function paste {
        Get-Clipboard
    }

## Reload the profile
    . $PROFILE

## Modify Environment Variable $env:PATH (For CTFs) (Risker: Programs there take priority over system/user executables with the same name.)
    $env:PATH = "$HOME\bin;" + $env:PATH

## Alternative Modify Environment Variable $env:PATH (Safer: system directories (C:\Windows\System32, etc.) still take priority.)
    $env:PATH = $env:PATH + ";$HOME\bin"

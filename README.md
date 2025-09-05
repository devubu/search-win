# search
Scripts for searching text.

## Installation
    git clone https://github.com/devubu/search-win.git ~/Tools/Custom/PowerShell/search-win

    New-Item -ItemType Directory -Force -Path $HOME/bin
    Invoke-WebRequest 'https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep-14.1.1-x86_64-pc-windows-msvc.zip' -OutFile $HOME/bin/ripgrep-14.1.1-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\ripgrep-14.1.1-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\ripgrep-14.1.1-x86_64-pc-windows-msvc\rg.exe" "$HOME\bin\rg.exe"
    Remove-Item "$HOME\bin\ripgrep-14.1.1-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\ripgrep-14.1.1-x86_64-pc-windows-msvc" -Recurse

    Invoke-WebRequest 'https://github.com/sharkdp/fd/releases/download/v10.3.0/fd-v10.3.0-x86_64-pc-windows-msvc.zip' -OutFile $HOME/bin/fd-v10.3.0-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\fd-v10.3.0-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\fd-v10.3.0-x86_64-pc-windows-msvc\fd.exe" "$HOME\bin\fd.exe"
    Remove-Item "$HOME\bin\fd-v10.3.0-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\fd-v10.3.0-x86_64-pc-windows-msvc" -Recurse

    Invoke-WebRequest 'https://github.com/junegunn/fzf/releases/download/v0.65.2/fzf-0.65.2-windows_amd64.zip' -OutFile $HOME/bin/fzf-0.65.2-windows_amd64.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\fzf-0.65.2-windows_amd64.zip", "$HOME\bin")
    Remove-Item "$HOME\bin\fzf-0.65.2-windows_amd64.zip"

    Invoke-WebRequest 'https://github.com/sharkdp/bat/releases/download/v0.25.0/bat-v0.25.0-x86_64-pc-windows-msvc.zip' -OutFile $HOME/bin/bat-v0.25.0-x86_64-pc-windows-msvc.zip
    Add-Type -AssemblyName System.IO.Compression.FileSystem
    [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\bin\bat-v0.25.0-x86_64-pc-windows-msvc.zip", "$HOME\bin")
    Move-Item "$HOME\bin\bat-v0.25.0-x86_64-pc-windows-msvc\bat.exe" "$HOME\bin\bat.exe"
    Remove-Item "$HOME\bin\bat-v0.25.0-x86_64-pc-windows-msvc.zip"
    Remove-Item "$HOME\bin\bat-v0.25.0-x86_64-pc-windows-msvc" -Recurse

## Check if you have a profile 
    $PROFILE

## Create a backup of the profile
    Copy-Item -Path $PROFILE -Destination "$PROFILE.bak" -Force

## If the profile doesn’t exist, create the it
    New-Item -ItemType File -Force -Path $PROFILE

### Open the profile
    notepad $PROFILE

## Add the following to the profile
    function cbat {
        bat --paging=never @Args
    }
    
    function ff {
        fd . -a -u -t f | fzf --preview="bat --color=always {}"
    }
    
    function bfzf {
        fzf --preview="bat --color=always {}"
    }
    
    function gfzf {
        $reload = 'reload:rg -uuu --column --color=always --smart-case {q} || :'
    
        fzf --disabled --ansi `
            --bind "start:$reload" `
            --bind "change:$reload" `
            --delimiter ":" `
            --preview 'bat --style=numbers --color=always --highlight-line {2} {1}' `
            --preview-window "+{2}/2"
    }
    
    function copy {
        Set-Clipboard
    }
    
    function paste {
        Get-Clipboard
    }

## Reload the profile
    . $PROFILE

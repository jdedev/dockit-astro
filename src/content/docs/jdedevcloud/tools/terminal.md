---
title : linux terminal
---

## linux terminal

### customize the prompt by adding color codes

```shell
PS1='[\[\033[01;32m\]\u\[\033[00m\]@\[\033[01;34m\]\h\[\033[00m\]:\[\033[01;33m\]\w\[\033[00m\]]\$ '

```

## windows command 

### get variables

```shell
set
```

```shell
# command output
POSH_INSTALLER=ws
POSH_THEMES_PATH=C:\Users\PR042999\AppData\Local\Programs\oh-my-posh\themes
PROCESSOR_ARCHITECTURE=AMD64

```


## install termminal

### download 

> https://github.com/microsoft/terminal/releases

### install 

- Download Pre Install Kit

```shell
Add-AppxPackage -path .\Microsoft.VCLibs.140.00.UWPDesktop_14.0.30704.0_x64__8wekyb3d8bbwe.appx

Add-AppxPackage -path .\Microsoft.UI.Xaml.2.7_7.2208.15002.0_x64__8wekyb3d8bbwe.appx

Add-AppxPackage -path .\1ff951bd438b4b28b40cb1599e7c9f72.msixbundle

```

- install terminal bundle

```shell
Add-AppxPackage -path .\Microsoft.WindowsTerminal_1.17.11461.0_8wekyb3d8bbwe.msixbundle
```

## oh my posh

[oh my posh](https://ohmyposh.dev/docs/)

### install

### windows fonts

Place fonts

```shell
%AppData%\Local\Microsoft\Windows\Fonts
```

### setup terminal

```json
        "defaults": 
        {
            "font": 
            {
                "face": "MesloLGLDZ Nerd Font Mono",
                "size": 11.0
            }
        },
```

### setup visual studio code

```json
"terminal.integrated.fontFamily": "MesloLGLDZ Nerd Font Mono",
```

### set theme

### download tjems (windows)

```shell
Get-PoshThemes
```

#### download themes (linux)

```shell
git clone --depth=1 --filter=blob:none --sparse https://github.com/JanDeDobbeleer/oh-my-posh.git ~/.poshthemes
cd ~/.poshthemes
git sparse-checkout set themes
mv themes/* .
rm -rf themes .git  # Clean up
```

#### windows 

> notepad $PROFILE

```shell
oh-my-posh init pwsh --config '%APPDATA%\Local\Programs\oh-my-posh\themes\rudolfs-dark.omp.json' | Invoke-Expression
```

#### lunux

> ~/.bashrc

```shell
eval "$(oh-my-posh init bash --config ~/.poshthemes/rudolfs-dark.omp.json)"
```

## winget update

> get list of upgradable modules/programs

```shell
winget upgrade
```

> upgrade module/program

```bash
winget upgrade Git.Git
```


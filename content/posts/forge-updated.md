+++ 
draft = false
date = 2024-10-03T19:05:39-04:00
title = "Forge Updated"
description = ""
slug = ""
authors = []
tags = ["CLI Tooling"]
categories = []
externalLink = ""
series = []
+++

# Forge
This was a personal project that I made while I was searching for a new job and I got it to working state that I was happy with **_HOWEVER_** I wanted to add some improvements to it.

## New Changes:
- Support for generating python as well, which also includes setting up a venv environment.
- Auto completions can be generated by cloning the repository and running task commands.

zsh-ac:
    desc: Creates zsh auto completion file
    cmds:
        forge completion zsh > zsh.txt

#### Mac/Linux
/Users/$USER/.oh-my-zsh/completions

pwsh-ac:
    desc: Creates powershell auto completion file
    cmds:
        forge completion powershell > pwsh.txt

#### For Windows: 
Create a folder for completions in your PowerShell profile folder. Then, add a script block to your Microsoft.PowerShell_profile.ps1 to register completions based on whether bash or Git for Windows is present.


In the future I want to add more to but unsure what else to add since my main languages I work with are go and python.
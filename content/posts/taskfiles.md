+++ 
draft = false
date = 2024-05-08T12:11:05-04:00
title = "Taskfile"
description = "Taskfiles"
slug = ""
authors = ["Matt Maxwell"]
tags = ["dev"]
categories = ["automation"]
#externalLink = "https://taskfile.dev"
series = ["dev"]
+++

# Taskfiles and what are they?
They are a bit easier to work with than something like GNU Make files, essentially being a modern version of a make file written with Go. This leverages the fact that it is a single binary that is easy installed or removed from an environment.

Here's an a simple task file to show off the format, which gets saved as taskfile.yml in the directory you want the task runner to operate in.

```yml
version: '3'

tasks:
    hello:
        cmds:
        - echo 'Hello world!'
```


Moving on to a more fleshed out example....

```yml
version: '3'

tasks:
  build-mac:
    desc: Build the app
    cmds:
      - go build -o forge main.go

  build-linux:
    desc: Build the app for linux
    cmds:
      - GOOS=linux GOARCH=amd64 go build -o bin/forge main.go

  build-windows:
    desc: Build the app for windows
    cmds:
      - GOOS=windows GOARCH=amd64 go build -o forge.exe main.go

  bin-mac:
    desc: Adds to user bin to use anywhere on the computer in terminal
    cmds:
      - sudo mv forge /usr/local/bin

  bin-linux:
    desc: Adds to user bin to use anywhere on the computer in terminal
    cmds:
      - sudo mv bin/forge /usr/local/bin

  bin-windows:
    desc: Adds to Windows/System32 to use anywhere on the computer in terminal
    cmds:
      - Move-Item .\forge.exe C:\Windows\System32\
```

This is part of the taskfile I have in my forge project, and you might be getting a better idea of the power a taskfile can have. Let's continue on with my most used tasks from this.

```sh
task build-mac bin-mac
```

1. First this begins by building the binary for forge and placing it in the current directory.
2. Next we chain the bin-mac command in to move it to the /usr/local/bin to have the bianry available in the $PATH
3. Since this is moving in to /bin it does need elevated privelage so enter in your password to move it.
4. Now `forge` works anywhere on my system in the terminal


## Further steps and other information

I would encourage people to take a look at [taskfile.dev](https://taskfile.dev) since this is a beginner guide of sorts, mainly meant to encourage people to find out about taskfiles and maybe incorporate them in to their workflows.

Lastly, you can do some fun things such as including shell scripts as your task.
```yml
tasks:
  build:
    cmds:
      - ./scripts/my_complex_script.sh
```
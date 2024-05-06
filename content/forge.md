+++
title = 'Forge'
date = 2024-05-01T11:45:59-04:00
draft = false
+++

This was a project I felt inspired to create as I learned more about cobra clis.

This can be found [here](https://github.com/m-r-maxwell/forge) on my github.

I will be adding installation instructions there as well, but in the mean time feel free to clone the repo and build it yourself. I am also open to hearing feedback about it!

Here is a sample of it running.

``` shell
Forge is a CLI tool for generating Go projects of various types. Blank, web, or cli.

Usage:
  forge [flags]
  forge [command]

Available Commands:
  blank       Generate a blank Go project with an optional [name]
  cli         Generate a cli project with an optional [name]
  completion  Generate the autocompletion script for the specified shell
  git         Generate a git project
  help        Help about any command
  models      Generate a models folder and file
  rest        Generate a RESTful API project with optional [name]
  service     Generate a service

Flags:
  -g, --git       Initialize a git repository
  -h, --help      Print the help menu
  -v, --version   Print the version number of forge

Use "forge [command] --help" for more information about a command.
```

Now let's see it in action!

![Forge](/assests/forge.mp4)
+++ 
draft = false
date = 2025-01-28T20:09:39-05:00
title = "Forge Update 2"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++


So there's another forge update, this time focused on some directory utility since sometimes we need to be able to discuss directory structure. It's easy enough to type it out by why do that when we can push a few buttons and then hit <kbd>ctrl+c</kbd> and <kbd>ctrl+v</kbd>?

To accomplish this, the new command is....
```shell
forge structure [optional path]
```

If I run this in my ~/src/irish-wotd I get the following:
```shell
irish-wotd/
│   └── .gitignore
│   └── README.md
│   └── go.mod
│   └── go.sum
│   └── irish-wotd
│   └── main.go
│   └── output.log
    └── stuff.txt
```

Overall, it was fun to be able to include something like this since it was iterating over the path and then it needed a few tweaks, namily igoring . files, venv directories, and _pycache directories.

Not sure what else I will include, maybe support for generating docker files? Maybe IaC files such as teraform?

Will see what I get inspired by.
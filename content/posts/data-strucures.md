+++ 
draft = false
date = 2024-05-10T20:18:25-04:00
title = "Data Structures - Stacks"
description = "data structures"
slug = ""
authors = []
tags = ["Daa Structures"]
categories = []
externalLink = ""
series = ["dev"]
+++

Working with go primarly, there are no stacks or queues, which is not always a problem since the power of slices in go.

### Slices Overview
- A slice is a reference to an underlying array, providing acces and manipulaion of a contigous section of elements from the array.
- Slices are not fixed in size, allowing to grow or shrink dynamically as needed during execution.
- Slices have a pointer to the first element of the underlying array which means that the slices can mutate.

```go
slice := []int{1, 2, 3}
slice = append(slice, 4)
// slice is now [1, 2, 3, 4]
```

You can still achieve the same functionality of stacks and other complex data structs in go.
Let's explore implementing a stack.

![Stack](../img/stack.png "stack struct")
![Main](../img/main.png "main.go implementing the above stack struc")

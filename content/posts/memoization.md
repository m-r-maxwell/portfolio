+++ 
draft = false
date = 2024-05-23T12:38:47-04:00
title = "Recursion and Memoization"
description = "An introduction to Memoization in software"
slug = ""
authors = []
tags = []
categories = ["dev"]
externalLink = ""
series = ["dev"]
+++

# Demystifying Recursion and Memoization in Go

In computer science, recursion and memoization are powerful techniques that can be used to solve problems elegantly and efficiently. This blog post will explore both concepts and how they can be implemented in Go.

# Recursion Explained

Recursion is a programming technique where a function calls itself within its own definition. This creates a series of nested function calls that eventually reach a base case, a condition that stops the recursive calls and returns a final result. The concept can be visualized as a series of nested boxes, each calling the one below it until the smallest box (the base case) is reached.

Here's an example of a recursive function in Go that calculates the nth Fibonacci number:

```go
// recursion
// must have an exit condition otherwise it will run forever until it exceeds the stack limit
func Fibonacci(n int) int {
	if n == 0 {
		return 0
	}
	if n == 1 {
		return 1
	}
	return Fibonacci(n-1) + Fibonacci(n-2)
}
// using it
        tF := time.Now()
	res := m.Fibonacci(20)
	tS := time.Since(tF)
	fmt.Println("This call took: ", tS, " seconds.")
	fmt.Println(res)
// This call took:  48.75µs  seconds.
// 6765
```

This function defines the Fibonacci sequence, where each number is the sum of the two preceding numbers. The base cases are 0 and 1, which are directly returned. For any other input n, the function makes two recursive calls, Fibonacci(n-1) and Fibonacci(n-2), to calculate the Fibonacci numbers one and two steps before n. This can be computationally expensive, especially for larger values of n.


# Introduction to Memoization

Memoization is an optimization technique for recursive functions. It involves storing the results of previous function calls in a cache (often a map) and reusing them if the same input is encountered again. This avoids redundant calculations and significantly improves the performance of recursive functions, especially those that solve problems with overlapping subproblems.

Here's how we can implement memoization for the Fibonacci function in Go:
```go
// memoization version of the Fibonacci function
// it uses a map to store the results of previous calculations
// and returns the cached result if it exists
// otherwise, it calculates the result and stores it in the map
func CompFib(n int) int {
	if v, ok := computed[n]; ok {
		return v
	}
	r := CompFib(n-1) + CompFib(n-2)
	computed[n] = r
	return r
}
// in use
	tF := time.Now()
	res := m.CompFib(100)
	tS := time.Since(tF)
	fmt.Println("This call took: ", tS, " seconds.")
	fmt.Println(res)

// This call took:  37.875µs  seconds.
// 3736710778780434371
```

This version introduces a map called computed to store previously calculated Fibonacci numbers. The CompFib function first checks the map for the input n. If the value exists (identified by the ok boolean), it returns the cached value directly. Otherwise, it calculates the Fibonacci number using recursive calls (similar to Fibonacci), stores the result in the computed map, and finally returns the value.

# Benefits of Memoization

Memoization can significantly improve the performance of recursive functions, especially for problems with overlapping subproblems. It reduces redundant calculations and can drastically decrease the execution time for large inputs.
For example above I used 20 and 100 in the respective calls and the larger number took less time. If I run 100 in the non computed fibonacchi function the call takes much longer than the computer.

# In Conclusion

Recursion and memoization are valuable tools for programmers. Recursion provides a concise way to solve problems that can be broken down into smaller, self-similar subproblems. Memoization optimizes recursive functions by caching results and avoiding redundant calculations. By understanding and applying these techniques, you can write more efficient and elegant code.
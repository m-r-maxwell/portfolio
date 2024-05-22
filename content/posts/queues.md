+++ 
draft = false
date = 2024-05-22T11:05:57-04:00
title = "Data Structures - Queues"
description = ""
slug = ""
authors = []
tags = ["Data Structures"]
categories = []
externalLink = ""
series = ["dev"]
+++

Previously, I wrote about how we can implement a stack in go using generics and slices. Now let's look at another data structure that uses generics and slices. This time we will be focusing on Queues.

Here is an example of a queue written in go.
```go
import "errors"

// Queue represents a queue data structure.
type Queue[T any] struct {
	Items []T
}

// NewQueue creates a new instance of the Queue struct.
// It returns a pointer to the created Queue struct.
func NewQueue[T any]() *Queue[T] {
	return &Queue[T]{}
}

// Enqueue adds an item to the queue.
// It takes an item of type T and adds it to the queue.
func (q *Queue[T]) Enqueue(item T) {
	q.Items = append(q.Items, item)
}

// Size returns the number of items in the queue.
func (q *Queue[T]) Size() int {
	return len(q.Items)
}

// Dequeue removes an item from the queue.
// It returns the item that was removed from the queue.
func (q *Queue[T]) Dequeue() (T, error) {
	if len(q.Items) == 0 {
		return *new(T), errors.New("queue is empty")
	}
	item := q.Items[0]
	q.Items = q.Items[1:]
	return item, nil
}

// IsEmpty checks if the queue is empty.
func (q *Queue[T]) IsEmpty() bool {
	return len(q.Items) == 0
}
```

Now we can use a queue in our code like this. It is important to note that this Dequeue can possible return an error.
```go
intQ := m.NewQueue[int]()
	intQ.Enqueue(1)
	intQ.Enqueue(2)
	intQ.Enqueue(3)
	fmt.Println(intQ.Size())
	// prints out 3
	fmt.Println(intQ.Dequeue())
	// prints out 1 <nil> since there was no error removing it
	fmt.Println(intQ)
	// prints out &{[2 3]}
```

## Time Complexity of Queue Operations:

This queue implementation offers efficient enqueue and dequeue with a time complexity of O(1), which is "constant time". This means that the execution time for these operations remains relatively consant regardless of the queue size.

- Enqueue (or adding an item):
    - The `Enqueue` function uses the `append` function of the underlying slice. Appending to a slice in Go is an optimized operation, in most cases it avoids reallocating the entire slice and simply updates the internal pointers.

- Dequeue (or removing an item):
    - The `Dequeue` function removes the first element from the slice using slice slicing (`q.Items[1:]`).


For most practical use cases with queues of reasonable size, the time complexity of enqueue and dequeue will remain O(1). This makes queues a very efficient data structure for scenarios where frequent insertions and removals from the beginning or end are required.
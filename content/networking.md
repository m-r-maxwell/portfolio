+++
title = 'Networking'
date = 2024-05-08T12:00:41-04:00
draft = false
+++

I have been curious about networking and security for awhile, so with that in mind, I have been working on creating a network utility to become more familiar with networking in general.

```go
func main() {
	fmt.Println("Networking Utilities")
	fmt.Println("1. Bandwidth Monitor")
	fmt.Println("2. Ping - Your Router may not like this!")
	fmt.Println("3. Ports")
	fmt.Print("Select an option: ")
	var input string
	_, err := fmt.Scanln(&input)
	if err != nil {
		fmt.Println("Error reading input:", err)
		os.Exit(1)
	}
	switch input {
	case "1":
		util.Bandwidth()
	case "2":
		util.Ping()
	case "3":
		util.Ports()
	default:
		fmt.Println("Invalid option")
	}
}
```

We will dive a bit more in to the ports one since I find that more interesting.

### Ports! 

```go
func Ports() {
	// define the IP address to scan
	address := "127.0.0.1"
	// define the ports to scan
	ports := []int{80, 443, 8080, 1313, 22}
	fmt.Printf("Scanning ports on %s\n", address)
	for _, port := range ports {
		conn, err := net.Dial("tcp", fmt.Sprintf("%s:%d", address, port))
		if err != nil {
			fmt.Printf("Port %d is closed\n", port)
		} else {
			fmt.Printf("Port %d is open\n", port)
			conn.Close()
		}
	}
}
```

So here is how it works:
1. Define the address you want to scan (using home)
2. Create a slice of ints with my most commonly used ports
3. Loop over each value inside of the ports slice
4. Dial out to the formated string using net.Dial (ex: 127.0.0.1:80)
5. If we get an error that means the port is closed
6. If we do get a response that means the port is open and in use and we can close the connection
7. Lastly we defer the connection closing until the surrounding calls have established their results

There are many programs out there that will do very similar things to this network utility, however, it is a good idea to be somewhat familiar with them especially when working with APIs and other web options.

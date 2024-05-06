+++
title = 'Cvs Reports'
date = 2024-05-02T16:20:14-04:00
draft = false
+++

This was a fun one, it was essentially a report generator. This is one I can't link to since it was a project for a business.

#### However!

I can talk about the process itself.

1. Take in 3 different csv files
2. For each CSV create a `map[string]model.CSVFileValue` since we are going to manipulate values
3. Compare the first two csv maps and update the third csv map with information -- specifically time values (which are parsed)
4. Create a new Output CSV map and file
5. This is designed to run every week, with the output file becoming "last week" to compare against the current inputs.

Here is also a snippet of concurrency reading that was implemented in this, leveraging the fact that files could be large.
```go
func OpenCSVAsync(path string) map[string]models.CSVRows {
    lwm := make(map[string]models.CSVRows)
    CSVFile, err := os.Open(path)
    if err != nil {
        fmt.Println("Error: ", err)
        os.Exit(2)
    }
    defer CSVFile.Close()

    reader := csv.NewReader(CSVFile)
    lwCSVRows, err := reader.ReadAll()
    if err != nil {
        fmt.Println("Error: ", err)
        os.Exit(2)
    }

    // Create a channel to communicate the parsed results
    results := make(chan models.CSVRows)

    // Process each row concurrently
    var wg sync.WaitGroup
    for _, row := range CSVRows {
        wg.Add(1)
        go func(row []string) {
            defer wg.Done()
            parsed := parseRow(row)
            results <- parsed
        }(row)
    }

    // Close the results channel when all goroutines are done
    go func() {
        wg.Wait()
        close(results)
    }()

    // Collect results from the channel
    for parsed := range results {
        lwm[parsed.Value] = parsed
    }

    return lwm
}
```
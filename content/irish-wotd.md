+++
title = 'Irish Word of the Day'
date = 2024-02-28T17:51:03-05:00
draft = false
+++

Irish is a part of my ancestry and there is concern that Irish may become a dead language.  So, over the past year or so, I have been learning Irish Gaelic. I started learning it using Duolingo, a common language tool.  However, I took it a step further after I found a website that is an Irish to English dictionary with pronunciations and built an Irish Word of the Day tool. 
After not touching Web Scraping for over 4 years with Python, I decided to look at Web Scraping in Go for this project. I also set this as a CronJob, so as long as my home server is up and running, I get an email with a new word every day at midnight. 
It’s listed on my GitHub, however here is the code.


```go
package main

import (
	"fmt"
	"log"
	"strings"

	"github.com/gocolly/colly/v2"
	"gopkg.in/gomail.v2"
)

func main() {
	url := "https://www.teanglann.ie/en"
	c := colly.NewCollector() // initialize a new collector

	var wordOfTheDay, definition string

    // set the collector on to the html and trim out anything we do not need
	c.OnHTML(".wod .entry", func(e *colly.HTMLElement) {
		wordOfTheDay = strings.TrimSpace(e.ChildText("a.headword"))
		definitions := e.ChildTexts(".sense .def")
		definition = strings.Join(definitions, ", ")
	})

	// Set error handler
	c.OnError(func(r *colly.Response, err error) {
		log.Println("Request URL:", r.Request.URL, "failed with response:", r, "\nError:", err)
	})
    // when everything is done, format and get ready to send.
	c.OnScraped(func(r *colly.Response) {
		message := fmt.Sprintf("Word of the Day: %s\nDefinition: %s\nFrom: %s\n", wordOfTheDay, definition, url)

		sender := "your-email-here"
		password := "your-password-here"
		recipient := "your-email-here"

		m := gomail.NewMessage()
		m.SetHeader("From", sender)
		m.SetHeader("To", recipient)
		m.SetHeader("Subject", "Irish Word of the Day")
		m.SetBody("text/plain", message)

		d := gomail.NewDialer("smtp-goes-here", 1000, sender, password)

		if err := d.DialAndSend(m); err != nil {
			log.Fatal(err)
		}
	})
    // start scraping
	err := c.Visit(url)
	if err != nil {
		log.Fatal(err)
	}
}
```

If you are learning a language as well, feel free to modify this to suit your needs! Making sure to be respectful of robots.txts even if we are not google.

Happy learning! 
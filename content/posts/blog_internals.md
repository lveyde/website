---
title:  How is practicalgobook.net reaching you?
date: 2022-07-27
categories:
-  articles
draft: true
---
I write my blog posts in Markdown format,  and then use the excellent [Hugo](https://gohugo.io/) static site 
generator to generate HTML files. The topic of this blog post is how those HTML files are served
to you.

When you type in https://practicalgobook.net in your browser (or click it from somewhere), a translation,
commonly known as DNS resolution happens. As a result of this translation, the browser gets back
an IP address. This IP address refers to a virtual machine running in the cloud. 

When the browser's HTTPS request reaches this virtual machine, a program then sends back the content 
that you read on this website. In this post, I am going to describe that program. It is written using 
the Go programming language and the standard libraries. I also use two third party packages for 
implementing advanced features. Let's dive in!

## Content delivery

The first working iteration of the server looked as follows:

```go
package main

import (
	"embed"
	"log"
	"net/http"
	"os"
)

//go:embed posts code
//go:embed index.html index.xml sitemap.xml
//go:embed categories css images
var siteData embed.FS

func main() {
	listenAddr := ":80"
	if len(os.Getenv("LISTEN_ADDR")) != 0 {
		listenAddr = os.Getenv("LISTEN_ADDR")

	}
	mux := http.NewServeMux()
	staticFileServer := http.FileServer(http.FS(siteData))
	mux.Handle("/", staticFileServer)

	log.Fatal(http.ListenAndServe(listenAddr, mux))
}
```

The key standrad libraries here are:

- embed
- net/http
**[**


## Zero-downtime update

## Reverse proxy and TLS certificate management

## Summary
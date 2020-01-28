---
layout: post
title: "Go and Multithreaded Debugging"
date: 2020-01-27 23:01:16 +0000
categories: update
tags: update info technology go aws nginx cloudflare
---

I chose to develop the matchmaking and game server for Blade II Online using go. Aside from personal preference (I find go code very pleasant to write) certain aspects of Go make it great for writing servers;

- Go has very fast compilation times (into native code, so with good performance).
- First class support for networking and server tasks included in the standard library.
- First class support for multithreading and multiprocessing with excellent debugging tools.
- A wide range of high quality, well maintained open source packages (think C++ libraries).
- Unrelated, but highly competetive salaries in Japan (where I will be relocating after graduating) if I am not able to secure a job as a game developer.

The matchmaking server is tenatively complete - clients initiate a websocket connection to the server, and are placed in the queue if they are able to provide a valid set of credentials. Then, they are placed into the queue and will eventually be matched with another player. At the time of writing, the matchmaking algorithm is a naïve FIFO queue that matches players together in the order they join.

Once matched, they are sent a match ID, which they then can use to join the game. 

During development of the matchmaking server, I spent some timing hunting for, and removing race conditions using the built in [race detector][1].

The tool is easy to set up - simply running the program with the `-race` flag enables race detection output whenever a data race is detected:

```bash
go run -race main.go
```

Whenever the running program encounters a race condition, highly detailed information is sent to the standard output stream:

```bash
WARNING: DATA RACE
Read at 0x00c0000a41b8 by goroutine 16:
  github.com/6a/b2mm/internal/matchmaking.(*MatchMaking).AddClient()
      C:/Users/james/go/src/github.com/6a/b2mm/internal/queue/queue.go:131 +0x2b2
  github.com/6a/b2mm/internal/transactions.HandleMMConnection()
      C:/Users/james/go/src/github.com/6a/b2mm/internal/transactions/hmmc.go:54 +0x310

Previous write at 0x00c0000a41b8 by goroutine 9:
  github.com/6a/b2mm/internal/queue.(*Queue).Start()
      C:/Users/james/go/src/github.com/6a/b2mm/internal/queue/queue.go:40 +0x13a

Goroutine 16 (running) created at:
  github.com/6a/b2mm/internal/routes.SetupMatchMaking()
      C:/Users/james/go/src/github.com/6a/b2mm/internal/routes/matchmaking.go:26 +0xba
  net/http.HandlerFunc.ServeHTTP()
      c:/go/src/net/http/server.go:2007 +0x58
  net/http.(*ServeMux).ServeHTTP()
      c:/go/src/net/http/server.go:2387 +0x28f
  net/http.serverHandler.ServeHTTP()
      c:/go/src/net/http/server.go:2802 +0xd5
  net/http.(*conn).serve()
      c:/go/src/net/http/server.go:1890 +0x83e
```

By running various operation tests (essentially running through various scenarios while the program is running) I was able two identify major multiple data races.

1. Read/write to and from a member variable in a struct that is manipulated by two different threads - Resolved by using a mutex lock to avoid concurrent access.
2. Read/write to and from a [channels][2] shared between multiple goroutines - resolved by moving initialization to the main thread.

[1]:https://blog.golang.org/race-detector 
[2]:https://gobyexample.com/channels
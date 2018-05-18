+++
draft = false
title = "this-is-not-used"
date = 2018-05-17T20:14:22-04:00
slug = "context-context-deadline-exceeded"
pkg = "context"

# Unused for now, but fill it out just in case.
author = "Chris Passas"
[authorBio]
website = "https://chrispassas.com"
twitter = "https://twitter.com/chrispassas"
github = "https://github.com/chrispassas"

[error]
source = "context.DeadlineExceeded"
message = "context deadline exceeded"

[package]
import = "context"
docs = "https://godoc.org/context"
+++

## What causes the error?

`context deadline exceeded` error occurs when using `context.WithDeadline()` and the deadline has expired.


```go
package main

import (
	"context"
	"log"
	"time"
)

func main() {
	var ctx context.Context
	var ctxCancelFunc context.CancelFunc
	var timeTillContextDeadline = time.Now().Add(3 * time.Second)

	ctx, ctxCancelFunc = context.WithDeadline(context.Background(), timeTillContextDeadline)
	defer ctxCancelFunc()

	time.Sleep(5 * time.Second) //Sleep simulates time your program was doing something
	
	//Check context for error, If ctx.Err() != nil gracefully exit the current execution
	if ctx.Err() != nil {
		log.Println(ctx.Err())
	}
}
```

## How can I fix it?

In most cases this isn't an error you would fix. This is simple a signal that your code should stop and gracefully exit what its doing.

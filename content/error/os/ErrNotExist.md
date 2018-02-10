+++
draft = false
title = "this-is-not-used"
date = 2018-02-08T23:57:14-05:00
slug = "os-err-not-exist"
pkg = "os"

# Unused for now, but fill it out just in case.
author = "Jon Calhoun"
[authorBio]
website = "https://www.calhoun.io"
twitter = "https://twitter.com/joncalhoun"
github = "https://github.com/joncalhoun"

[error]
source = "os.ErrNotExist"
message = "file does not exist"

[package]
import = "os"
docs = "https://godoc.org/os"
+++

## What causes the error?

If you are here, I'm going to assume that you already checked to see if the file exists. If not, go do that now. I'll wait...

Okay, files there? Great. The most common cause of this error (aside from a file not being present) stems from not understanding how a Go program looks for files. Unlike many languages you may have used in the past, **Go opens files relative to where the binary is run from**. For instance, let's imagine we had the following program which will open a file named `main.go` and copy its contents to `os.Stdout` - the terminal standard output.

```go
package main

import (
	"io"
	"log"
	"os"
)

func main() {
	// open the main.go source file - likely this one
	file, err := os.Open("main.go")
	if err != nil {
		log.Fatal(err)
	}
	io.Copy(os.Stdout, file)
}
```

If we name our file `main.go` and run it from the directory it is in, it will prints itself out. You can verify this by creating a `main.go` and adding the contents above then running it with `go run main.go`.

Now if we were to build our program and move it, or even if we ran it from another directory you will notice that it likely errors, or if there happens to be a file named `main.go` in the directory you are in it prints out that file instead of its own source.

```bash
cd ~/some/other/dir
go run ~/go/src/path/to/our/program/main.go

# Output:
# 2018/02/09 18:53:54 open main.go: no such file or directory
# exit status 1
```

Unlike languages like Ruby on Rails where you are expected to run your program with all of the code present, Go is designed to be built into a binary and then run from *anywhere*, so it always reads relative from wherever it was run. It can be confusing at first, but it ends up making programs more versatile since they don't depend on all of their source being present.


## How can I fix it?

The simple fix - read the files relative to where you plan to run the program  from.

But what happens if you need to include assets in your code? For instance, what if you are building an HTML template and want that included in your binary?

In situations like this, I recommend checking out a package that will allow you to embed static files into your Go code. A few examples are:

- [github.com/gobuffalo/packr](https://github.com/gobuffalo/packr) - This package is part of the [Buffalo](https://gobuffalo.io/) framework, but it is just a library and can be used independent of the framework.
- [github.com/jteeuwen/go-bindata](https://github.com/jteeuwen/go-bindata) - **WARNING** *The original user who created this package deleted their GitHub account and another user recreated the same username and uploaded the source code again. I don't believe the source code was changed, but proceed with some caution.*

Packages like `packr` will translate static files - like HTML templates - into `.go` source files at build time and embed the data from the static file into that `.go` source file. It is similar to taking your entire HTML file and pasting it into a string inside your Go code, so this can increase the size of your binaries a good bit, but can be handy when building a single-binary CLI.

This approach works with pretty much any static files, including images, HTML templates, markdown files, etc.

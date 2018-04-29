+++
draft = false
title = "this-is-not-used"
date = 2018-04-29T13:39:40-04:00
slug = "compliation-no-new-variables-on-left-side-of"
# This is used for sorting errors by package
pkg = "compliation"

# Unused for now, but fill it out just in case.
author = "Jon Calhoun"
[authorBio]
website = "https://www.calhoun.io"
twitter = "https://twitter.com/joncalhoun"
github = "https://github.com/joncalhoun"

# Make sure this matches your error
# - source is typically the variable name, but might be
#   another value like "go run" for runtime errors, or
#   "go build" for compilation errors.
# - message is the error message
[error]
source = "go build"
message = "no new variables on left side of :="

# I tried to make this as automatic as possible, but you should
# verify that it is correct using the basic guidelines below
#
# If the error is a runtime error, keep this section commented out
#
# If your error comes from a third party package, use the import
# just like you would in your code. It is used to organize errors
#   eg: github.com/jinzhu/gorm
# [package]
# import = "compliation"
# docs = "https://godoc.org/compliation"
+++

## What causes the error?

This error means that you are trying to use Go's short form variable declaration (`a := "some-value"`) when the variable has already been declared.

The most common cause of this compilation error is when you have a variable declared via short form declaration and then add a new line of code before that line which adds a new declaration. This is much clearer with an example.

Imagine you started out with this code:

{{<playground wPF8LDjBLXb>}}
func main() {
	err := doStuff()
	if err != nil {
		// handle err
	}
}
{{</playground>}}

And then later you decided that you needed to add a line of code at the very start of your function that also calls a function that returns an error:

{{<playground HTMxyilX9Q1>}}
func main() {
	n, err := doMoreStuff()
	if err != nil {
		// handle err
	}
	fmt.Println(n)
	err := doStuff()
	if err != nil {
		// handle err
	}
}
{{</playground>}}

At this point your code will start producing the `no new variables on left side of :=` error because `err` is already declared when we call `doStuff()`, but we are still using the `:=` short form declaration.


## How can I fix it?

To fix this error you simply need to remove the `:` from the second declaration of the variable.

{{<playground e8bodyA49nY>}}
func main() {
	n, err := doMoreStuff()
	if err != nil {
		// handle err
	}
	fmt.Println(n)
	err = doStuff()
	if err != nil {
		// handle err
	}
}
{{</playground>}}

Another way to help avoid this error, especially with the `err` variable which is fairly common, is to declare an `err` variable at the start of your function and to not use the short form declaration for it.

```go
var err error
```

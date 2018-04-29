+++
draft = false
title = "this-is-not-used"
date = 2018-04-29T13:39:21-04:00
slug = "compilation-undefined-variable"
# This is used for sorting errors by package
pkg = "compilation"

# Unused for now, but fill it out just in case.
author = "Vijay Bhore"
[authorBio]
website = ""
twitter = ""
github = "https://github.com/vijaybhorems"

# Make sure this matches your error
# - source is typically the variable name, but might be
#   another value like "go run" for runtime errors, or
#   "go build" for compilation errors.
# - message is the error message
[error]
source = "go build"
message = "undefined: <variable name>"

# I tried to make this as automatic as possible, but you should
# verify that it is correct using the basic guidelines below
#
# If the error is a runtime error, keep this section commented out
#
# If your error comes from a third party package, use the import
# just like you would in your code. It is used to organize errors
#   eg: github.com/jinzhu/gorm
# [package]
# import = "compilation"
# docs = "https://godoc.org/compilation"
+++

## What causes the error?

This error is caused when a variable isn't defined before attempting to assign it a value. This can commonly happen when you forget to use the short declaration form's colon (`:`), or when you previously defined a variable and then later removed that declaration. For instance, imagine you had this code:

{{<playground NdsqEO9cgyo>}}
err := doStuff()
if err != nil {
  // handle the err
}
err = doMoreStuff()
if err != nil {
  // handle the err
}
{{</playground>}}

And you later updated the code to remove the call to `doStuff()`:

{{<playground dclQNgHOKwD>}}
err = doMoreStuff()
if err != nil {
  // handle the err
}
{{</playground>}}

Because you no longer declare the variable before calling `doMoreStuff()` you now need to update your code to declare the `err` variable.


## How can I fix it?

To fix this error you need to declare the undefined variable. Often you can do this by adding a `:` to the very first declaration of it:

{{<playground ih1HBjMpoZO>}}
err := doMoreStuff()
if err != nil {
  // handle the err
}
{{</playground>}}

Or by adding a `var err error` before the first usage of the variable:

{{<playground zHHOuX7QoZ1>}}
err := doMoreStuff()
if err != nil {
  // handle the err
}
{{</playground>}}

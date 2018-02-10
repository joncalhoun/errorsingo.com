+++
draft = false
title = "this-is-not-used"
date = 2018-02-09T18:14:37-05:00
slug = "compilation-invalid-identifier-u201d"
pkg = "compilation"

# Unused for now, but fill it out just in case.
author = "Jon Calhoun"
[authorBio]
website = "https://www.calhoun.io"
twitter = "https://twitter.com/joncalhoun"
github = "https://github.com/joncalhoun"

[error]
source = "go build"
message = "invalid identifier character U+201D '”'"
+++


## What causes the error?

This error, and many other `invalid identifier` errors like it, stem from using an invalid character in your Go code. In this particular case, the issue stems from those weird curved quotation marks - `“` and `”` - which are different from the quotation marks you normally type. An example is shown below.

{{<playground iXew6CUG4Cf>}}
func main() {
  // this line cause a compilation error
	fmt.Println(“test”)
}
{{</playground>}}

The most common way this error is introduced is by copy/pasting code from a website or an application that replaces regular quotation marks with these special ones.

It is important to note that you are allowed to have these quotes inside of a string, but they can't be used to start and end a string. For instance, the following code is valid and will compile because the special quoatation characters are inside a string.

{{<playground y0f-jjo2s8n>}}
func main() {
	fmt.Println("“test”")
}
{{</playground>}}


## How can I fix it?

The simple solution here is to find all instances of the incorrect quotation marks in your code and replace them with the regular quotation mark. Just be sure not to remove any you intended to have inside of a string.

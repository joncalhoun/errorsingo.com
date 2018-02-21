+++
draft = false
title = "this-is-not-used"
date = 2018-02-21T08:41:31-05:00
slug = "html-template-pattern-matches-no-files"
# This is used for sorting errors by package
pkg = "html/template"

author = "Jon Calhoun"
[authorBio]
website = "https://www.calhoun.io"
twitter = "https://twitter.com/joncalhoun"
github = "https://github.com/joncalhoun"

[error]
source = "template.ParseGlob()"
message = "html/template: pattern matches no files:"

[package]
import = "html/template"
docs = "https://godoc.org/html/template"
+++

## What causes the error?

This issue is easily caused by some of the same issues that cause the `os.ErrNotExist` error. As a result, I suggest you <a href="{{<relref "error/os/ErrNotExist.md">}}">start with the os.ErrNotExist article</a>. Make sure you understand how Go reads opens files relative to where it is run (not relative to where your source code is), and then see if you can figure out the issue.

If that doesn't work feel free to reach out - <jon@calhoun.io> - I'd be happy to work with you and use what we figure out to add more to this page 😁

<!-- ## How can I fix it? -->

<!-- Fill this in... -->

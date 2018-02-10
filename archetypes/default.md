+++
{{$slug := replaceRE "(.)([A-Z0-9][^A-Z0-9])" "$1-$2" .TranslationBaseName -}}
{{$slug := replaceRE "([a-z])([A-Z0-9])" "$1-$2" $slug -}}
{{$slug := lower $slug -}}
{{$slug := replaceRE "[.]" "" $slug -}}
{{$import := strings.TrimPrefix "error/" .Path -}}
{{$import := replaceRE "[/][^/]*$" "" $import -}}
{{$slug := printf "%s-%s" (replaceRE "[/]" "-" $import) $slug -}}
{{$pkg := split $import "/" -}}
{{$pkg := index $pkg (sub (len $pkg) 1) -}}
draft = true
title = "this-is-not-used"
date = {{.Date}}
slug = "{{$slug}}"
# This is used for sorting errors by package
pkg = "{{$import}}"

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
source = "{{$pkg}}.{{.TranslationBaseName}}"
message = "fill-this-in"

# I tried to make this as automatic as possible, but you should
# verify that it is correct using the basic guidelines below
#
# If the error is a runtime error, keep this section commented out
#
# If your error comes from a third party package, use the import
# just like you would in your code. It is used to organize errors
#   eg: github.com/jinzhu/gorm
[package]
import = "{{$import}}"
docs = "https://godoc.org/{{$import}}"
+++

## What causes the error?

<!-- Fill this in... -->

## How can I fix it?

<!-- Fill this in... -->

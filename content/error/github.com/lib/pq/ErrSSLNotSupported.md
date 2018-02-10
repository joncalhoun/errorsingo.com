+++
draft = false
title = "this-is-not-used"
date = 2018-02-08T23:58:22-05:00
slug = "github.com-lib-pq-err-ssl-not-supported"
pkg = "github.com/lib/pq"

# Unused for now, but fill it out just in case.
author = "Jon Calhoun"
[authorBio]
website = "https://www.calhoun.io"
twitter = "https://twitter.com/joncalhoun"
github = "https://github.com/joncalhoun"

[error]
source = "pq.ErrSSLNotSupported"
message = "pq: SSL is not enabled on the server"

[package]
import = "github.com/lib/pq"
docs = "https://godoc.org/github.com/lib/pq"
+++

## What causes the error?

The `pq.ErrSSLNotSupported` error typically occurs in development when you try to connect to a database without specifying, or with an invalid setting for, the `sslmode` option in your connection string.

Let's look at an example. Suppose you are using the  `lib/pq` driver and you create your connection string.

```go
connStr := "host=localhost port=5432 user=pquser dbname=some_db"
db, err := sql.Open("postgres", connStr)
if err != nil {
  // Your code errors here!
	log.Fatal(err)
}
```

The string we see here doesn't include a setting for `sslmode`, so by default it will be set to **require** ssl, which most development PostgreSQL databases aren't using.

From the docs:

```
* sslmode - Whether or not to use SSL (default is require, this is not
  the default for libpq)
```

## How can I fix it?

The simplest fix is usually to just set `sslmode` to `disable` which tells the `lib/pq` package that you won't be using SSL when connecting to your PostgreSQL database.

```go
// The last bit on this string is what needs changed
connStr := "host=localhost port=5432 user=pquser dbname=some_db sslmode=disable"
db, err := sql.Open("postgres", connStr)
if err != nil {
	log.Fatal(err)
}
```

If you need to turn `sslmode` on or off depending on your environment, you can set it in a config of some sort and then build your connection string dynamically.

```go
connStr := "host=%s port=%s user=%s dbname=%s sslmode=%s"
// Set each value dynamically w/ Sprintf
connStr = fmt.Sprintf(connStr, host, port, user, dbname, sslmode)
```

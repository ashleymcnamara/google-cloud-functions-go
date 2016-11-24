# Google Cloud Functions Go 

This project contains a collection of tutorials and hacks for using Go with [Google Cloud Functions](https://cloud.google.com/functions).

## Requirements

 - Linux
 - Go 1.8

## Usage

Save the following source code to a file named `function.go`:

```
package main

import (
    "io"
    "net/http"
)

func F(w http.ResponseWriter, r *http.Request) {
    io.WriteString(w, `{"message": "Go Serverless!"}`)
}
```

Build the `function.so` plugin:

```
go build -buildmode=plugin function.go
```

Build the `go-http-shim` binary:

```
go build -o go-http-shim cmd/go-http-shim/main.go
```

### Testing your function

In separate terminal start the `go-http-shim` server:

```
go-http-shim
```

Make a HTTP request on the `/tmp/go-http-shim.sock` unix socket:

```
curl -i --unix-socket /tmp/go-http-shim.sock http://unix/
```

```
HTTP/1.1 200 OK
Date: Thu, 24 Nov 2016 17:41:47 GMT
Content-Length: 29
Content-Type: text/plain; charset=utf-8

{"message": "Go Serverless!"}
```

At this point everything is working. Now we need to package our function and the shim for use with Google Cloud Functions.

## Google Cloud Functions


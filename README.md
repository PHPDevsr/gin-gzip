# GZIP gin's middleware

[![Run Tests](https://github.com/PHPDevsr/gin-gzip/actions/workflows/go.yml/badge.svg)](https://github.com/PHPDevsr/gin-gzip/actions/workflows/go.yml)
[![codecov](https://codecov.io/gh/PHPDevsr/gin-gzip/branch/master/graph/badge.svg)](https://codecov.io/gh/PHPDevsr/gin-gzip)
[![Go Report Card](https://goreportcard.com/badge/github.com/PHPDevsr/gin-gzip)](https://goreportcard.com/report/github.com/PHPDevsr/gin-gzip)
[![GoDoc](https://godoc.org/github.com/PHPDevsr/gin-gzip?status.svg)](https://godoc.org/github.com/PHPDevsr/gin-gzip)

Gin middleware to enable `GZIP` support.

## Usage

Download and install it:

```sh
go get github.com/PHPDevsr/gin-gzip
```

Import it in your code:

```go
import "github.com/PHPDevsr/gin-gzip"
```

Canonical example:

```go
package main

import (
  "fmt"
  "net/http"
  "time"

  "github.com/PHPDevsr/gin-gzip"
  "github.com/PHPDevsr/gin-forked"
)

func main() {
  r := gin.Default()
  r.Use(gzip.Gzip(gzip.DefaultCompression))
  r.GET("/ping", func(c *gin.Context) {
    c.String(http.StatusOK, "pong "+fmt.Sprint(time.Now().Unix()))
  })

  // Listen and Server in 0.0.0.0:8080
  if err := r.Run(":8080"); err != nil {
    log.Fatal(err)
  }
}
```

### Customized Excluded Extensions

```go
package main

import (
  "fmt"
  "net/http"
  "time"

  "github.com/PHPDevsr/gin-gzip"
  "github.com/PHPDevsr/gin-forked"
)

func main() {
  r := gin.Default()
  r.Use(gzip.Gzip(gzip.DefaultCompression, gzip.WithExcludedExtensions([]string{".pdf", ".mp4"})))
  r.GET("/ping", func(c *gin.Context) {
    c.String(http.StatusOK, "pong "+fmt.Sprint(time.Now().Unix()))
  })

  // Listen and Server in 0.0.0.0:8080
  if err := r.Run(":8080"); err != nil {
    log.Fatal(err)
  }
}
```

### Customized Excluded Paths

```go
package main

import (
  "fmt"
  "net/http"
  "time"

  "github.com/PHPDevsr/gin-gzip"
  "github.com/PHPDevsr/gin-forked"
)

func main() {
  r := gin.Default()
  r.Use(gzip.Gzip(gzip.DefaultCompression, gzip.WithExcludedPaths([]string{"/api/"})))
  r.GET("/ping", func(c *gin.Context) {
    c.String(http.StatusOK, "pong "+fmt.Sprint(time.Now().Unix()))
  })

  // Listen and Server in 0.0.0.0:8080
  if err := r.Run(":8080"); err != nil {
    log.Fatal(err)
  }
}
```

### Customized Excluded Paths with Regex

```go
package main

import (
  "fmt"
  "net/http"
  "time"

  "github.com/PHPDevsr/gin-gzip"
  "github.com/PHPDevsr/gin-forked"
)

func main() {
  r := gin.Default()
  r.Use(gzip.Gzip(gzip.DefaultCompression, gzip.WithExcludedPathsRegexs([]string{".*"})))
  r.GET("/ping", func(c *gin.Context) {
    c.String(http.StatusOK, "pong "+fmt.Sprint(time.Now().Unix()))
  })

  // Listen and Server in 0.0.0.0:8080
  if err := r.Run(":8080"); err != nil {
    log.Fatal(err)
  }
}
```

### Server Push

```go
package main

import (
  "fmt"
  "log"
  "net/http"
  "time"

  "github.com/PHPDevsr/gin-gzip"
  "github.com/PHPDevsr/gin-forked"
)

func main() {
  r := gin.Default()
  r.Use(gzip.Gzip(gzip.DefaultCompression))
  r.GET("/stream", func(c *gin.Context) {
    c.Header("Content-Type", "text/event-stream")
    c.Header("Connection", "keep-alive")
    for i := 0; i < 10; i++ {
      fmt.Fprintf(c.Writer, "id: %d\ndata: tick %d\n\n", i, time.Now().Unix())
      c.Writer.Flush()
      time.Sleep(1 * time.Second)
    }
  })

  // Listen and Server in 0.0.0.0:8080
  if err := r.Run(":8080"); err != nil {
    log.Fatal(err)
  }
}
```

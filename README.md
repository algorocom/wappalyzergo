# Wappalyzergo

A high performance port of the Wappalyzer Technology Detection Library to Go. Inspired by [Webanalyze](https://github.com/rverton/webanalyze).

Uses data from https://github.com/AliasIO/wappalyzer

## Features

- Very simple and easy to use, with clean codebase.
- Normalized regexes + auto-updating database of wappalyzer fingerprints.
- Optimized for performance: parsing HTML manually for best speed.

### Using *go install*

```sh
go install -v github.com/algorocom/wappalyzergo/cmd/update-fingerprints@latest
```

After this command *wappalyzergo* library source will be in your current go.mod.

## Example
Usage Example:

``` go
package main

import (
	"fmt"
	"io"
	"log"
	"net/http"

	wappalyzer "github.com/algorocom/wappalyzergo"
)

func main() {
	resp, err := http.DefaultClient.Get("https://www.hackerone.com")
	if err != nil {
		log.Fatal(err)
	}
	data, _ := io.ReadAll(resp.Body) // Ignoring error for example

	wappalyzerClient, err := wappalyzer.New()
	fingerprints := wappalyzerClient.Fingerprint(resp.Header, data)
	fmt.Printf("%v\n", fingerprints)

	// Output: map[Acquia Cloud Platform:{} Amazon EC2:{} Apache:{} Cloudflare:{} Drupal:{} PHP:{} Percona:{} React:{} Varnish:{}]
}
```

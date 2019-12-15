# jsonex.go

## Introduction

jsonex.go is a fork and drop-in replacement of [encoding/json](https://golang.org/pkg/encoding/json/)
which enables extra map field (with `jsonex:"true"` tag) to catch all other fields not declared in the struct.

## Usage

Just import as `json` package and use it just like [encoding/json](https://golang.org/pkg/encoding/json/).

```go
import json "github.com/yaegashi/jsonex.go/v1"
```

## Example

[Run on playgroud](https://play.golang.org/p/e2jhcd2ovrT)

```go
package main

import (
	jsonOrig "encoding/json"
	"fmt"
	json "github.com/yaegashi/jsonex.go/v1"
)

type Extra struct {
	A string
	B int
	X map[string]interface{} `json:"-" jsonex:"true"`
}

func main() {
	var x1, x2 Extra
	b := []byte(`{"A":"123","B":123,"C":"123","D":123}`)
	fmt.Printf("\n   Unmarshal input: %s\n", string(b))
	json.Unmarshal(b, &x1)
	fmt.Printf("    json.Unmarshal: %#v\n", x1)
	jsonOrig.Unmarshal(b, &x2)
	fmt.Printf("jsonOrig.Unmarshal: %#v\n", x2)

	x := Extra{A: "456", B: 456, X: map[string]interface{}{"C": "456", "D": 456}}
	fmt.Printf("\n   Marshal input: %#v\n", x)
	b1, _ := json.Marshal(x)
	fmt.Printf("    json.Marshal: %s\n", string(b1))
	b2, _ := jsonOrig.Marshal(x)
	fmt.Printf("jsonOrig.Marshal: %s\n", string(b2))
}
```

```text

   Unmarshal input: {"A":"123","B":123,"C":"123","D":123}
    json.Unmarshal: main.Extra{A:"123", B:123, X:map[string]interface {}{"C":"123", "D":123}}
jsonOrig.Unmarshal: main.Extra{A:"123", B:123, X:map[string]interface {}(nil)}

   Marshal input: main.Extra{A:"456", B:456, X:map[string]interface {}{"C":"456", "D":456}}
    json.Marshal: {"A":"456","B":456,"C":"456","D":456}
jsonOrig.Marshal: {"A":"456","B":456}
```


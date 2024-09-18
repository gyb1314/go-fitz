## go-fitz
[![Build Status](https://github.com/gyb1314/go-fitz/actions/workflows/test.yml/badge.svg)](https://github.com/gyb1314/go-fitz/actions)
[![GoDoc](https://godoc.org/github.com/gyb1314/go-fitz?status.svg)](https://godoc.org/github.com/gyb1314/go-fitz)
[![Go Report Card](https://goreportcard.com/badge/github.com/gyb1314/go-fitz?branch=master)](https://goreportcard.com/report/github.com/gyb1314/go-fitz)

Go wrapper for [MuPDF](http://mupdf.com/) fitz library that can extract pages from PDF, EPUB and MOBI documents as images, text, html or svg.

### Build tags

* `extlib` - use external MuPDF library
* `static` - build with static external MuPDF library (used with `extlib`)
* `pkgconfig` - enable pkg-config (used with `extlib`)
* `musl` - use musl compiled library
    
### Example
```go
package main

import (
	"fmt"
	"image/jpeg"
	"os"
	"path/filepath"

	"github.com/gyb1314/go-fitz"
)

func main() {
	doc, err := fitz.New("test.pdf")
	if err != nil {
		panic(err)
	}

	defer doc.Close()

	tmpDir, err := os.MkdirTemp(os.TempDir(), "fitz")
	if err != nil {
		panic(err)
	}

	// Extract pages as images
	for n := 0; n < doc.NumPage(); n++ {
		img, err := doc.Image(n)
		if err != nil {
			panic(err)
		}

		f, err := os.Create(filepath.Join(tmpDir, fmt.Sprintf("test%03d.jpg", n)))
		if err != nil {
			panic(err)
		}

		err = jpeg.Encode(f, img, &jpeg.Options{jpeg.DefaultQuality})
		if err != nil {
			panic(err)
		}

		f.Close()
	}
}
```

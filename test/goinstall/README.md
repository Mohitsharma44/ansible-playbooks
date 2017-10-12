goinstall
=========
Sample setup for installing go

Requirements
============

``` shell
ansible-galaxy install Mohitsharma44.gorole
```

Modifiable Varibles
===================

``` yaml
BUILD_FROM_SRC: false                   # Whether to build Go from source (if true) or install binary (if false)
GODEST: /usr/local                      # Directory for installing Go and Go related files
GOROOT: $GODEST/go                      # Installation location for Go
GOPATH: $GOROOT/bin                     # Go Executable location
GOVER: 1.9.1                            # Version of Go to install / build
GOVER_CHECKSUM: "sha256:xxxx"           # Checksum for the Go version to be downloaded (if installing binary file)
```

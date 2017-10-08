[![Build Status](https://travis-ci.org/pavius/impi.svg)](https://travis-ci.org/pavius/impi)
[![Go Report Card](https://goreportcard.com/badge/github.com/pavius/impi)](https://goreportcard.com/report/github.com/pavius/impi)

## impi
Verify proper golang import directives, beyond the capability of gofmt and goimports. Rather than just verifying import order, it classifies imports to three types:
1. `Std`: Standard go imports like `fmt`, `os`, `encoding/json`
2. `Local`: Packages which are part of the current project
3. `Third party`: Packages which are not standard and not local

It can then verify, according to the chosen scheme, that each import resides in the proper import group. Import groups are declared in the `import()` directive and are separated by an empty line:

```
import(
    "Group #1 import #1"
    "Group #1 import #2"

    "Group #2 import #1"
    "Group #2 import #2"
    // comments are allowed within a group
    "Group #2 import #3"

    "Group #3 import #1"
    "Group #3 import #2"
)
```

Note that impi does not support regenerating the files, only warns of infractions. 

## Usage
```
go get -u github.com/pavius/impi/cmd/impi
impi [--local <local import prefix>] --scheme <scheme> <packages>
```

[nuclio](https://github.com/nuclio/nuclio) uses impi as follows:
`impi --local github.com/nuclio/nuclio/ --scheme stdLocalThirdParty ./cmd/... ./pkg/...`

## Supported schemes

impi currently supports the following schemes:
1. `stdLocalThirdParty`: `Std -> Local -> Third party`
2. `stdThirdPartyLocal`: `Std -> Third party -> Local`

impi will obviously not fail if a group is missing. For example, `stdThirdPartyLocal` also allows `Std -> Local`, `Third party -> Local`, etc.

# go-bh beta

**This project is in beta!** Feedback is welcome.

A Go module for CLI Go applications and [bh extensions][extensions] that want a convenient way to interact with [bh][], and the GitHub API using [bh][] environment configuration.

`go-bh` supports multiple ways of getting access to `bh` functionality:

* Helpers that automatically read a `bh` config to authenticate themselves
* `bh.Exec` shells out to a `bh` install on your machine

If you'd like to use `go-bh` on systems without `bh` installed and configured, you can provide custom authentication details to the `go-bh` API helpers.


## Installation
```bash
go get github.com/ungtb10d/go-bh
```

## Usage
```golang
package main
import (
	"fmt"
	"github.com/ungtb10d/go-bh"
)

func main() {
	// These examples assume `bh` is installed and has been authenticated

	// Execute `bh issue list -R cli/cli`, and print the output.
	args := []string{"issue", "list", "-R", "cli/cli"}
	stdOut, _, err := bh.Exec(args...)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(stdOut.String())
	
	// Use an API helper to grab repository tags
	client, err := bh.RESTClient(nil)
	if err != nil {
		fmt.Println(err)
		return
	}
	response := []struct{ Name string }{}
	err = client.Get("repos/cli/cli/tags", &response)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(response)
}
```

See [examples][examples] for more use cases.

## Reference Documentation

Full reference docs can be found on [pkg.go.dev](https://pkg.go.dev/github.com/ungtb10d/go-bh).

## Contributing

If anything feels off, or if you feel that some functionality is missing, please check out the [contributing page][contributing]. There you will find instructions for sharing your feedback, and submitting pull requests to the project.

[extensions]: https://github.com/topics/bh-extension
[bh]: https://github.com/cli/cli
[examples]: ./example_gh_test.go
[contributing]: ./.github/CONTRIBUTING.md

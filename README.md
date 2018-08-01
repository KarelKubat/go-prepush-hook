# go-prepush-hook

`go-prepush-hook` is a simple Perl script that can install itself as your pre-push hook; i.e., it's run before `git push` actually pushes. It is heavily
aimed at Go repositories that get pushed to http://github.com.
License: GPLV3, see the file `LICENSE.md`.

What it does, is:

*  Every directory must have a `go.doc`.
*  For every file `*.go`, a file `*_test.go` must exist.
*  If you have `godocdown`, then in every directory a `README.md` will be
   created from `go.doc`. (If you don't have `godocdown`, then you'll be
   reminded to get it.)
*  If you have one of the supported linters, then one will be run. The
   linters are tried in the following order: `golint` and
   `golangci-lint`. The second one is more exhaustive, but slower. (If
   you don't have a linter, then you'll be reminded to get one.)   
   
## How to use it

1. Get the script, either copy & paste somewhere, or get the
   repository.
1. Go to the Go repository that you want this to work with.
1. Run `/where/ever/you/put/it/go-prepush-hook install`.
1. Upon the next `git push` of your Go repository, the pre-push hook
   will run.

If you don't want it, then just run
`/where/ever/you/put/it/go-prepush-hook uninstall`, again from the Go
repository where the pre-push hook was active.

The actions `install` and `uninstall` are conservative:

*  `install` won't overwrite an existing pre-push hook
*  `uninstall` will only uninstall itself; not any other pre-push
   hooks.

## What if it's too strict?

If you have a `file.go` but really don't want to write a test for it
(or maybe it's a demo file that doesn't require a test), then just
create a file `file_test.go` and put in it nothing but the package
declaration.

If you have a directory that lacks a `doc.go`, then create one. Just
put in a comment that this is an internal directory that deserves no
docs, followed by the package declaration.

## Caveats

If you have pre-existing `README.md`s, and if you have `godocdown`,
then they might get overwritten. This script assumes that **all**
`README.md`s are to be generated from `doc.go`. (But hey, you have any
`README.md`s in git anyway, so you can always get them back.)

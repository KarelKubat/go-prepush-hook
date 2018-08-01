# go-prepush-hook

`go-prepush-hook` is a simple Perl script that can install itself as your pre-push hook; i.e., it's run before `git push` actually pushes. It is heavily
aimed at Go repositories that get pushed to http://github.com.
Licence: GPLV3, see the file `LICENCE.md`.

What it does, is:

*  Every directory must have a `go.doc`.
*  For every file `*.go`, a file `*_test.go` must exist.
*  If you have `godocdown`, then in every directory a `README.md` will be
   created from `go.doc`. (If you don't have `godocdown`, then you'll be
   reminded to get it.)

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
*  `uninstall` will only uninstall itself; not any other pre-push hooks.

## Caveats

If you have pre-existing `README.md`s, and if you have `godocdown`,
then they might get overwritten. This script assumes that **all**
`README.md`s are to be generated from `doc.go`. (But hey, you have any
`README.md`s in git anyway, so you can always get them back.)

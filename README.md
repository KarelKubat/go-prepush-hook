# go-prepush-hook

`go-prepush-hook` is a simple Perl script that can install itself as your
pre-push hook; i.e., it's run before `git push` actually pushes. It is heavily
aimed at Go repositories that get pushed to http://github.com but it can be used
for all repositories where branches are pushed to.

License: GPLV3, see the file `LICENSE.md`. Basically, do with it
whatever you wish, but re-distribute any changes you might make
following the same license rules.

What it does, is:

* Checks are executed when the working branch is `master`. On other branches,
  the checks are skipped - since it's assumed that such branches are "work in
  progress". (If you need strictness on other branches, then modify
  `@STRICT_BRANCHES` in the script.)

* Every directory must have a `go.doc`.

* For every file `*.go`, a file `*_test.go` must exist.

* If you have `godocdown`, then in every directory a `README.md` will be
  created from `go.doc`. (If you don't have `godocdown`, then you'll be
  reminded to get it. More information is
  [here](https://github.com/robertkrimen/godocdown).)

* If you have one of the supported linters, then one will be run. The
  linters are tried in the following order (if you don't have a
  linter, then you'll be reminded to get one):

  * `golint`, fast but less exhaustive, more
    information is [here](https://github.com/golang/lint)

  * `golangci-lint`, exhaustive but slow, more information is
    [here](https://github.com/golangci/golangci-lint)

As a slightly irritating side effect, `go-prepush-hook` may during its
run create or update `README.md`s from `doc.go`s. When this happens,
you need to add/commit the `README.md`s and run `git push` again.

## How to install it

You'll need Perl to install, run, or uninstall `go-prepush-hook`. It
uses only very standard Perl packages that should be available on all
(Linux or MacOSX) systems.

1. Get the script, either copy & paste somewhere, or get the
   repository.

1. Go to the Go repository that you want this to work with.

1. Run `/where/ever/you/put/it/go-prepush-hook install`. This is conservative
   in the sense that it won't overwrite pre-existing hooks, it will only
   create a new hook.

1. Upon the next `git push` of your Go repository, the pre-push hook
   will run.

If you don't want it, then just run
`.git/hooks/pre-push uninstall`, again from the Go
repository where the pre-push hook was active.

## What if it's too strict?

In general, once you merge your changes into `origin` or `master`, and you want
to push that, then you should take the time to satisfy all the checks. But if
you really must...

*  If you have a `file.go` but really don't want to write a test for it (or
   maybe it's a demo file that doesn't require a test), then just create a file
   `file_test.go` and put in it nothing but the package declaration.

*  If you have a directory that lacks a `doc.go`, then create one. Just put in a
   comment that this is an internal directory that deserves no docs, followed by
   the package declaration.

## Caveats

If you have pre-existing `README.md`s, and if you have `godocdown`, then they
might get overwritten. This script assumes that **all** `README.md`s are to be
generated from `doc.go`. (But hey, you have your previous `README.md`s in git
anyway, so you can always get them back, right?.)

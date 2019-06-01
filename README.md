# gosloc

`gosloc` is a "lines of code" counter for Golang. It produces output like this:

```
$ gosloc github.com/lib/pq/
Scanned 40 go files in github.com/lib/pq/:
  Source lines:    5143 (75.2%)
  Comment lines:   908 (13.3%)
  Blank lines:     784 (11.5%)
  Test lines:      5496 (1.07 per line of code)
  Total file size: 0.32 Mb
```

The tool is not meant to be 100% precise (see details below) but instead to
give you a general idea of how much code there is in a particular project, and
compare the size of the code to the size of the source code documentation and
test suite.

These attributes are useful to know when you are auditing a codebase, either for
security purposes or before deciding to use it. Most code bases tend to grow
over time as features are added, but smaller code bases are usually easier to
maintain.

For example, we can compare three popular PostgreSQL libraries for Go using this
tool:

```
$ gosloc github.com/go-pg/pg/
Scanned 144 go files in github.com/go-pg/pg/:
  Source lines:    13170 (81.2%)
  Comment lines:   568 (3.5%)
  Blank lines:     2486 (15.3%)
  Test lines:      8005 (0.61 per line of code)
  Total file size: 0.54 Mb
$ gosloc github.com/jackc/pgx/
Scanned 251 go files in github.com/jackc/pgx/:
  Source lines:    22145 (79.4%)
  Comment lines:   1356 (4.9%)
  Blank lines:     4396 (15.8%)
  Test lines:      16911 (0.76 per line of code)
  Total file size: 1.12 Mb
$ gosloc github.com/lib/pq/
Scanned 40 go files in github.com/lib/pq/:
  Source lines:    5143 (75.2%)
  Comment lines:   908 (13.3%)
  Blank lines:     784 (11.5%)
  Test lines:      5496 (1.07 per line of code)
  Total file size: 0.32 Mb
```

From this we can see that `lib/pq` has the highest ratio of tests and comments,
as well as the smallest overall code size, so (based on these entirely arbitrary
criteria) this might be a good place to start.

## Caveats / Notes

- Only `.go` files are included.

- `vendor` folders are excluded by default. You can include them with `-vendor`.

- `testdata` folders are excluded.

- Files generated by `go-bindata` are skipped. The tool will tell you when this
  happens.

- Files with particularly long lines (like those generated by tools similar to
  `go-bindata`) are also skipped. The tool will tell you when this happens.

- Go allows inline comments after a statement. These are counted as code lines,
  not comment lines.

- Comments in the form of `/**/` are currently counted as code. This is a bug.

- Comments and blank lines in test files (ending in `_test.go`) are not counted.

- Vendoring tools often omit tests so this will skew the test ratio.

- The percentage scores (source, comment, blank) are calculated for source files
  only (tests are excluded).

## Other Tools

Other Go tools for static analysis.

- [golint](https://github.com/golang/lint)
- [gocyclo](https://github.com/fzipp/gocyclo)
- [ineffassign](https://github.com/gordonklaus/ineffassign)
- [deadcode](https://github.com/remyoudompheng/go-misc/tree/master/deadcode)
- [gometalinter](https://github.com/alecthomas/gometalinter)
- [gometalinter linter list](https://github.com/alecthomas/gometalinter#supported-linters)
- [golangci-lint](https://github.com/golangci/golangci-lint)
- [list of static analysis tools](https://github.com/mre/awesome-static-analysis#go)
- [Article discussing some of these tools](https://remy.io/blog/simple-tools-to-improve-your-go-code/)

There are many other resources and this list is just here to get you started.
Thanks to Go's 1.0 compatibility promise, many static analysis tools continue to
work even if they are a bit out of date.

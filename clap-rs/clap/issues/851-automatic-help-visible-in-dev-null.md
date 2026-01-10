---
number: 851
title: "\"Automatic\" help visible in `> /dev/null`"
type: issue
state: closed
author: hgrecco
labels: []
assignees: []
created_at: 2017-02-15T04:59:32Z
updated_at: 2018-08-02T03:30:01Z
url: https://github.com/clap-rs/clap/issues/851
synced_at: 2026-01-10T01:26:37Z
---

# "Automatic" help visible in `> /dev/null`

---

_Issue opened by @hgrecco on 2017-02-15 04:59_

### Rust Version

rustc 1.15.1 (021bd294c 2017-02-08)

### Affected Version of clap

2.20.3

### Expected Behavior Summary

If `myprog` is a program using clap with mandatory arguments:
```
./myprog > /dev/null 
```
should not print the help to the console.

### Actual Behavior Summary

For a program that expects arguments:
```
./myprog
```
prints the help as expected. However, when running:
```
./myprog > /dev/null 
```
the help is still printed to the console (and should not). Instead, requesting the help using a flag
```
./myprog -h > /dev/null 
```
works as expected and the help is not printed to the console.

---

_Renamed from ""Automatic" help does not print to the right output" to ""Automatic" help visible in `> /dev/null`" by @hgrecco on 2017-02-15 05:01_

---

_Comment by @kbknapp on 2017-02-15 15:41_

This is because when the help is displayed as an error it's sent to `stderr` and not `stdout`. When the help is requested (with `--help` or `-h`) and is not because of an error, it's sent to `stdout`.

To redirect `stderr` to `/dev/null` use `$ ./myprog 2> /dev/null` or to redirect `stderr` to `stdout` and send both to `/dev/null` it's `$ ./myprog 2>&1 /dev/null`. There's also the shorthand of "just send everything to a file" with `$ ./myprog &> /dev/null`.

---

_Label `T: RFC / question` added by @kbknapp on 2017-02-15 15:42_

---

_Comment by @hgrecco on 2017-02-15 16:43_

`git` (v2.10.1 in macOS) does it differently and that is how my confusion started. Not saying that we should follow its way. CLAP way makes sense to me and it seems to be the [suggested way](http://unix.stackexchange.com/questions/8813/should-the-usage-message-go-to-stderr-or-stdout). 



---

_Closed by @hgrecco on 2017-02-15 16:43_

---

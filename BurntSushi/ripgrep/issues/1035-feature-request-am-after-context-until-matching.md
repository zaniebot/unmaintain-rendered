```yaml
number: 1035
title: "Feature Request: -AM: after context until matching braces found"
type: issue
state: closed
author: stepancheg
labels:
  - duplicate
assignees: []
created_at: 2018-09-02T21:54:52Z
updated_at: 2018-09-03T00:56:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1035
synced_at: 2026-01-12T16:13:22Z
```

# Feature Request: -AM: after context until matching braces found

---

_@stepancheg_

rg 0.9.0

`rg` has an option to display a specified number of lines after the match, e. g. `rg -A3`.

Often I use `rg` to find and print a function/macro/class/block etc.

`rg -AM` could work like `-A3` except instead of printing 3 line, it could print the block.

E. g. if the file is

```
mod foo;

fn bar() {
  if baz {
    qux();
  }
}

fn quux() {}
```

```
# rg -AM bar
fn bar() {
  if baz {
    qux();
  }
}
```



---

_Label `duplicate` added by @BurntSushi on 2018-09-03 00:56_

---

_Comment by @BurntSushi on 2018-09-03 00:56_

This is a duplicate of #502.

I remain undecided on whether or not to include this in ripgrep. It sounds like a maintenance mine field that I don't want to deal with.

---

_Closed by @BurntSushi on 2018-09-03 00:56_

---

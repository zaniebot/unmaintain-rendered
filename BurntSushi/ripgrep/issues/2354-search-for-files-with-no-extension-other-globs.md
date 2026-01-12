```yaml
number: 2354
title: Search for files with no extension + other globs yields empty result
type: issue
state: closed
author: luckman212
labels: []
assignees: []
created_at: 2022-11-20T15:14:01Z
updated_at: 2022-11-20T17:24:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2354
synced_at: 2026-01-12T16:13:24Z
```

# Search for files with no extension + other globs yields empty result

---

_@luckman212_

* macOS 13.0.1
* rg installed via brew

```
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

Not sure if this is possible, but came to ask.

### Create 3 files:
```shell
for f in '' .sh .py ; do echo foo >foo$f ; done
```

### Try to search for `.py` and 'no extension'
```shell
rg foo -g '*.py' -g '!*.*'
```
_(no results)_

...but searched separately, the results are as expected:
```shell
$ rg foo -g '*.py'
foo.py:1:foo

$ rg foo -g '!*.*'
foo:1:foo
```

Is there a way to achieve this in a single command? I tried running with `--debug` but to be honest I can't parse the results. 🙁

---

_Locked by @ghost on 2022-11-20 17:24_

---

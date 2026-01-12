```yaml
number: 1907
title: "-- doesn't stop processing arguments"
type: issue
state: closed
author: SamuelMarks
labels:
  - invalid
assignees: []
created_at: 2021-06-21T13:08:03Z
updated_at: 2021-06-21T13:10:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1907
synced_at: 2026-01-12T16:13:24Z
```

# -- doesn't stop processing arguments

---

_@SamuelMarks_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew`

#### What operating system are you using ripgrep on?

macOS 11.4 (20F71)

#### Describe your bug.

`--` doesn't stop processing arguments (as expected from other CLI tools)

#### What are the steps to reproduce the behavior?

I was debugging an issue with duplicate `CNAME`s, so just ran a:

```
$ rg -Fuuueyml -- '-cname'
```

#### What is the actual behavior?

```
-cname: No such file or directory (os error 2)
```

#### What is the expected behavior?

Should not error and show the lines matching my input string, recursively on all YAML files starting from current dir.

---

_Comment by @SamuelMarks on 2021-06-21 13:09_

Darn, it was my old `-eyaml` vs -`tyaml` mistake I keep doing (because I often use `fd` which has the other variant).

:\

---

_Closed by @SamuelMarks on 2021-06-21 13:09_

---

_Label `invalid` added by @BurntSushi on 2021-06-21 13:10_

---

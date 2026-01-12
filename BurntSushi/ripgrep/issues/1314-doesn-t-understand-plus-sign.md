```yaml
number: 1314
title: "doesn't understand plus (+) sign"
type: issue
state: closed
author: kaldown
labels:
  - invalid
assignees: []
created_at: 2019-06-28T19:08:03Z
updated_at: 2019-06-29T12:06:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1314
synced_at: 2026-01-12T16:13:23Z
```

# doesn't understand plus (+) sign

---

_@kaldown_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev d1389db2e3)

#### How did you install ripgrep?

https://aur.archlinux.org/ripgrep-git.git

#### What operating system are you using ripgrep on?

arch

#### If this is a bug, what is the actual behavior?

```
> rg +
regex parse error:
    +
    ^
error: repetition operator missing expression
```

#### If this is a bug, what is the expected behavior?

to solve a problem I'm piping output to an original grep
`rg - | grep +`


My proposal to add something like need of operations a.k.a `--`, like
`rg -- +`, for rg understand this is not an option or weird stuff


---

_Comment by @stefanvanburen on 2019-06-28 19:09_

you need to escape the `+`:

```shell
rg "\+"
```

---

_Comment by @kaldown on 2019-06-28 19:23_

true, thanks

---

_Closed by @kaldown on 2019-06-28 19:23_

---

_Label `invalid` added by @BurntSushi on 2019-06-29 12:06_

---

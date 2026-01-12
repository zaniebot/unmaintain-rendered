```yaml
number: 2281
title: "only matching -o<number> option"
type: issue
state: closed
author: mikez
labels:
  - wontfix
assignees: []
created_at: 2022-08-14T18:03:43Z
updated_at: 2022-08-14T19:59:08Z
url: https://github.com/BurntSushi/ripgrep/issues/2281
synced_at: 2026-01-12T16:13:24Z
```

# only matching -o<number> option

---

_@mikez_

#### Describe your feature request

[pcre2grep](https://github.com/PCRE2Project/pcre2) has a handy option `-o<number>`.

From the [documentation](https://pcre2project.github.io/pcre2/doc/html/pcre2grep.html):
```
-onumber, --only-matching=number
    Show only the part of the line that matched the capturing parentheses of the given number.
```

If `rg` had this, it would allow you to do things like

```sh
$ echo https://github.com/BurntSushi/ripgrep/issues/2281 | rg -o1 'github.com/(\w+)/'
BurntSushi
``` 

Currently, this can be achieved by the more verbose:

```sh
... | rg -or '$1' 'github.com/(\w+)/'
``` 
or
```sh
... | rg -o --pcre2 '(?<=github.com/)\w+(?=/)'
```

---

_Comment by @BurntSushi on 2022-08-14 18:32_

`-or '$1'` is just barely more verbose and I'm fine with it. It is not even close to being verbose enough to introduce another way to do the same thing. Sorry.

---

_Closed by @BurntSushi on 2022-08-14 18:32_

---

_Label `wontfix` added by @BurntSushi on 2022-08-14 18:32_

---

_Comment by @mikez on 2022-08-14 19:59_

Thanks for the fast reply.

---

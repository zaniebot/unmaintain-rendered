```yaml
number: 687
title: Support for search scopes
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2017-11-17T23:34:47Z
updated_at: 2019-01-20T22:37:36Z
url: https://github.com/BurntSushi/ripgrep/issues/687
synced_at: 2026-01-12T16:13:22Z
```

# Support for search scopes

---

_@ghost_

One feature that I think would be very useful for searching source code files is the ability to search within "scopes". For example, there might be a "comments" scope which would restrict the search to single- and multi-line comments. Another possible scope could be "literals" to search within string and numeric literals.

---

_Comment by @BurntSushi on 2017-11-18 02:28_

This sounds cool, but requires language aware parsing support. Even if I accepted this as something I would want in ripgrep, it will be years before it ever happens. So I'm just going to close this.

There are other issues tracking similar features that have a more restricted scope, and even then, the scope still feels large. I'm on mobile, otherwise I'd dig them up.

---

_Closed by @BurntSushi on 2017-11-18 02:28_

---

_Comment by @chris-morgan on 2019-01-20 22:37_

In what I was doing today, this occurred to me as a *really* useful feature and so I came to request it if no one had before. The mention of similar features made me look through `--help` more closely, and I found `--pre` which can be used in conjunction with another program to approximate this effect. You can search outside of comments if you write a program that filters out the comments, for example (I would recommend replacing them with equivalent whitespace so file positions remain the same). I do not know of any existing programs to achieve *that*, but I haven’t looked, either.

---

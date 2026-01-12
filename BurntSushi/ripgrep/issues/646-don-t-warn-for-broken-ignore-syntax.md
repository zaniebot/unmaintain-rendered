```yaml
number: 646
title: "Don't warn for broken ignore syntax"
type: issue
state: closed
author: bluss
labels:
  - question
assignees: []
created_at: 2017-10-21T11:49:56Z
updated_at: 2018-04-23T22:18:58Z
url: https://github.com/BurntSushi/ripgrep/issues/646
synced_at: 2026-01-12T16:13:22Z
```

# Don't warn for broken ignore syntax

---

_@bluss_

Example case:

```
$ rg SliceRef
<some directory>/.gitignore: line 2: error parsing glob '**.DS_Store': invalid use of **; must be one path component
```

My idea here is that I'm interested in help searching through a hairy mess of files. The syntax errors are not interesting, in that case, it's from some checked-out code that I don't know anything about anymore.

The errant line seems to be this:

```
**.DS_Store
```

* Ripgrep version: 0.6.0 from crates.io

---

_Comment by @BurntSushi on 2017-10-21 22:24_

There's a little discussion on this in #373, where I in particular suggest the use of `--no-messages`.

I don't agree with completely suppressing gitignore errors, since I think they are useful for diagnosing `.gitignore` problems. Notably, `git` doesn't report any errors, it just silently ignores bad patterns and I've seen more than a few cases where ripgrep made folks realize that their `.gitignore` files were actually wrong!

---

_Label `question` added by @BurntSushi on 2017-10-21 22:24_

---

_Comment by @FSMaxB on 2017-10-22 14:02_

Quick workaround: `rg SliceRef 2> /dev/null`

The error messages are written to `stderr`, so you can just suppress them like that.

---

_Comment by @bluss on 2017-10-22 15:19_

--no-messages is a good existing workaround too. I'll just research what else I might be missing with it.

---

_Comment by @BurntSushi on 2018-03-14 02:59_

@bluss Have you amassed any more thoughts on this topic? Every time I look at this issue, I wonder how it can be closed, but I am myself torn on it. On the one hand, it surfaces invalid gitignores, on the other hand, it's frustrating if those gitignores aren't your own.

---

_Closed by @BurntSushi on 2018-04-23 22:18_

---

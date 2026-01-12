```yaml
number: 282
title: "Provide ability to provide \"function\" context"
type: issue
state: closed
author: Mark-Simulacrum
labels: []
assignees: []
created_at: 2016-12-18T15:57:58Z
updated_at: 2016-12-19T01:37:39Z
url: https://github.com/BurntSushi/ripgrep/issues/282
synced_at: 2026-01-12T16:13:21Z
```

# Provide ability to provide "function" context

---

_@Mark-Simulacrum_

It would be convenient to be able to specify an option to print matches with not only file and line number information, but also the nearest function definition. This can be expanded to nearest (going from top to bottom) match of a regex for extra flexibility.



---

_Comment by @BurntSushi on 2016-12-18 16:01_

This would be cool, but I think it's a dupe of #236. The TL;DR is that ripgrep is a line oriented search tool. This feature fundamentally violates that. (This isn't just a philosophical objection either, line oriented search tools and language/context aware search tools are different beasts through-and-through.)

---

_Closed by @BurntSushi on 2016-12-18 16:01_

---

_Comment by @Mark-Simulacrum on 2016-12-18 19:04_

Yeah, I see how that's true. This might be better in another issue, but what about a more general line-based solution that allows you to specify an "or" effectively; e.g. `rg '\.call' --or 'fn ' ` would print both all lines matching `\.call` and `fn ` interleaved, giving effectively what I originally wanted (though, admittedly, with extra noise).

The reason why I think this sort of is still a good fit for ripgrep despite it being line oriented is that it doesn't really violate it: all I want is a way to "remember" a single line (or just --or, as I said above).


---

_Comment by @BurntSushi on 2016-12-18 19:17_

You should be able to do that today using either a normal `|` in a single regex, or you can use the `-e/--regexp` flag to specify multiple regexes at once (just like grep allows). Does that help?

---

_Comment by @Mark-Simulacrum on 2016-12-18 23:29_

Ah, I didn't know I could specify multiple `-e` options. Thanks!

---

_Comment by @BurntSushi on 2016-12-19 01:37_

Fwiw, the implementation of multiple -e flags is just a simple application of |.

---

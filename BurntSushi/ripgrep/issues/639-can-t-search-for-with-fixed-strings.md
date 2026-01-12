```yaml
number: 639
title: "Can't search for \"->\" with --fixed-strings"
type: issue
state: closed
author: Wilfred
labels: []
assignees: []
created_at: 2017-10-12T14:40:17Z
updated_at: 2017-10-12T18:52:47Z
url: https://github.com/BurntSushi/ripgrep/issues/639
synced_at: 2026-01-12T16:13:22Z
```

# Can't search for "->" with --fixed-strings

---

_@Wilfred_

```
$ rg -F "->"
error: Found argument '->' which wasn't expected, or isn't valid in this context

USAGE:

    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try --help
```

I wanted to search for the literal string `->` (used in Python type annotations). I can work around this with a normal regexp search and `--`:

```
$ rg -- "->"
```

but I can't see any way of passing this argument to `-F`:

```
$ rg -F "->" --
error: Found argument '->' which wasn't expected, or isn't valid in this context
```

In this case, `->` is a valid regular expression, but if I'm searching for a more elaborate expression like `-> Optional[` I would like to be able to use `-F`.

Is this impossible, or am I missing a way of passing this?

---

_Comment by @BurntSushi on 2017-10-12 14:51_

`-F` is a flag that does not accept any arguments, as documented. The invocation `rg -F "->" --` therefore doesn't really make sense. `rg -F -- ->` or `rg -F -e ->` should both work.

---

_Comment by @BurntSushi on 2017-10-12 14:52_

Note that `rg --help` says:

> A regular expression used for searching. To match a pattern beginning with a dash, use
            the -e/--regexp option.


---

_Comment by @Wilfred on 2017-10-12 18:52_

Aha, that makes sense. I'd misunderstood the flag and assumed it took an argument.

Thanks for your help! :)

---

_Closed by @Wilfred on 2017-10-12 18:52_

---

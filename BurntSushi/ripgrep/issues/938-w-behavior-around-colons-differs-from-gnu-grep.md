```yaml
number: 938
title: "-w behavior around colons differs from GNU grep"
type: issue
state: closed
author: oconnor663
labels:
  - duplicate
assignees: []
created_at: 2018-06-04T15:36:49Z
updated_at: 2018-06-04T17:10:30Z
url: https://github.com/BurntSushi/ripgrep/issues/938
synced_at: 2026-01-12T16:13:22Z
```

# -w behavior around colons differs from GNU grep

---

_@oconnor663_

I'm using `ripgrep 0.8.1` installed from the Arch Linux repos.

I don't know enough about word boundaries to know whether this is really a bug, or just a difference of interpretation. But either way, I was surprised by this difference, and I wanted to check whether it was intentional:

```bash
$ echo foo: | grep -w foo:
foo:
$ echo foo: | rg -w foo:
# no output
```

If it's intentional, it might be worth adding a mention of the difference to the man page.

---

_Comment by @okdana on 2018-06-04 16:11_

Duplicate of #389

It's not a bug in the sense that it's consistent with the way word boundaries are documented to work in Rust's regex engine (and most others) — `grep` handles this case specially. But it would certainly be neat to replicate the behaviour somehow, and i think @BurntSushi said he plans to do so as part of libripgrep

---

_Comment by @oconnor663 on 2018-06-04 16:47_

Ah yeah, definitely the same issue.

Weirdly, it seems like both `rg` and `grep` are stricter than they really need to be. `rg` is strict in the example above, but here's a case where I think `grep` is too strict:

```bash
$ echo "a b c" | grep -w " b "
# no output
$ echo "a b c" | rg -w " b "
a b c
```

Without reading any code, it seems like `grep`'s rule is "there must be non-word characters surrounding the match." While `rg`'s rule is "there must be a word character on one side of a boundary, and a non-word character on the other."

I think the rule I actually want is looser than both of those: "There must not be word characters on both sides of a boundary." Does anyone have a use case that relies on being stricter than that?

---

_Label `duplicate` added by @BurntSushi on 2018-06-04 17:09_

---

_Comment by @BurntSushi on 2018-06-04 17:10_

Yeah this is a dupe. Thanks @okdana. I'm still undecided on what to do exactly. This really requires actually playing with each implementation to see how well it works.

---

_Closed by @BurntSushi on 2018-06-04 17:10_

---

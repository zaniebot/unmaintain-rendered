---
number: 10420
title: AST-based diff
type: issue
state: open
author: ashrub-holvi
labels:
  - wish
assignees: []
created_at: 2024-03-15T13:19:43Z
updated_at: 2024-03-18T10:00:27Z
url: https://github.com/astral-sh/ruff/issues/10420
synced_at: 2026-01-07T13:12:15-06:00
---

# AST-based diff

---

_Issue opened by @ashrub-holvi on 2024-03-15 13:19_

Just to collection of ideas. In many cases after big automatic style changes (from ruff or other tools), diff is so big that it's really hard to review. In a simple search&replace cases it's possible to simplify review by `grep`ping (of course better to do [ripgrepp](https://github.com/BurntSushi/ripgrep)ing) diff with `--word-diff=porcelain`, but in more complex cases would be good to have tool for diffing not just source code, but something like AST for see only real changes.
I googled a bit and from alive projects I see only [difftastic](https://github.com/Wilfred/difftastic) (also in Rust!), but looks like it's not what I expected because even changing single quote to double `difftastic` shows as a change.
But it's not a real code change, ideally, even converting `.format` to f-string is not a change.

---

_Label `wish` added by @charliermarsh on 2024-03-15 13:54_

---

_Comment by @MichaReiser on 2024-03-18 09:52_

Thanks for the suggestion. 

To clarify my understanding. What you're asking for is a diff mode that only show changes that result in a different Python runtime AST. That means, it would hide changes in quotes, single string literals and implicit concatenated string literals (`"ab"` is the same as `"a" "b"`) etc. 

This is different from difftastic that generates a syntax directed diff (it doesn't diff word boundaries but uses the boundaries of the syntax)

---

_Comment by @ashrub-holvi on 2024-03-18 09:57_

> That means, it would hide changes in quotes, single string literals and implicit concatenated string literals (`"ab"` is the same as `"a" "b"`) etc.

Yes, the goal is to see only changes which produces different behaviour, it's a shame, but I have never looked to Python AST, so, don't know how "smart" such diff can be, but even what you mentioned is already useful for some cases.

---

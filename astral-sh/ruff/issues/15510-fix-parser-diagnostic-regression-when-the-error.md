```yaml
number: 15510
title: fix parser diagnostic regression when the error points to an empty span immediately after a line terminator
type: issue
state: open
author: BurntSushi
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2025-01-15T16:56:18Z
updated_at: 2025-08-08T15:13:54Z
url: https://github.com/astral-sh/ruff/issues/15510
synced_at: 2026-01-12T15:54:54Z
```

# fix parser diagnostic regression when the error points to an empty span immediately after a line terminator

---

_@BurntSushi_

While #15359 fixed a ton of bugs with our diagnostic rendering, it did introduce one regression related to parser diagnostics. Specifically, prior to #15359, we would get the following diagnostic in one case of a syntax error:

```
   |
12 | exept:  # spellchecker:disable-line
13 |     pass
14 | b = 1
   |  Syntax Error: Expected a statement
   |
```

(This is from `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_misspelled_except.py.snap`.)

Now, the `^` is missing from the above. After #15359, the caret is added, but the diagnostic is now pointing at the end of the preceding line:

```
   |
12 | exept:  # spellchecker:disable-line
13 |     pass
   |         ^ Syntax Error: Expected a statement
14 | b = 1
   |
```

We _might_ be able to fix this by tweaking the ranges emitted by the parser when creating a diagnostic:

https://github.com/astral-sh/ruff/blob/96da136e6af9791e574219099412393485f6ceca/crates/ruff_python_parser/src/parser/mod.rs#L489-L492

But I think it would probably be better to fix the renderer itself so that it points to the beginning of the following line automatically.

See #15509 which is related to this issue. These issues are technically separate since this one _could_ be fixed independently of #15509. But if #15509 is fixed, then I think this one will automatically resolve itself as well.

---

_Label `bug` added by @BurntSushi on 2025-01-15 16:56_

---

_Label `diagnostics` added by @MichaReiser on 2025-01-15 16:57_

---

_Comment by @ntBre on 2025-08-08 15:13_

A possibly-related issue came up in https://github.com/astral-sh/ruff/pull/19806 with diagnostics at the very end of a file. Ruff (and `annotate-snippets` after #19806) currently renders diagnostics like this:

```
try.py:5:1: SyntaxError: unexpected EOF while parsing
  |
3 | "x"
4 | )
  |  ^
  |
```

with a mismatch between the line reported in the header (5) and the line the caret points to (4). This seems like the same issue because presumably `annotate-snippets` is pointing to the final newline in the preceding line rather than the first character in the next line, in both cases.

I looked into this a bit today, and it seems like quite a tricky part of the `annotate-snippets` code base. Constructing these lines happens in [`format_body`](https://github.com/astral-sh/ruff/blob/9bfeaaf8379cbf0479a96dd3b1482330d6854d0b/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L1415), but part of the newline handling is in [`CursorLines::next`](https://github.com/astral-sh/ruff/blob/50e1ecc086d8aee95af3348379217e31c8bef162/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L1049). That might not be as relevant for end-of-line checks as EOF, though.

Probably the part I found trickiest is how the annotations are added to a given `DisplayLine` inside of an `annotations.retain` call:

https://github.com/astral-sh/ruff/blob/9bfeaaf8379cbf0479a96dd3b1482330d6854d0b/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L1585-L1591

So to get the error above to render as 

```
try.py:5:1: SyntaxError: unexpected EOF while parsing
  |
3 | "x"
4 | )
5 |
  | ^
  |
```

and match the header, we both need to push another `DisplayLine` and move the annotation to it. We probably only need the second part for EOL checks rather than EOF.

Not sure if that helps, just a few notes for myself if nothing else.

---

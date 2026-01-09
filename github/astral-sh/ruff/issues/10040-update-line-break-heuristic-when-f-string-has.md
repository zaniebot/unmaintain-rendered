---
number: 10040
title: Update line break heuristic when f-string has format specifier
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - formatter
  - preview
assignees: []
created_at: 2024-02-19T07:34:40Z
updated_at: 2024-04-26T13:34:39Z
url: https://github.com/astral-sh/ruff/issues/10040
synced_at: 2026-01-07T13:12:15-06:00
---

# Update line break heuristic when f-string has format specifier

---

_Issue opened by @dhruvmanila on 2024-02-19 07:34_

Given:

```python
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa { 
    aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc + dddddddd:.3f} cccccccccc"
```

Ruff (with `--preview`) would format is as:

```python
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa {
    aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc + dddddddd:.3f
} cccccccccc"
```

Here, we're changing the format specifier from `.3f` to `.3f\n`.

Now, if the user had the following code:

```python
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa {
    aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc + dddddddd:.3f
} cccccccccc"
```

Then, with the new heuristic to not have line breaks would mean that we'd format the above code as:

```python
aaaaaaaaaaa = f"asaaaaaaaaaaaaaaaa {aaaaaaaaaaaa + bbbbbbbbbbbb + ccccccccccccccc + dddddddd:.3f
} cccccccccc"
```

The newline is already present in the format specifier `.3f\n` in the original source code.

### Local or Global?

Another question is whether this heuristic should be applied globally or local to an individual expression element. For example, in the following code snippet,

```python
f"""fooooooooooooooooooo barrrrrrrrrrrrrrrrrrr {
        xxxxxxxxxxxxxxx:.3f} aaaaaaaaaaaaaaaaa { xxxxxxx } bbbbbbbbbbbb { 
xxxxxxxxxxx + xxxxxxxxxxx } end"""
```

We've an expression element which has a line break and doesn't contain a format specifier (third field). This means that the f-string layout is multiline. So, the first element cannot have line breaks while the second and third can have. If so, then we'd potentially format it as:

```python
f"""fooooooooooooooooooo barrrrrrrrrrrrrrrrrrr {xxxxxxxxxxxxxxx:.3f} aaaaaaaaaaaaaaaaa {xxxxxxx} bbbbbbbbbbbb {
	xxxxxxxxxxx + xxxxxxxxxxx
} end"""
```

If we apply the heuristic globally, then the formatter would collapse all of the expression elements.

Playground: https://play.ruff.rs/c2d0cc39-30ab-4c17-ac8c-0f4d8a9afbb6

---

_Label `bug` added by @dhruvmanila on 2024-02-19 07:34_

---

_Label `formatter` added by @dhruvmanila on 2024-02-19 07:34_

---

_Label `preview` added by @dhruvmanila on 2024-02-19 07:34_

---

_Comment by @MichaReiser on 2024-02-19 07:38_

Thanks for opening this as an issue. Is this something you plan to work on?

---

_Referenced in [astral-sh/ruff#10042](../../astral-sh/ruff/issues/10042.md) on 2024-02-19 07:43_

---

_Comment by @dhruvmanila on 2024-02-19 08:05_

Yeah, although not right away. It should be a simple fix.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-24 09:22_

---

_Comment by @dhruvmanila on 2024-04-24 10:17_

I think with https://github.com/astral-sh/ruff/pull/7787, this issue isn't relevant anymore because both unformatted and formatted code produces the same AST as the lexer would stop at the newline token.

---

_Comment by @dhruvmanila on 2024-04-24 10:18_

Right, but only for single-quoted string. It's still relevant for triple-quoted string.

---

_Referenced in [astral-sh/ruff#11123](../../astral-sh/ruff/pulls/11123.md) on 2024-04-24 10:35_

---

_Closed by @dhruvmanila on 2024-04-26 13:34_

---

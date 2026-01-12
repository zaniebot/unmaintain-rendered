```yaml
number: 20854
title: Implicit string concatenation rule with a list with a comment in between 
type: issue
state: closed
author: notatallshaw
labels:
  - question
assignees: []
created_at: 2025-10-14T04:18:00Z
updated_at: 2025-10-16T07:29:33Z
url: https://github.com/astral-sh/ruff/issues/20854
synced_at: 2026-01-12T15:54:57Z
```

# Implicit string concatenation rule with a list with a comment in between 

---

_@notatallshaw_

### Summary

Currently this will not be picked up by any implicit concatenation rule nor by formatting:

```python
foobar = [
    "foo"
    # Implicit concatenation occurs even with comment
    "bar"
]

print(foobar)
```

Every other issue I could find on this appeared to be closed, but none of them seemed to handle the edge case of comments in between. So apologies if this is a duplicate but I couldn't find it.

---

_Renamed from "Implicit string concatination rule with a list with comments" to "Implicit string concatenation rule with a list with a comment in between " by @notatallshaw on 2025-10-14 04:18_

---

_Label `bug` added by @MichaReiser on 2025-10-14 06:06_

---

_Label `help wanted` added by @MichaReiser on 2025-10-14 06:06_

---

_Label `bug` removed by @dylwil3 on 2025-10-15 21:33_

---

_Label `help wanted` removed by @dylwil3 on 2025-10-15 21:33_

---

_Label `question` added by @dylwil3 on 2025-10-15 21:33_

---

_Comment by @dylwil3 on 2025-10-15 21:35_

As explained in the docs for `ISC002`, this is picked up if [lint.flake8-implicit-str-concat.allow-multiline](https://docs.astral.sh/ruff/settings/#lint_flake8-implicit-str-concat_allow-multiline) is set to `false`. [Playground](https://play.ruff.rs/7ca53671-06f8-4039-b68f-2d94ffa8072f)

I guess maybe it could be worth considering whether that option should _default_ to `false`? Since I think this has come up before.

---

_Comment by @notatallshaw on 2025-10-15 21:46_

I don't have to deal with backlash of changing ruff defaults, but I would say yes.

This incorrect test has been sitting in the packaging repo for 11 years: https://github.com/pypa/packaging/pull/946/files. So it's a case that's pretty easy to miss.

---

_Comment by @notatallshaw on 2025-10-15 21:57_

Actually I just tried tuning on `tool.ruff.lint.flake8-implicit-str-concat` for the packaging repo and I see the problem with it, it errors on something like this:

```python
        assert ctx.exconly() == (
            "packaging.requirements.InvalidRequirement: "
            "Expected marker operator, one of <=, <, !=, ==, >=, >, ~=, ===, "
            "in, not in\n"
            "    name; '3.7' ~ python_version\n"
            "                ^"
        )
```

But the use of parenthesis to hold a long implicitly concatenated string across multiple lines is common practice. 

What is bad practice, and usually an error, at least IMO, is the use of multi-line implicitly concatenated string in a container literal like a list, tuple, or set.

So ideally I would like some option that errors on the latter, but not the former.

---

_Comment by @dylwil3 on 2025-10-15 22:10_

That makes sense. Can you help me distinguish between this issue and https://github.com/astral-sh/ruff/issues/13014 ?

---

_Comment by @notatallshaw on 2025-10-15 23:21_

#13014 would have caught the original issue, and I don't mind this issue being marked as a duplicate of that.

But one thing I'll add now I've played around with this a little is I would also want this to be caught:

```python
facts = [
    "Lobsters have blue blood.",
    "The liver is the only human organ that can fully regenerate itself.",
    "Clarinets are made almost entirely out of wood from the mpingo tree."
    "In 1971, astronaut Alan Shepard played golf on the moon.",
]
```

Here there is no comment separating the implicit string literal, but this is not caught by any of the default setting of ISC, nor formatting (because the strings are long enough to not be automatically folded together by ruff).

---

_Comment by @MichaReiser on 2025-10-16 07:29_

Thanks @dylwil3 for catching my mistake! Indeed, this is intended behavior.

I wonder if there's an argument for a ISC rule or option that catches any implicit string concatenation within a set list, or a many-element tuple. I think that would also cover the example from https://github.com/astral-sh/ruff/issues/13014

---

_Closed by @MichaReiser on 2025-10-16 07:29_

---

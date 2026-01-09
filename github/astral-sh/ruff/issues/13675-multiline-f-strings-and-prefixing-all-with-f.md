---
number: 13675
title: "Multiline f-strings and prefixing all with `f`"
type: issue
state: open
author: PatrickJordanNutanix
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-10-08T09:19:13Z
updated_at: 2024-10-13T14:02:59Z
url: https://github.com/astral-sh/ruff/issues/13675
synced_at: 2026-01-07T13:12:15-06:00
---

# Multiline f-strings and prefixing all with `f`

---

_Issue opened by @PatrickJordanNutanix on 2024-10-08 09:19_

We have soft rule in our code base that means we should prefix all multiline f-strings with an `f` even if no interpolation is happening on part of the string. For example, instead of doing this:

```python
long_string = (
  "This is a long string to demonstrate that when it wraps on to the next "
  f"line, all lines should be prefixed with an {f_prefix} to improve "
  "readability and effectively treat them along the same lines as a trailing "
  "comma, even if interpolation only happens on a subset of the lines."
)
```

We should be doing this:

```python
long_string = (
  f"This is a long string to demonstrate that when it wraps on to the next "
  f"line, all lines should be prefixed with an {f_prefix} to improve "
  f"readability and effectively treat them along the same lines as a trailing "
  f"comma, even if interpolation only happens on a subset of the lines."
)
````

Is there an existing rule that can catch this linting error? If not, can I request one be added please?

---

_Comment by @MichaReiser on 2024-10-08 12:29_

Does this rule of yours only apply if one of the string literals contain an expression or for all implicit concatenated strings?

E.g. what about

```python
long_string = (
  "This is a long string to demonstrate that when it wraps on to the next "
  "line, all lines should be prefixed with an ... to improve "
  "readability and effectively treat them along the same lines as a trailing "
  "comma, even if interpolation only happens on a subset of the lines."
)
```

---

_Comment by @PatrickJordanNutanix on 2024-10-08 13:00_

> Does this rule of yours only apply if one of the string literals contain an expression or for all implicit concatenated strings?

Yes, only if at some point a variable is being interpolated into it. If it's a multiline string with no interpolation, then we do not prefix with an `f`.



---

_Label `rule` added by @MichaReiser on 2024-10-13 13:59_

---

_Label `needs-decision` added by @MichaReiser on 2024-10-13 13:59_

---

_Comment by @MichaReiser on 2024-10-13 14:02_

Thanks. I now understand your use case. We could phrase it in a more general way and say that you want a common prefix for all string literals in implicit concatenated strings. The rule would then also enforce that all strings are raw-strings 

```python
a = (
	r"flag"
	"this"
)

# instead write
a = (
	r"flag"
	r"this"
)

# or
a = (
	"flag"
	"this"
)
```

Considering that this is an opinionated stylistic rule, we should wait for #1774 to avoid enabling the rule by default for everyone using `select=ALL`.



---

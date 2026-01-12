```yaml
number: 2103
title: "`dict` with known literal keys and values being assignable to a variable annotated with `Literal` is unsupported"
type: issue
state: closed
author: CarrotManMatt
labels: []
assignees: []
created_at: 2025-12-19T08:35:10Z
updated_at: 2025-12-22T22:46:13Z
url: https://github.com/astral-sh/ty/issues/2103
synced_at: 2026-01-12T15:54:26Z
```

# `dict` with known literal keys and values being assignable to a variable annotated with `Literal` is unsupported

---

_@CarrotManMatt_

### Summary

Apologies if this is a known lacking feature due to ty being beta software, I couldn't find any issues referring to this problem.

I have a variable typed as `Final[Mapping[Literal[1, 2, 3], Literal["DEBUG", "INFO", "WARN", "ERROR", "CRITICAL"]]]`, yet a dictionary made with the correct literal keys or values seems to be incorrectly unassignable to this variable: `{1: "INFO", 2: "DEBUG", 3: "DEBUG"}`.

> Object of type \`dict[Unknown | int, Unknown | str]\` is not assignable to \`Mapping[Literal[1, 2, 3], Literal["DEBUG", "INFO", "WARN", "ERROR", "CRITICAL"]]\`

https://play.ty.dev/7170ec5b-b7ff-4227-91eb-e1f06e0477c7

### Version

ty 0.0.4

---

_Comment by @AlexWaygood on 2025-12-19 08:37_

Thanks, I think this is #1576 (we don't apply type context very well at the moment if the declared type is a supertype of the inferred type)

---

_Closed by @AlexWaygood on 2025-12-19 08:37_

---

_Comment by @CarrotManMatt on 2025-12-19 09:21_

Cheers, apologies for the bug report noise because of me not being able to find the correct issue.

---

_Comment by @AlexWaygood on 2025-12-19 09:25_

Np, it's impossible to find dupes on github, especially for a project full of jargon like a type checker 😃

---

_Comment by @carljm on 2025-12-22 22:46_

(Confirmed the PR for #1576 already fixes this)

---

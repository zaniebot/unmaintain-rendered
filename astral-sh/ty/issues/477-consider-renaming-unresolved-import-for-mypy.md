```yaml
number: 477
title: Consider renaming unresolved-import for mypy compatibility
type: issue
state: closed
author: danielhollas
labels:
  - question
  - diagnostics
assignees: []
created_at: 2025-05-21T20:14:03Z
updated_at: 2025-10-06T16:02:30Z
url: https://github.com/astral-sh/ty/issues/477
synced_at: 2026-01-12T15:54:23Z
```

# Consider renaming unresolved-import for mypy compatibility

---

_@danielhollas_

### Question

I am not sure what's the plan around compatibility of `type: ignore` comments in general, but I thought perhaps some uncontroversal error codes could be renamed to be the same as mypy?

In a particular case I ran into today is `unresolved-import` which is `import-not-found` in mypy.
(I am aware the ty is currently ignoring unused type-ignores, but my use case was in the other direction. If we were to adopt ty in our project at the moment, we would need to add this type ignore, (while for mypy we have a configuration in pyproject.toml that specifically ignores certain modules).

Although perhaps this ad-hoc approach is not tenable in general, I just checked with `pyrefly` and they use yet another error code (`import-error` 😢 ).

Also, huge kudos to the team for the ty's speed. I tried `pyrefly` on our project, and it is ten times slower (2s vs 200ms). 🤯  Although to be fair to pyrefly, it does not panic, unlike ty for some files. 😅 

### Version

_No response_

---

_Label `question` added by @danielhollas on 2025-05-21 20:14_

---

_Comment by @AlexWaygood on 2025-05-21 20:16_

Hi! I think you can use `ty: ignore` if you want to only suppress diagnostics from ty and want the suppression comment to be... ignored... by other type checkers

---

_Comment by @AlexWaygood on 2025-05-21 20:27_

In general it is not really a goal to have exactly the same error codes as mypy, unfortunately, because it's not always just an issue of our categories being differently named -- we often disagree with mypy about how an error should be categorised in the first place. It would be a pretty significant undertaking to try to be compatible with mypy's error categorisation -- and in this case, for example, I rather prefer our name for the error code, as it's somewhat shorter ;)

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-21 20:42_

---

_Closed by @AlexWaygood on 2025-10-06 16:02_

---

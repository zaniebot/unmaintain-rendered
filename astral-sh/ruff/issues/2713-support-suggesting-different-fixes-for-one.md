```yaml
number: 2713
title: Support suggesting different fixes for one violation
type: issue
state: open
author: not-my-profile
labels:
  - fixes
assignees: []
created_at: 2023-02-10T06:30:57Z
updated_at: 2023-02-18T20:48:54Z
url: https://github.com/astral-sh/ruff/issues/2713
synced_at: 2026-01-12T15:54:43Z
```

# Support suggesting different fixes for one violation

---

_@not-my-profile_

Some violations can be fixed in different ways. The Language Server Protocol supports multiple code actions for one location, so it would be nice if ruff could also suggest multiple autofixes for one violation.

Examples:

* `try: ...; except ...: pass` -> flake8-simplify suggests using [`contextlib.suppress`](https://docs.python.org/3/library/contextlib.html#contextlib.suppress) instead, whereas flake8-bandit suggests logging the exception

Can you think of other examples?

---

_Label `autofix` added by @charliermarsh on 2023-02-10 13:14_

---

_Comment by @ngnpope on 2023-02-10 15:11_

It seems that we could add categories/tags related to autofixing?

- `@safe`: Fixes that can be safely applied in 100% of cases.
- `@unsafe`: Fixes that may produce unexpected results and need manual checking.
- `@unfixable`: Violations cannot be fixed automatically. _(Always require manual intervention.)_
- `@opinionated`: Fixes that may not be the best solution to fixing the violation.

   So, using the case in the description above, we could automatically fix to use `contextlib.suppress(...)`, but we couldn't automatically fix to add logging as we wouldn't be able to provide the context for the log message. In many cases it's better to make sure that logging is not required and wasn't merely forgotten during development.

  I'm not sure if it would ever make sense to have multiple ways of fixing the same issue? How would that be configured? This example is a good case of an automatic vs a manual alternative. Could we ever have more than one automatic fix?

Maybe a fix tagged as `@unsafe` and `@opinionated` would require both tags to be enabled to be used (or explicitly enabled by the rule name). That way there may be `@safe` or `@unsafe` fixes that are `@opinionated`. _(Obviously the first three defined above are mutually exclusive.)_

Was also wondering whether we want to have a tag that implies "sometimes" to capture issues like that raised for `SIM108` in #2621 where the autofix is not available in 100% of cases?

---

_Comment by @not-my-profile on 2023-02-10 15:24_

Yes such a categorization is indeed a good idea, we already have #1997 for tracking that :)

---

_Comment by @sbrugman on 2023-02-10 20:25_

Pandas-vet has one rule where multiple fixes are possible: `.ix` is deprecated; use more explicit `.loc` or `.iloc`


---

_Comment by @sbrugman on 2023-02-17 06:55_

Another example: 

B905 zip-without-explicit-strict has two possible autofixes (`strict=True` and `strict=False`)

---

_Comment by @JonathanPlasse on 2023-02-18 16:05_

> Pandas-vet has one rule where multiple fixes are possible: `.ix` is deprecated; use more explicit `.loc` or `.iloc`

> B905 zip-without-explicit-strict has two possible autofixes (`strict=True` and `strict=False`)

These auto-fixes could be configured in settings.

---

_Comment by @sbrugman on 2023-02-18 16:29_

It depends. In most cases the autofix is constant per repository (in which case the autofix can be configured on repo level). We can also imagine to review on a case-by-case basis to be able to decide (something like [`autobot review`](https://github.com/charliermarsh/autobot) would be nice).

---

_Comment by @charliermarsh on 2023-02-18 20:48_

Autobot reference in Ruff?? First time!

---

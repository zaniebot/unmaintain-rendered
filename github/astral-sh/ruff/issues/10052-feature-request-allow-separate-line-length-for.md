---
number: 10052
title: "Feature request: Allow separate line length for formatting"
type: issue
state: closed
author: Hubro
labels: []
assignees: []
created_at: 2024-02-19T13:53:16Z
updated_at: 2024-02-19T15:08:48Z
url: https://github.com/astral-sh/ruff/issues/10052
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: Allow separate line length for formatting

---

_Issue opened by @Hubro on 2024-02-19 13:53_

At my job we use Ruff for linting and Black for formatting. I just recently discovered that Ruff can now be used for formatting too, which is amazing news :rocket: The performance of Black has gotten on my nerve many times.

The problem is, my team has a soft line length limit at 88 characters and a hard limit at 100 characters. We've configured Black to wrap at 88 characters, but we don't raise any linter warnings before a line goes over 100 characters. This gives some extra headroom for lines that can't be cleanly wrapped, like lists of long strings.

When formatting with `ruff format`, it uses the same line length setting as the checker, meaning that it formats our entire codebase with line length 100 rather than 88. Currently it's not possible to specify a separate checker line length and formatter line length, which would be necessary to replicate our current ruff + Black setup.

I think it would be nice if it was possible to specify a line length under `[format]` which takes precedence when formatting:

```toml
line-length = 100

[format]
line-length = 88
```

Otherwise it would be quite painful and hacky to adopt `ruff format` in our code base.

---

_Comment by @MichaReiser on 2024-02-19 14:00_

Hy. Is my assumption correct that you're using the `pcyodestyle` rules to enforce the max line length? 

The pycodestyle rules allow overriding the [max line length](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_max-line-length) for linting (hard limit) where the formatter continues to use the global `line-length` (soft limit)

```
line-length = 88 # The formatter wraps lines at a length of 88

[pycodestyle]
max-line-length = 100 # E501 reports lines that exceed the length of 100.
```


---

_Comment by @Hubro on 2024-02-19 15:08_

Oh dang, perfect! Thank you! :heart: 

---

_Closed by @Hubro on 2024-02-19 15:08_

---

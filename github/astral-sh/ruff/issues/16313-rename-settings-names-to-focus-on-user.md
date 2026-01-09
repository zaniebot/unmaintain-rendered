---
number: 16313
title: Rename settings names to focus on user-friendliness rather than provenance
type: issue
state: closed
author: NeilGirdhar
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2025-02-22T09:20:07Z
updated_at: 2025-02-23T13:07:14Z
url: https://github.com/astral-sh/ruff/issues/16313
synced_at: 2026-01-07T13:12:16-06:00
---

# Rename settings names to focus on user-friendliness rather than provenance

---

_Issue opened by @NeilGirdhar on 2025-02-22 09:20_

### Description

Please consider renaming settings to make their names as easy to read as possible.  I recognize that historically, the "flake8-bandit" plugin's rules are provided by Ruff.  But contemporary users may not have ever even used or configured flake8-bandit.

Consider settings like:
```toml
[tool.ruff.lint.flake8-errmsg]
max-string-length = 40

[tool.ruff.lint.flake8-bandit]
check-typed-exception = true
```
Would this be better?
```toml
[tool.ruff.lint.exceptions]  # These both have to do with exceptions, so why not group them?
max-string-length = 40
disallow-try-except = true  # Name is closer to meaning.
```

In general, is it really important to keep the "flake8", "pylint", "pyflakes" prefixes anywhere, or are they an anachronism?

Here's an hard to decipher option:
```toml
[tool.ruff.lint.flake8-gettext]
extend-function-names = ["ugettetxt"]
```
How about
```toml
[tool.ruff.lint.internationalization]  # or i18n or just gettext?
extend-function-names = ["ugettetxt"]
```

---

_Label `configuration` added by @dylwil3 on 2025-02-22 12:22_

---

_Label `needs-design` added by @dylwil3 on 2025-02-22 12:22_

---

_Comment by @MichaReiser on 2025-02-22 12:39_

That makes sense to me and I completely agree that Ruff's settings should be semantically grouped longterm. However, I do think that Ruff's current setting structure is the right fit today because the rules are also grouped by upstream linters. It makes it easier to discover the right setting given a specific rule. 

I'll close this because it's something we should tackle as part of https://github.com/astral-sh/ruff/issues/1774 because, IMO, it's important that settings and the rule structure align.

---

_Closed by @MichaReiser on 2025-02-22 12:39_

---

_Comment by @NeilGirdhar on 2025-02-22 13:15_

> That makes sense to me and I completely agree that Ruff's settings should be semantically grouped longterm. However, I do think that Ruff's current setting structure is the right fit today because the rules are also grouped by upstream linters. It makes it easier to discover the right setting given a specific rule.

Makes sense, thanks for the thoughtful reply.

> I'll close this because it's something we should tackle as part of [#1774](https://github.com/astral-sh/ruff/issues/1774) because, IMO, it's important that settings and the rule structure align.

I'm looking forward to the rule categorization as well, and I recognize that you want to do the work of reorganizing the rules with the settings.

I guess I thought that the settings could be done independently since in my codebases, I just include every rule, but only have a handful of settings.  But I guess it doesn't matter since your end goal is perfect for me.

---

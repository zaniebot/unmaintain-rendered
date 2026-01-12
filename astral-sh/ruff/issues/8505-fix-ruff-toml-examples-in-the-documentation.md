```yaml
number: 8505
title: Fix ruff.toml examples in the documentation
type: issue
state: closed
author: CharlesPerrotMinot
labels:
  - bug
  - documentation
assignees: []
created_at: 2023-11-06T05:15:56Z
updated_at: 2023-11-14T05:44:31Z
url: https://github.com/astral-sh/ruff/issues/8505
synced_at: 2026-01-12T15:54:48Z
```

# Fix ruff.toml examples in the documentation

---

_@CharlesPerrotMinot_

Hi,
Thank so much for tuff! Love it so far.

There are a couple inconsistencies in the documentation for settings, where the value given as an example for what to use in the `ruff.toml` is the one to put in the `pyproject.toml` ; it has `tool.ruff.` in the the section name.
- https://docs.astral.sh/ruff/settings/#extend-per-file-ignores
- https://docs.astral.sh/ruff/settings/#per-file-ignores
- https://docs.astral.sh/ruff/settings/#flake8-import-conventions-aliases
- https://docs.astral.sh/ruff/settings/#flake8-import-conventions-banned-aliases
- https://docs.astral.sh/ruff/settings/#flake8-import-conventions-extend-aliases
- https://docs.astral.sh/ruff/settings/#flake8-tidy-imports-banned-api
- https://docs.astral.sh/ruff/settings/#isort-sections

---

_Renamed from "Fix ruff.toml exemples in the documentation" to "Fix ruff.toml examples in the documentation" by @CharlesPerrotMinot on 2023-11-06 05:17_

---

_Label `bug` added by @charliermarsh on 2023-11-06 05:29_

---

_Label `documentation` added by @charliermarsh on 2023-11-06 05:29_

---

_Comment by @charliermarsh on 2023-11-06 05:30_

Thanks a lot for reporting this, good catch (the dual `pyproject.toml` and `ruff.toml` tabs are new).

---

_Comment by @doolio on 2023-11-06 10:15_

I also noticed the tabs have not been added to all the FAQs.

https://docs.astral.sh/ruff/faq/?query=If+Ruff+detects+multiple+configuration+files#i-want-to-use-ruff-but-i-dont-want-to-use-pyprojecttoml-what-are-my-options

I will submit a PR.


---

_Comment by @doolio on 2023-11-06 10:23_

Do you want the `ruff.toml` tab labels to include `.ruff.toml` as well i.e. something like `ruff.toml/.ruff.toml` or perhaps it would look better to have `.ruff.toml` as an additional tab but the same content as `ruff.toml`?

Edit: There is also an inconsistency in the markdown source blocks where for some the entire block is indented and for others it is not. Do you have a preference so I could apply the same style consistently?

Edit2: I can't seem to find the `settings.md` (or `rules.md`) file(s) in the docs folder.

---

_Comment by @doolio on 2023-11-06 10:56_

> I also noticed the tabs have not been added to all the FAQs.
> I will submit a PR.

See #8512 for consideration.

---

_Comment by @trag1c on 2023-11-06 18:26_

> Edit: There is also an inconsistency in the markdown source blocks where for some the entire block is indented and for others it is not. Do you have a preference so I could apply the same style consistently?

As far as I'm concerned indented code blocks are only used for tabs because that's the syntax they require (up until my change you couldn't even have indented code blocks because CI would fail otherwise).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-14 05:14_

---

_Closed by @charliermarsh on 2023-11-14 05:44_

---

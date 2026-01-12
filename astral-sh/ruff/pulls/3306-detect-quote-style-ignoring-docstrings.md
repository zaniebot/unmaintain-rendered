```yaml
number: 3306
title: Detect quote style ignoring docstrings
type: pull_request
state: merged
author: bz2
labels: []
assignees: []
merged: true
base: main
head: detect_quote_skip_docstrings
created_at: 2023-03-02T14:29:47Z
updated_at: 2023-03-03T16:02:14Z
url: https://github.com/astral-sh/ruff/pull/3306
synced_at: 2026-01-12T15:55:12Z
```

# Detect quote style ignoring docstrings

---

_@bz2_

Currently the quote style of the first string in a file is used for autodetecting what to use when rewriting code for fixes. This is an okay heuristic, but often the first line in a file is a docstring, rather than a string constant, and it's not uncommon for pre-Black code to have different quoting styles for those.

For example, in the Google style guide:
https://google.github.io/styleguide/pyguide.html
> Be consistent with your choice of string quote character within a file. Pick ' or " and stick with it. ... Docstrings must use """ regardless.

This branch adjusts the logic to instead skip over any `"""` triple doublequote string tokens. The default, if there are no single quoted strings, is still to use double quote as the style.

---

_Comment by @charliermarsh on 2023-03-03 04:58_

This seems like a reasonable change, though I guess it'll still have the potential to get things "wrong", since you could be using single quotes in your project, but have one file with a top-level docstring and nothing else.

---

_Comment by @charliermarsh on 2023-03-03 04:59_

(We could, in addition, let you specify a quote style for fixes. We'll want to do this anyway once we introduce the autoformatter. But what you have here seems like an improvement over the status quo anyway.)

---

_Merged by @charliermarsh on 2023-03-03 04:59_

---

_Closed by @charliermarsh on 2023-03-03 04:59_

---

_Comment by @bz2 on 2023-03-03 16:02_

@charliermarsh Thanks for landing!

Yeah, there are still various ways the heuristic can fail, but need to know more about the formatting to do better. Rules like `D300` could inform, but there are lots of little edge cases for string literals that aren't amenable to linting.

---

_Branch deleted on 2023-03-03 16:02_

---

```yaml
number: 10343
title: Allow trailing comments after supression comments
type: issue
state: open
author: zanieb
labels:
  - suppression
  - help wanted
  - needs-design
assignees: []
created_at: 2024-03-11T17:20:36Z
updated_at: 2024-03-11T18:57:27Z
url: https://github.com/astral-sh/ruff/issues/10343
synced_at: 2026-01-10T11:09:52Z
```

# Allow trailing comments after supression comments

---

_Issue opened by @zanieb on 2024-03-11 17:20_

We should allow content to follow suppression comments e.g. to provide a reason for the `# noqa`.

We need to decide on a format to support with considerations of compatibility with other tools.

Previously discussed in:

- https://github.com/astral-sh/ruff/issues/10239
- https://github.com/astral-sh/ruff/issues/8892
- https://github.com/astral-sh/ruff/pull/8876

---

_Label `suppression` added by @zanieb on 2024-03-11 17:20_

---

_Label `help wanted` added by @zanieb on 2024-03-11 17:20_

---

_Label `needs-design` added by @zanieb on 2024-03-11 17:20_

---

_Comment by @tylerlaprade on 2024-03-11 18:57_

I have been using `--` since it was encouraged by [this ESLint rule](https://mysticatea.github.io/eslint-plugin-eslint-comments/rules/require-description.html).

I'm not personally too particular about the format. However, I think it would be wise to ensure compatibility with both Flake8 and Black since many incoming codebases are migrated from one or both.

I also think it's important that this enhancement does not break compatibility with Pyright/Mypy suppressions on the same line. This is not an uncommon occurrence, since something that violates the type system likely also violates one or more lint rules.

---

```yaml
number: 14507
title: Use multiple columns for repology badge
type: pull_request
state: closed
author: sharkdp
labels:
  - internal
assignees: []
base: main
head: david/repology-badge
created_at: 2024-11-21T08:23:21Z
updated_at: 2024-11-21T19:33:53Z
url: https://github.com/astral-sh/ruff/pull/14507
synced_at: 2026-01-12T15:55:48Z
```

# Use multiple columns for repology badge

---

_@sharkdp_

## Summary

These repology badges grow very "tall" over time. We can use a `columns` argument to make more use of the available space. Not sure if there are any `@media`-query breakpoint implications here (max allowed width of images or similar). So feel free to close if this is not desired for some reason.

### before (current version on https://docs.astral.sh/ruff/installation/):

[![Packaging status](https://repology.org/badge/vertical-allrepos/ruff-python-linter.svg?exclude_unsupported=1)](https://repology.org/project/ruff-python-linter/versions)

### after:

[![Packaging status](https://repology.org/badge/vertical-allrepos/ruff-python-linter.svg?exclude_unsupported=1&columns=3)](https://repology.org/project/ruff-python-linter/versions)


---

_Label `internal` added by @sharkdp on 2024-11-21 08:23_

---

_Comment by @MichaReiser on 2024-11-21 08:24_

I don't mind the length too much, considering that it is at the end of the page.

Does the column layout result in vertical scroll bars on mobile?

---

_Comment by @sharkdp on 2024-11-21 08:34_

> Does the column layout result in vertical scroll bars on mobile?

No. But potentially gets harder to read.

![image](https://github.com/user-attachments/assets/9b935513-9863-49eb-929e-29a3638dc7fa)

> I don't mind the length too much, considering that it is at the end of the page.

Ok, let's just close it.

Edit: I just now saw https://github.com/astral-sh/ruff/pull/1779. Sorry for the noise.

---

_Closed by @sharkdp on 2024-11-21 08:34_

---

_Branch deleted on 2024-11-21 19:33_

---

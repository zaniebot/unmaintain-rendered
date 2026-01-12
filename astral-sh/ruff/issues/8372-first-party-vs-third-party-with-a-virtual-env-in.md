```yaml
number: 8372
title: First-party vs. third party with a virtual env in the project root
type: issue
state: closed
author: thernstig
labels:
  - question
assignees: []
created_at: 2023-10-31T10:18:54Z
updated_at: 2023-10-31T18:47:30Z
url: https://github.com/astral-sh/ruff/issues/8372
synced_at: 2026-01-12T15:54:48Z
```

# First-party vs. third party with a virtual env in the project root

---

_@thernstig_

Would it be possible for a virtual env placed inside the project directory to automatically not be considered a first party?

The default `exclude` option for example have `.venv` as an ignore, but I am not sure that should hold any bearing. Should excludes always be third party?

Or can Ruff possibly figure out if the virtual env is inside the root and omit that in its discovery as first-party?

## Additional context

I realize lots of thought have been done around this, by reading https://docs.astral.sh/ruff/settings/#src, the [FAQ ](https://docs.astral.sh/ruff/faq) and https://docs.astral.sh/ruff/contributing/#import-categorization_1m, as well as seeing other issue such as https://github.com/astral-sh/ruff/issues/2004. I am fine to close this immediately as such, but brought this up more as an idea/"what if it has not been thought about". Ergo, feel free to close immediately if this does not make sense. 😃  

---

_Comment by @charliermarsh on 2023-10-31 18:47_

Yeah, we may support discovering files within virtual environments eventually (at which point we could classify them as third-party, rather than relying on "not-first-party" as the definition of third-party), but right now it's not supported.

---

_Closed by @charliermarsh on 2023-10-31 18:47_

---

_Label `question` added by @charliermarsh on 2023-10-31 18:47_

---

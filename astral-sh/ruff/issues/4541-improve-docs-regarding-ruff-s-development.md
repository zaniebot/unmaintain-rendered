```yaml
number: 4541
title: "Improve docs regarding ruff's development philosophy"
type: issue
state: closed
author: Mr-Pepe
labels:
  - documentation
assignees: []
created_at: 2023-05-20T10:38:13Z
updated_at: 2024-01-11T01:37:33Z
url: https://github.com/astral-sh/ruff/issues/4541
synced_at: 2026-01-12T15:54:44Z
```

# Improve docs regarding ruff's development philosophy

---

_@Mr-Pepe_

I might have overlooked the relevant section but I don't fully understand the development philosophy behind ruff and think that could be made clearer in the docs. 

Is ruff supposed to "simply" be a reimplementation of existing Python linting tools to leverage the speed of Rust? Should feature requests and bug reports for specific checks be created with the respective upstream project? Why would anyone do that, once ruff has reached feature parity with the upstream project? 

To name an example, ruff has better usability and performance regarding the features originally provided by pyupgrade. It seems like a lot of effort to keep the two tools (features, bug fixes, documentation) in sync and why would users care about evolving the original tool if ruff is faster and nicer to use? Is there a goal of evolving ruff independently, once it has surpassed the original tool? Or is this still an open question and time will tell how things will develop organically?

---

_Comment by @johnthagen on 2023-05-21 17:54_

Some short-term development thoughts are captured here

- https://github.com/charliermarsh/ruff/issues/1992#issuecomment-1405497643

---

_Label `documentation` added by @charliermarsh on 2023-05-22 03:11_

---

_Comment by @charliermarsh on 2024-01-11 01:37_

I'm happy with the norms we've established here:

- We attempt to maintain parity with the "upstream" tools that we've reimplemented (including new rules). We deviate to fix clear bugs, and in some cases, to add configuration options.
- We have a few categories that don't match "upstream" in a way that's meaningful, and so we evolve independently (like `pyupgrade` rules -- there's no rule ID encoding in `pyupgrade`, so we add some of our own rules there).
- There are some categories (like `pydocstyle`) where the tool has since been archived, and so we're now free (and in fact have the responsibility) to evolve them over time.
- We have our own Ruff category for custom rules.

Our plan is to recategorize all rules into semantically-meaningful categories in the future, though it hasn't been a priority and so the details are still undefined. Our intent, though, is to evolve towards a more Ruff-centric API over time.

---

_Closed by @charliermarsh on 2024-01-11 01:37_

---

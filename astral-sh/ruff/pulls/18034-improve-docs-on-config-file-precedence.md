```yaml
number: 18034
title: Improve docs on config file precedence
type: pull_request
state: closed
author: timhoffm
labels:
  - documentation
assignees: []
base: main
head: doc-config-preference
created_at: 2025-05-12T09:21:05Z
updated_at: 2025-05-23T18:20:39Z
url: https://github.com/astral-sh/ruff/pull/18034
synced_at: 2026-01-10T18:51:01Z
```

# Improve docs on config file precedence

---

_Pull request opened by @timhoffm on 2025-05-12 09:21_

When working with ruff, I fell into the trap of having `.ruff.toml` and `pyproject.toml` config files. It took me some time to find the precedence rules from the docs. This PR aims to make the docs slightly more clear.

- In the section "Config file discovery" the precedence was described quite lengthy at the very end of the section. I propose it's more concise to describe this with a single sentence immediately after stating that there can be one config file per directory.
- At the very top, when listing all config file types, list them in the order of their precedence. Being equal otherwise, matching the order of precendence may alreay be a subtle hint to users.



---

_Label `documentation` added by @ntBre on 2025-05-12 12:47_

---

_@MichaReiser reviewed on 2025-05-13 07:20_

---

_Review comment by @MichaReiser on `docs/configuration.md`:3 on 2025-05-13 07:20_

I can see how the current ordering can be misleading as it can give the impression that it matches the precedence in which configuration files are discovered. However, the ordering is somewhat intentional because most users should use `pyproject.toml` files over a `ruff.toml` 

---

_@MichaReiser reviewed on 2025-05-13 07:21_

---

_Review comment by @MichaReiser on `docs/configuration.md`:309 on 2025-05-13 07:21_

I prefered this phrasing because it is more specific about when the precedence applies (it applies on a per directory level)

---

_@timhoffm reviewed on 2025-05-13 08:21_

---

_Review comment by @timhoffm on `docs/configuration.md`:3 on 2025-05-13 08:21_

Sure if there's an intended order through usage priority, let's keep it. Maybe it's reasonable to add a short paragraph on the advantages/disadvantages of the different options and when to use what. But that can be a separate topic.

---

_@timhoffm reviewed on 2025-05-13 08:21_

---

_Review comment by @timhoffm on `docs/configuration.md`:309 on 2025-05-13 08:21_

Would it work for you if we keep the exact wording and just move the lines 306-308 up?

---

_Comment by @timhoffm on 2025-05-22 19:46_

I've kept the original phrasing and just move the text up. Feel free to merge or close depending on what makes more sense to you.

---

_Comment by @MichaReiser on 2025-05-23 15:06_

I slightly prefer the currnet structure. That's why I'll close this PR. But thanks for discussing this change

---

_Closed by @MichaReiser on 2025-05-23 15:06_

---

_Branch deleted on 2025-05-23 18:20_

---

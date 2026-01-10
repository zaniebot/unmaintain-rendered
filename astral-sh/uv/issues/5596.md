```yaml
number: 5596
title: "Feature Request: `extend` config option"
type: issue
state: open
author: jvacek
labels:
  - wish
  - configuration
assignees: []
created_at: 2024-07-30T11:57:59Z
updated_at: 2025-05-29T09:05:06Z
url: https://github.com/astral-sh/uv/issues/5596
synced_at: 2026-01-10T03:41:46Z
```

# Feature Request: `extend` config option

---

_Issue opened by @jvacek on 2024-07-30 11:57_

The usecase for us is that we share a lot of configs among multiple repositories. We track the "shared" configuration separately to our codebases, and the individual repos pull that as a submodule, and extend from it in pyproject.toml. If we need to do major changes like changing our extra-index address for example, this would help us do that more efficiently.

Essentially, I'd appreciate having something like [`ruff`'s `extend`](https://docs.astral.sh/ruff/settings/#extend) directive available for `uv`'s config. None of the additional `extend-[...]` options are really relevant here in my opinion, as there aren't any arrays to extend in `uv`'s config as far as I can tell.

This already works great for us with `ruff` and also `pyright`, and while `uv` requires substantially less configuration, would still be nice to have it work similarly.

---

_Label `wish` added by @charliermarsh on 2024-07-30 12:19_

---

_Label `configuration` added by @charliermarsh on 2024-07-30 12:19_

---

_Comment by @jvacek on 2024-10-22 12:14_

Heya, is there any chance this can get some priority? Would help us roll out `uv` in our org :)

---

_Comment by @zanieb on 2024-10-22 12:24_

Given the lack of additional interest and the amount of work we have on our plate right now, it seems unlikely that we can devote time to this in the near future. Sorry.

---

_Comment by @Korben11 on 2025-05-29 09:05_

Would it be an idea to rethink `extend` to be usable anywhere out of box? ATM its implemented for some parts like `[tool.ruff]`, but it doesn't work for `[tool.ruff.lint]`.

For example to keep our monorepo CI setup DRY we use [include](https://docs.gitlab.com/ci/yaml/includes/) in combination with [extends](https://docs.gitlab.com/ci/yaml/#extends).

---

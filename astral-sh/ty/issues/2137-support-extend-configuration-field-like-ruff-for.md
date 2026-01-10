---
number: 2137
title: "Support `extend` configuration field (like `ruff`) for config inheritance"
type: issue
state: open
author: aeroyorch
labels:
  - configuration
assignees: []
created_at: 2025-12-20T23:08:08Z
updated_at: 2026-01-02T15:59:30Z
url: https://github.com/astral-sh/ty/issues/2137
synced_at: 2026-01-10T01:51:14Z
---

# Support `extend` configuration field (like `ruff`) for config inheritance

---

_Issue opened by @aeroyorch on 2025-12-20 23:08_

First of all, thanks for ty!

I’d like to ask whether `ty` plans to support an `extend` configuration field, similar to [the one provided by `ruff`](https://docs.astral.sh/ruff/configuration/#config-file-discovery), to allow explicit configuration inheritance.

This would be helpful for sharing common configuration parameters in monorepositories with multiple `pyproject.toml` files, while keeping overrides explicit and localized.

---

_Label `question` added by @aeroyorch on 2025-12-20 23:08_

---

_Label `question` removed by @MichaReiser on 2025-12-21 09:24_

---

_Label `configuration` added by @MichaReiser on 2025-12-21 09:24_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-21 09:24_

---

_Comment by @MichaReiser on 2025-12-22 12:28_

I'll add this as a sub issue to https://github.com/astral-sh/ty/issues/175 as it is about the same functionality -- some way to share configurations. 

@aeroyorch could you say a bit more about your project layout? More specifically, where do you want to define the shared configuration and where would you use it (projects of the same workspace or across workspaces?)

---

_Comment by @aeroyorch on 2025-12-22 15:50_

> I'll add this as a sub issue to [#175](https://github.com/astral-sh/ty/issues/175) as it is about the same functionality -- some way to share configurations.
> 
> [@aeroyorch](https://github.com/aeroyorch) could you say a bit more about your project layout? More specifically, where do you want to define the shared configuration and where would you use it (projects of the same workspace or across workspaces?)

In my case, this is a typical `uv` workspace with several packages and a root `pyproject.toml`. Maybe, supporting configuration inheritance or extension in `ty`, similar to `ruff`, would help keep configuration patterns consistent across both tools.

---

_Comment by @mathias-kende on 2026-01-02 15:59_

If the inherited pyproject.toml can be anywhere (same behavior as the `extend` key of ruff) instead of just in a parent folder (as per #175) this would allow more use cases (e.g. a monorepo where the root is not a pyproject can still have some global ruff + ty configuration defined in a, for example, `python_linter` directory that all other projects inherit).

Either way, to me this alone would be a killer feature to adopt ty (as is the equivalent ruff option).

---

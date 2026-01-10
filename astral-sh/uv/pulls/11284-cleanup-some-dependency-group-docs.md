```yaml
number: 11284
title: cleanup some dependency-group docs
type: pull_request
state: merged
author: Gankra
labels:
  - documentation
assignees: []
merged: true
base: main
head: gankra/dococlock
created_at: 2025-02-06T16:31:07Z
updated_at: 2025-02-07T15:41:00Z
url: https://github.com/astral-sh/uv/pull/11284
synced_at: 2026-01-10T11:10:34Z
```

# cleanup some dependency-group docs

---

_Pull request opened by @Gankra on 2025-02-06 16:31_

Some additional details, more mentioning of related flags, and some minor rewordings to avoid misconceptions I had from the current docs.

Closes #11205 

---

_Label `documentation` added by @Gankra on 2025-02-06 16:31_

---

_@Gankra reviewed on 2025-02-06 16:32_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2715 on 2025-02-06 16:32_

Notably this specific wording gave me a big misconception early on. I thought the exclusion was at the level of the dependencies references, and not just the names of the groups themselves.

---

_@zanieb reviewed on 2025-02-06 17:56_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2703 on 2025-02-06 17:56_

This is a bit more casual than we usually use. I'd say

> See `--no-default-groups` to disable all default groups instead.

I'm a little hesitant to have this here, but it seems okay. Generally, `--no-dev` and `--dev` exist for users that don't care about using multiple groups.

---

_@zanieb reviewed on 2025-02-06 17:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2718 on 2025-02-06 17:57_

I think this is a little hard to follow. Maybe just

> This option always takes precedence over default groups, `--all-groups`, and `--group`.

---

_@zanieb reviewed on 2025-02-06 17:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2724 on 2025-02-06 17:58_

Maybe this should be "Ignore the default dependency groups"

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2727 on 2025-02-06 18:01_

Then, following above, we can say....

> uv includes the groups defined in `tool.uv.default-groups` by default. This disables that option, however,
> specific groups can still be included with `--group`.

We could have something like

> In contrast, `--no-group` cannot be overridden by `--group`.

but I think it's kind of too in the weeds here? It makes more sense to cover it in `--group` / `--no-group`.

---

_@zanieb reviewed on 2025-02-06 18:01_

---

_@zanieb reviewed on 2025-02-06 18:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2733 on 2025-02-06 18:01_

```suggestion
    /// The project and its dependencies will be omitted.
```

---

_@zanieb reviewed on 2025-02-06 18:02_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2733 on 2025-02-06 18:02_

(and same below)

I'm hesitant of introducing the concept of "normal" dependencies — it's not used elsewhere.

---

_@Gankra reviewed on 2025-02-07 13:02_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2727 on 2025-02-07 13:02_

Hmm I agree on removing the second paragraph as too in-the-weeds.

---

_@Gankra reviewed on 2025-02-07 13:04_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:2727 on 2025-02-07 13:04_

Or rather I'm taking what you said here in the first quote.

---

_@zanieb reviewed on 2025-02-07 15:11_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:612 on 2025-02-07 15:11_

My earlier note applies here too

---

_@zanieb approved on 2025-02-07 15:12_

---

_Merged by @Gankra on 2025-02-07 15:40_

---

_Closed by @Gankra on 2025-02-07 15:40_

---

_Branch deleted on 2025-02-07 15:41_

---

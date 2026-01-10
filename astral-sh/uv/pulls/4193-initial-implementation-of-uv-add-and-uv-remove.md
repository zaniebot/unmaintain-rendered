```yaml
number: 4193
title: "Initial implementation of `uv add` and `uv remove`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: uv-add
created_at: 2024-06-10T11:36:58Z
updated_at: 2024-06-11T20:52:05Z
url: https://github.com/astral-sh/uv/pull/4193
synced_at: 2026-01-10T13:54:02Z
```

# Initial implementation of `uv add` and `uv remove`

---

_Pull request opened by @ibraheemdev on 2024-06-10 11:36_

## Summary

Basic implementation of `uv add` and `uv remove` that supports writing PEP508 requirements to `project.dependencies`. 

First step for https://github.com/astral-sh/uv/issues/3959 and https://github.com/astral-sh/uv/issues/3960.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-10 11:37_

---

_@ibraheemdev reviewed on 2024-06-10 11:40_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/remove.rs`:55 on 2024-06-10 11:40_

I'm not sure if this is correct? Should dev dependencies ever be synced even if they weren't modified?

---

_@charliermarsh reviewed on 2024-06-10 12:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/remove.rs`:55 on 2024-06-10 12:30_

It's possible that the dev dependencies _could_ change by adding a non-dev dependency, because they're all locked together. (E.g., maybe the dep you add has a transitive dep on one of the dev deps, so now it's more constrained.) So, I guess we do have the include them? But it's not really right to sync them unconditionally. The same problem exists with extras. We shouldn't be syncing them unconditionally. We need to know which extras the user wants activated.

I'm wondering if `add` should imply `lock`, but not `sync`? Since locking is much simpler: we lock everything.

(I think there is a general problem here that exists as long as `add` implies `lock` or `sync`. (Rye has this problem too). Any arguments that are typically passed to `lock` or `sync` to guide resolution or installation now need to be passed to `add` too... So, e.g., we might need `exclude-newer` too.)


---

_@charliermarsh reviewed on 2024-06-10 12:31_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/remove.rs`:35 on 2024-06-10 12:31_

What happens if you `uv remove anyio`, without including the specifier?

---

_@charliermarsh reviewed on 2024-06-10 12:32_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:34 on 2024-06-10 12:32_

I think we should enumerate some of the TODOs that aren't tackled yet. For example:

- We should accept unnamed URLs, and resolve them to named requirements.
- We need to figure out how this interacts with `tool.uv.sources`. When do we add data to `tool.uv.sources` rather than `project.dependencies`? \cc @konstin, you two should discuss this.
- Editable requirements?

---

_@charliermarsh reviewed on 2024-06-10 12:34_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1957 on 2024-06-10 12:34_

I wonder if this should be `Requirement` as long as we're parsing these directly. But, I think what you have here is probably better.

---

_@charliermarsh reviewed on 2024-06-10 12:35_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1932 on 2024-06-10 12:35_

Nit: for consistency...

```
/// The packages to remove, as PEP 508 requirements (e.g., `flask==2.2.3`).
```

---

_@charliermarsh reviewed on 2024-06-10 12:35_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:37 on 2024-06-10 12:35_

Where does this requirement come from? Any way we can avoid it?

---

_@charliermarsh reviewed on 2024-06-10 12:36_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:80 on 2024-06-10 12:36_

What if a dep is included multiple times? E.g.:

```toml
dependencies = [
  "flask > 2 ; python_version >= '3.6'",
  "flask < 2 ; python_version < '3.6'",
]
```


---

_@charliermarsh reviewed on 2024-06-10 12:37_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:92 on 2024-06-10 12:37_

Nit: can we use descriptive names, like `req` or over `x`?

---

_Review comment by @konstin on `crates/uv-distribution/src/pyproject.rs`:35 on 2024-06-11 07:27_

This can be a `toml::de::Error` result

---

_@konstin reviewed on 2024-06-11 07:45_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:92 on 2024-06-11 10:22_

Fixed, some of this code was copy-pasted from Rye and I didn't edit it too carefully.

---

_@ibraheemdev reviewed on 2024-06-11 10:22_

---

_@ibraheemdev reviewed on 2024-06-11 10:39_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/remove.rs`:35 on 2024-06-11 10:39_

`remove_dependency` removes by package name, so that works.

---

_@ibraheemdev reviewed on 2024-06-11 10:40_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/remove.rs`:55 on 2024-06-11 10:40_

Made `add` and `remove` do an unconditional full sync for now.

---

_@ibraheemdev reviewed on 2024-06-11 10:41_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:80 on 2024-06-11 10:41_

Hmm it would only remove the first occurrence then. I think Rye has this bug as well?

---

_@ibraheemdev reviewed on 2024-06-11 11:00_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:80 on 2024-06-11 11:00_

Updated to remove all occurrences. Also changed `uv remove` to take package names, not full requirements, because otherwise `uv remove flask>2` becomes more complicated. I think package names make more sense anyways? Unless we want to support e.g. removing unnamed requirements by URL?

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-11 11:52_

---

_@ibraheemdev reviewed on 2024-06-11 12:02_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/add.rs`:34 on 2024-06-11 12:02_

Tracking in https://github.com/astral-sh/uv/issues/3959.

---

_@charliermarsh reviewed on 2024-06-11 12:17_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:24 on 2024-06-11 12:17_

Nit: wrap pyproject.toml in backticks for consistency with the other error variant.

---

_@charliermarsh reviewed on 2024-06-11 12:18_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:48 on 2024-06-11 12:18_

`eq_ignore_ascii_case` isn't quite right here. These should be `ProjectName` which are already normalized.

---

_@charliermarsh reviewed on 2024-06-11 12:19_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:57 on 2024-06-11 12:19_

What should happen if there are multiple occurrences? My guess is we replace them all.

---

_@charliermarsh reviewed on 2024-06-11 12:20_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/pyproject_mut.rs`:118 on 2024-06-11 12:20_

Unnecessary `pub`, I think.

---

_@charliermarsh reviewed on 2024-06-11 12:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:34 on 2024-06-11 12:22_

I think we should just use `pep508_rs::Requirement::from_str` instead of `LenientRequirement`... We are a little inconsistent about it, but in general, `LenientRequirement` is really intended for third-party metadata over which the user has no control. We shouldn't let them add invalid requirements here.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:34 on 2024-06-11 12:23_

This also applies to the modification code in `crates/uv-distribution/src/pyproject_mut.rs`. What do you think?

---

_@charliermarsh reviewed on 2024-06-11 12:23_

---

_@charliermarsh approved on 2024-06-11 12:26_

A few more comments. I am happy to re-review but also trust you to address them and merge when you think they're closed out.

---

_@ibraheemdev reviewed on 2024-06-11 12:38_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:57 on 2024-06-11 12:38_

Replace them all with the single new requirement? i.e. replace the first occurrence and remove the rest?

---

_@ibraheemdev reviewed on 2024-06-11 12:53_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/add.rs`:34 on 2024-06-11 12:53_

That makes sense to me, I removed the use of  `LenientRequirement`.

---

_@ibraheemdev reviewed on 2024-06-11 13:20_

---

_Review comment by @ibraheemdev on `crates/uv-distribution/src/pyproject_mut.rs`:57 on 2024-06-11 13:20_

Implemented it that way, I think that makes the most sense, although it might be good to warn/log because this whole scenario is a bit questionable.

---

_Merged by @ibraheemdev on 2024-06-11 13:21_

---

_Closed by @ibraheemdev on 2024-06-11 13:21_

---

_Label `preview` added by @zanieb on 2024-06-11 20:52_

---

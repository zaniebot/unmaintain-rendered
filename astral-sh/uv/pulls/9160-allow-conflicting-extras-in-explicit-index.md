```yaml
number: 9160
title: Allow conflicting extras in explicit index assignments
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-11-16T02:46:45Z
updated_at: 2024-11-19T13:44:44Z
url: https://github.com/astral-sh/uv/pull/9160
synced_at: 2026-01-12T16:08:40Z
```

# Allow conflicting extras in explicit index assignments

---

_@charliermarsh_

## Summary

This PR enables something like the "final boss" of PyTorch setups -- explicit support for CPU vs. GPU-enabled variants via extras:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.13.0"
dependencies = []

[project.optional-dependencies]
cpu = [
    "torch==2.5.1+cpu",
]
gpu = [
    "torch==2.5.1",
]

[tool.uv.sources]
torch = [
    { index = "torch-cpu", extra = "cpu" },
    { index = "torch-gpu", extra = "gpu" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[tool.uv]
conflicts = [
    [
        { extra = "cpu" },
        { extra = "gpu" },
    ],
]
```

It builds atop the conflicting extras work to allow sources to be marked as specific to a dedicated extra being enabled or disabled.

As part of this work, sources now have an `extra` field. If a source has an `extra`, it means that the source is only applied to the requirement when defined within that optional group. For example, `{ index = "torch-cpu", extra = "cpu" }` above only applies to `"torch==2.5.1+cpu"`.

The `extra` field does _not_ mean that the source is "enabled" when the extra is activated. For example, this wouldn't work:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.13.0"
dependencies = ["torch"]

[tool.uv.sources]
torch = [
    { index = "torch-cpu", extra = "cpu" },
    { index = "torch-gpu", extra = "gpu" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

In this case, the sources would effectively be ignored. Extras are really confusing... but I think this is correct? We don't want enabling or disabling extras to affect resolution information that's _outside_ of the relevant optional group.


---

_Label `enhancement` added by @charliermarsh on 2024-11-16 02:46_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-16 02:50_

---

_Review requested from @konstin by @charliermarsh on 2024-11-16 02:50_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-16 02:50_

---

_Marked ready for review by @charliermarsh on 2024-11-16 02:50_

---

_Comment by @charliermarsh on 2024-11-16 02:50_

This needs documentation, but I'm gonna wait for approval.

---

_Comment by @zanieb on 2024-11-16 15:55_

I worry about the confusion of

> The extra field does not mean that the source is "enabled" when the extra is activated.

But otherwise I'm into it! We could be more verbose and say `from-extra` to help clarify that case, but I don't know if I love that.

---

_Comment by @charliermarsh on 2024-11-16 17:29_

This now also supports dependency groups equally.

---

_Comment by @charliermarsh on 2024-11-16 17:30_

Umm. Maybe `from = { extra = "cpu" }` and `from = { extra = "gpu" }`?

---

_Comment by @charliermarsh on 2024-11-16 17:31_

I could also try to make it such that we error if we see the dependency requested as a production dependency, when it has a source with an extra on it?

---

_Comment by @zanieb on 2024-11-17 16:05_

> from = { extra = "cpu" } and from = { extra = "gpu" }?

I can't think of a case from `from = <package>` makes sense which would be a mismatch with our other uses of `from`. Hm.

> I could also try to make it such that we error if we see the dependency requested as a production dependency, when it has a source with an extra on it?

This seems reasonable, or at least a warning.

---

_Comment by @charliermarsh on 2024-11-17 16:37_

Ok, I'll start by adding some dedicated errors around this stuff. I think with good error messages (e.g., detect if `torch` is in `project.dependencies` but not in the relevant extra) it could be fine.

---

_Comment by @charliermarsh on 2024-11-17 18:21_

Ok, I added some dedicated error messages around this.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/indexes.rs`:28 on 2024-11-18 17:41_

This is the subset of forks in which this entry occurs, not a conflict we generated?

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:1134 on 2024-11-18 17:43_

Is that reachable?

---

_@konstin approved on 2024-11-18 17:49_

This is a cool application of conflicting extras!

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/lowering.rs`:46 on 2024-11-18 18:30_

Should these be `Option<&ExtraName>` and `Option<&GroupName>`? It looks like the Clippy lint for this was suppressed explicitly, so I'm guessing there's a reason for it, but it doesn't immediately stand out to me.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/indexes.rs`:71 on 2024-11-18 18:35_

Nice

---

_@BurntSushi approved on 2024-11-18 18:36_

Nice! I think this all makes sense to me.

---

_Comment by @zanieb on 2024-11-18 19:20_

Just to clarify, this looks like it includes `group = ` as well as `extra = ` support?

---

_Comment by @charliermarsh on 2024-11-18 19:23_

It does yes. I mentioned that [here](https://github.com/astral-sh/uv/pull/9160#issuecomment-2480676512) but I guess not in the PR summary.

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:46 on 2024-11-19 00:45_

They should but I can't for the life of me figure out how to get it to work at the call site.

---

_@charliermarsh reviewed on 2024-11-19 00:45_

---

_@charliermarsh reviewed on 2024-11-19 00:48_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:46 on 2024-11-19 00:48_

Figured it out by removing a lifetime.

---

_Merged by @charliermarsh on 2024-11-19 01:06_

---

_Closed by @charliermarsh on 2024-11-19 01:06_

---

_Branch deleted on 2024-11-19 01:06_

---

_@BurntSushi reviewed on 2024-11-19 13:44_

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/lowering.rs`:46 on 2024-11-19 13:44_

Ahhh yeah I see now. Nice.

---

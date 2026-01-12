```yaml
number: 9296
title: "Repeated markers in `resolution-markers`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-11-20T23:36:14Z
updated_at: 2024-12-10T19:58:40Z
url: https://github.com/astral-sh/uv/issues/9296
synced_at: 2026-01-12T15:59:46Z
```

# Repeated markers in `resolution-markers`

---

_@charliermarsh_

If you lock:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]
environments = ["platform_system != 'Darwin'", "platform_system == 'Darwin'"]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

Then the top of the lockfile includes:

```toml
resolution-markers = [
    "platform_system != 'Darwin'",
    "platform_system != 'Darwin'",
    "platform_system != 'Darwin'",
    "platform_system == 'Darwin'",
    "platform_system == 'Darwin'",
    "platform_system == 'Darwin'",
]
```

And the packages have entries like:

```toml
[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_system != 'Darwin'",
    "platform_system != 'Darwin'",
    "platform_system != 'Darwin'",
    "platform_system == 'Darwin'",
    "platform_system == 'Darwin'",
    "platform_system == 'Darwin'",
]
```

---

_Label `bug` added by @charliermarsh on 2024-11-20 23:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-21 00:48_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-11-21 00:50_

---

_Comment by @charliermarsh on 2024-11-21 00:50_

@BurntSushi -- I think this is somehow related to #9289. When we first fork, we create three forks:

```
DEBUG fork: ResolverEnvironment { kind: Universal { initial_forks: [platform_system != 'Darwin', platform_system == 'Darwin'], markers: platform_system != 'Darwin', exclude: {ConflictItem { package: PackageName("project"), conflict: Extra(ExtraName("cu124")) }, ConflictItem { package: PackageName("project"), conflict: Extra(ExtraName("cpu")) }} } }
DEBUG fork: ResolverEnvironment { kind: Universal { initial_forks: [platform_system != 'Darwin', platform_system == 'Darwin'], markers: platform_system != 'Darwin', exclude: {ConflictItem { package: PackageName("project"), conflict: Extra(ExtraName("cu124")) }} } }
DEBUG fork: ResolverEnvironment { kind: Universal { initial_forks: [platform_system != 'Darwin', platform_system == 'Darwin'], markers: platform_system != 'Darwin', exclude: {ConflictItem { package: PackageName("project"), conflict: Extra(ExtraName("cpu")) }} } }
```

But those aren't "differentiated" in their markers.

---

_Comment by @BurntSushi on 2024-11-21 12:38_

Hmmm. This might be a separate issue from #9289. I think the problem here is that we've traditionally assumed that forks are disjoint based on markers. But with forking based on extras, multiple forks can have the same markers. So we might be able to just de-deduplicate them?

---

_Comment by @charliermarsh on 2024-11-21 12:46_

Maybe, I’m not sure though… The forks do represent something different than one another, but it’s not captured by the markers. Is that ok?

---

_Comment by @BurntSushi on 2024-11-21 12:51_

It should be captured by the conflicts written to the lock file.

I guess I do wonder if perhaps the forks need their markers associated with the specific conflicts that arise for that fork during resolution.

---

_Comment by @charliermarsh on 2024-11-21 13:58_

Yeah we may be able to dedupe. It just feels slightly off, so I'm unsure. Maybe just think on it as you solve the deeper issue around transitive deps with conflicts etc.

---

_Comment by @charliermarsh on 2024-12-07 00:51_

@BurntSushi -- Do you know how this changes with your PR?

---

_Comment by @BurntSushi on 2024-12-09 13:28_

I don't think there are any specific changes. It would, I believe, be pretty straight-forward to include conflict markers in the lock file's `resolution-markers` now, but I don't know what the full implications of that are. I don't think we have any cases where conflict markers are required in that context.

---

_Assigned to @BurntSushi by @BurntSushi on 2024-12-09 16:27_

---

_Comment by @BurntSushi on 2024-12-10 15:10_

I've thought about this, and while I'm not 100% certain about it, I can't think of a case where we need to record the conflict markers in `resolution-markers`. So I'm going to submit a PR that just de-duplicates them. At the very least, this can't be _more_ wrong than the status quo.

---

_Closed by @BurntSushi on 2024-12-10 19:58_

---

_Closed by @BurntSushi on 2024-12-10 19:58_

---

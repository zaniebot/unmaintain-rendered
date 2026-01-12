```yaml
number: 7769
title: "Allow multiple pinned indexes in `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/index-api-multiple-indexes
created_at: 2024-09-28T23:24:22Z
updated_at: 2024-10-15T22:58:16Z
url: https://github.com/astral-sh/uv/pull/7769
synced_at: 2026-01-12T16:07:59Z
```

# Allow multiple pinned indexes in `tool.uv.sources`

---

_@charliermarsh_

## Summary

This PR lifts the restriction that a package must come from a single index. For example, you can now do:

```toml
[project]
name = "project"
version = "0.1.0"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["jinja2"]

[tool.uv.sources]
jinja2 = [
    { index = "torch-cu118", marker = "sys_platform == 'darwin'"},
    { index = "torch-cu124", marker = "sys_platform != 'darwin'"},
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"

[[tool.uv.index]]
name = "torch-cu124"
url = "https://download.pytorch.org/whl/cu124"
```

The construction is very similar to the way we handle URLs today: you can have multiple URLs for a given package, but they must appear in disjoint forks. So most of the code is just adding that abstraction to the resolver, following our handling of URLs.

Closes #7761.

---

_Label `enhancement` added by @charliermarsh on 2024-09-28 23:24_

---

_Marked ready for review by @charliermarsh on 2024-09-28 23:29_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-28 23:29_

---

_Review requested from @konstin by @charliermarsh on 2024-09-28 23:29_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:14693 on 2024-09-29 00:15_

This one actually fails:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["jinja2>=3"]

[tool.uv.sources]
jinja2 = [
    { index = "torch-cu118", marker = "sys_platform == 'win32'"},
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"
```

(It succeeds if you use `explicit = true` on the index.)

The issue is that the `sys_platform == 'win32'` branch uses the `torch-cu118` index explicitly, but then the implied `sys_platform != 'win32'` branch _also_ resolves to that same index.

---

_@charliermarsh reviewed on 2024-09-29 00:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:878 on 2024-09-30 09:44_

?

---

_Review comment by @konstin on `crates/uv-resolver/src/fork_indexes.rs`:45 on 2024-09-30 09:48_

nit:

```suggestion
        if let Some(previous) = self.0.insert(package_name.clone(), index.clone()) {
            if &previous != index {
                let mut conflicts = vec![previous.to_string(), index.to_string()];
                conflicts.sort();
                return match fork_markers {
                    ResolverMarkers::Universal { .. } | ResolverMarkers::SpecificEnvironment(_) => {
                        Err(ResolveError::ConflictingIndexesUniversal(
                            package_name.clone(),
                            conflicts,
                        ))
                    }
                    ResolverMarkers::Fork(fork_markers) => {
                        Err(ResolveError::ConflictingIndexesFork {
                            package_name: package_name.clone(),
                            indexes: conflicts,
                            fork_markers: fork_markers.clone(),
                        })
                    }
                };
            }
        }
```

---

_@konstin approved on 2024-09-30 09:48_

YES

Mark this bold in the release notes

---

_@zanieb approved on 2024-10-01 14:01_

---

_Comment by @zanieb on 2024-10-01 14:01_

Looks like this needs a docs update though.

---

_Comment by @chitralverma on 2024-10-03 08:52_

would love to use this. looking forward to a release with this and some updated docs!
any ideas on when we can expect this?

---

_Comment by @charliermarsh on 2024-10-03 11:53_

Probably next week just because I'm traveling right now.

---

_Comment by @chitralverma on 2024-10-12 04:54_

Hi @charliermarsh any updates on this one, anything I can help with?

---

_Comment by @charliermarsh on 2024-10-12 04:56_

No, it’ll ship on Monday.

---

_Comment by @chitralverma on 2024-10-14 05:14_

@charliermarsh can we also publish some new docs for torch installation with this PR?

---

_Merged by @charliermarsh on 2024-10-15 22:58_

---

_Closed by @charliermarsh on 2024-10-15 22:58_

---

_Branch deleted on 2024-10-15 22:58_

---

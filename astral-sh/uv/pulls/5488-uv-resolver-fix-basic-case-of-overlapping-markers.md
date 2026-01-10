```yaml
number: 5488
title: "uv-resolver: fix basic case of overlapping markers"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/overlapping-markers-basic
created_at: 2024-07-26T18:43:30Z
updated_at: 2024-08-23T15:32:15Z
url: https://github.com/astral-sh/uv/pull/5488
synced_at: 2026-01-10T13:09:50Z
```

# uv-resolver: fix basic case of overlapping markers

---

_Pull request opened by @BurntSushi on 2024-07-26 18:43_

Consider the following packse scenario:

```toml
[root]
requires = [
  "a>=1.0.0 ; python_version < '3.10'",
  "a>=1.1.0 ; python_version >= '3.10'",
  "a>=1.2.0 ; python_version >= '3.11'",
]

[packages.a.versions."1.0.0"]
[packages.a.versions."1.1.0"]
[packages.a.versions."1.2.0"]
```

On current `main`, this produces a dependency on `a` that looks like
this:

```toml
dependencies = [
    { name = "fork-overlapping-markers-basic-a", marker = "python_version < '3.10' or python_version >= '3.11'" },
]
```

But the marker expression is clearly wrong here, since it implies that
`a` isn't installed at all for Python 3.10. With this PR, the above
dependency becomes:

```toml
dependencies = [
    { name = "fork-overlapping-markers-basic-a" },
]
```

That is, it's unconditional. Which is I believe correct here since there
aren't any other constraints on which version to select.

The specific bug here is that when we found overlapping dependency
specifications for the same package *within* a pre-existing fork, we
intersected all of their marker expressions instead of unioning them.
That in turn resulted in incorrect marker expressions.

While this doesn't fix any known bug on the issue tracker (like #4640),
it does appear to fix a couple of our snapshot tests. And fixes a basic
test case I came up with while working on #4732.

For the packse scenario test: https://github.com/astral-sh/packse/pull/206


---

_Review requested from @konstin by @BurntSushi on 2024-07-26 18:43_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-26 18:43_

---

_@charliermarsh approved on 2024-07-26 18:45_

---

_Merged by @BurntSushi on 2024-07-26 19:06_

---

_Closed by @BurntSushi on 2024-07-26 19:06_

---

_Branch deleted on 2024-07-26 19:06_

---

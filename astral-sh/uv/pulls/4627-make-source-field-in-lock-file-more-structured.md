```yaml
number: 4627
title: "make `source` field in lock file more structured"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/rejigger-source
created_at: 2024-06-28T16:17:55Z
updated_at: 2024-07-01T07:19:16Z
url: https://github.com/astral-sh/uv/pull/4627
synced_at: 2026-01-12T16:06:21Z
```

# make `source` field in lock file more structured

---

_@BurntSushi_

The essence of this change is from this:

```toml
source = "editable+."
```

to this:

```toml
source = { editable = "." }
```

And similarly for other source types. For example, instead of `registry+https://...`, we now have:

```toml
source = { registry = "https://..." }
```

Note though that we retain URLs for git sources. For example:

```toml
source = { git = "https://github.com/pytest-dev/iniconfig?rev=93f5930e668c0d1ddf4597e38dd0dea4e2665e7a#93f5930e668c0d1ddf4597e38dd0dea4e2665e7a" }
```

I don't have a super compelling reason for this, but felt like it was
nice to keep the git info in a compact format. But if folks want to see
the git data more structured, I'd be happy to make that change.


---

_Review requested from @konstin by @BurntSushi on 2024-06-28 17:19_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-28 17:19_

---

_@charliermarsh approved on 2024-06-28 18:44_

I'm a fan of the change.

---

_Merged by @BurntSushi on 2024-06-28 19:02_

---

_Closed by @BurntSushi on 2024-06-28 19:02_

---

_Branch deleted on 2024-06-28 19:02_

---

_Comment by @konstin on 2024-07-01 07:19_

Love this! Splitting the git urls would be nice, too

---

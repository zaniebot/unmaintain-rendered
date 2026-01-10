```yaml
number: 9730
title: Fix commits_since_last_tag in version info
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: version
created_at: 2024-12-09T03:23:02Z
updated_at: 2024-12-19T09:16:31Z
url: https://github.com/astral-sh/uv/pull/9730
synced_at: 2026-01-10T12:00:01Z
```

# Fix commits_since_last_tag in version info

---

_Pull request opened by @j178 on 2024-12-09 03:23_

## Summary

Before:
```console
$ cargo run -- --version
uv 0.5.7 (b17902da0 2024-12-09)
```

After:
```console
$ cargo run -- --version
uv 0.5.7+14 (7cd0ab77a 2024-12-09)
```

Currently `cargo run -- --version` does not includes the number of commits since last tag, because `cargo-dist` create non-annotated tag, and
`git log -1 --date=short --abbrev=9 --format='%H %h %cd %(describe)'` use only annoated tags by default.

```console
$ git log -1 --date=short --abbrev=9 --format='%H %h %cd %(describe)'
7cd0ab77a91b57f5c101aa0bd46396ede947f330 7cd0ab77a 2024-12-09
```

To include these tags, use `git log -1 --date=short --abbrev=9 --format='%H %h %cd %(describe:tags)'`, which will display:

```console
$ git log -1 --date=short --abbrev=9 --format='%H %h %cd %(describe:tags)'
7cd0ab77a91b57f5c101aa0bd46396ede947f330 7cd0ab77a 2024-12-09 0.5.7-14-g7cd0ab77a
```


---

_Comment by @zanieb on 2024-12-09 15:36_

Oh they don't use annotated tags? That's surprising. Thanks for the fix though!

---

_Merged by @zanieb on 2024-12-09 15:43_

---

_Closed by @zanieb on 2024-12-09 15:43_

---

_Label `internal` added by @zanieb on 2024-12-09 15:43_

---

_Branch deleted on 2024-12-19 09:16_

---

```yaml
number: 7887
title: "uv tree: include workspace members in roots"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: tree-member
created_at: 2024-10-03T07:48:28Z
updated_at: 2024-11-06T05:48:13Z
url: https://github.com/astral-sh/uv/pull/7887
synced_at: 2026-01-12T16:08:04Z
```

# uv tree: include workspace members in roots

---

_@j178_

## Summary

For a workspace, when one member depends on another, `uv tree` and `uv tree --package` does not show the denpendent member.

For example:

```console
$ uv init project
$ cd project
$ uv init foo
$ uv init bar
$ uv add --package foo bar
$ uv tree
Resolved 3 packages in 4ms
foo v0.1.0
└── bar v0.1.0
project v0.1.0
$ uv tree --package bar
Resolved 3 packages in 5ms
```

This PR makes sure workspace members are always included in the `roots`, even if they are dependencies, so `uv tree` will display them as roots.

After this PR:
```console
$ cargo run -- tree
Resolved 3 packages in 11ms
bar v0.1.0
foo v0.1.0
└── bar v0.1.0
project v0.1.0
$ cargo run -- tree --package bar
Resolved 3 packages in 9ms
bar v0.1.0
```


---

_Renamed from "uv tree: include workspace memebers in roots" to "uv tree: include workspace members in roots" by @j178 on 2024-10-03 07:48_

---

_Review comment by @j178 on `crates/uv-resolver/src/lock/tree.rs`:330 on 2024-10-03 07:50_

These changes come from #7885

---

_@j178 reviewed on 2024-10-03 07:50_

---

_Marked ready for review by @j178 on 2024-10-03 13:19_

---

_Comment by @charliermarsh on 2024-10-04 11:33_

Should we just get rid of `non_roots` entirely then, and rely solely on the list of members?

---

_Converted to draft by @j178 on 2024-10-04 14:06_

---

_Closed by @j178 on 2024-10-25 13:53_

---

_Branch deleted on 2024-11-06 05:48_

---

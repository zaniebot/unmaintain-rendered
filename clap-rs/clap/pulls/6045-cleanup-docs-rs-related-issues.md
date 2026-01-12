```yaml
number: 6045
title: Cleanup docs.rs related issues
type: pull_request
state: merged
author: GuillaumeGomez
labels: []
assignees: []
merged: true
base: master
head: cleanup-docsrs
created_at: 2025-06-25T14:30:37Z
updated_at: 2025-06-26T14:35:10Z
url: https://github.com/clap-rs/clap/pull/6045
synced_at: 2026-01-12T16:14:17Z
```

# Cleanup docs.rs related issues

---

_@GuillaumeGomez_

This PR fixes an intra-doc link warning and remove unneeded `--cfg=docsrs` set in `Cargo.toml` files.

Also: the `--generate-link-to-definition` currently doesn't work and I think it's because docs.rs related values are not read in workspace `Cargo.toml` files. I think it should be declared in all sub-`Cargo.toml` files.

---

_@pksunkara approved on 2025-06-25 14:43_

---

_@epage reviewed on 2025-06-26 14:33_

---

_Review comment by @epage on `src/_tutorial.rs`:165 on 2025-06-26 14:33_

Which warning is this?  I'm not seeing it with our CI

---

_@epage reviewed on 2025-06-26 14:35_

---

_Review comment by @epage on `Cargo.toml`:130 on 2025-06-26 14:35_

This entry is mostly managed by our project template at https://github.com/epage/_rust

I removed the redundant `--cfg docsrs` in https://github.com/epage/_rust/commit/12af6b5ef664062f834b91cd3ade224c90d0c4a9

I'm not aware of a problem with this being redundant, so I figure we can wait until I next do an update from the template

---

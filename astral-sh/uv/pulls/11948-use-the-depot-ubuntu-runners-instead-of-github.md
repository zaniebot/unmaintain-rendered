```yaml
number: 11948
title: Use the Depot Ubuntu runners instead of GitHub for release workflows
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - releases
assignees: []
merged: true
base: main
head: zb/release-runners
created_at: 2025-03-04T14:07:11Z
updated_at: 2025-03-04T19:28:16Z
url: https://github.com/astral-sh/uv/pull/11948
synced_at: 2026-01-12T16:10:04Z
```

# Use the Depot Ubuntu runners instead of GitHub for release workflows

---

_@zanieb_

See

- https://opensource.axo.dev/cargo-dist/book/reference/config.html#github-custom-runners
- https://github.com/axodotdev/cargo-dist/issues/1760
- #11935

---

_Label `internal` added by @zanieb on 2025-03-04 14:07_

---

_Label `releases` added by @zanieb on 2025-03-04 14:07_

---

_Renamed from "Switch to Depot latest" to "Use the Depot Ubuntu runners instead of GitHub for release workflows" by @zanieb on 2025-03-04 14:07_

---

_Review requested from @Gankra by @zanieb on 2025-03-04 14:23_

---

_Review requested from @konstin by @zanieb on 2025-03-04 14:23_

---

_@Gankra approved on 2025-03-04 18:58_

I believe this should work fine, yes.

The only thing I might be paranoid about is if this will confuse some glibc-version-fallback code that may-or-may-not exist (forcing users onto musl-static over-agressively, which isn't a catastrophe).

---

_Comment by @zanieb on 2025-03-04 19:08_

glibc-verison-fallback code in axo?

---

_Comment by @Gankra on 2025-03-04 19:19_

So normally when cargo-dist is controlling the builds it takes some time on the build-machine to go "hey you're linking glibc, what version of glibc was installed on this machine". It then gathers up all those versions and injects them into the installer-shell-scripts as constraints to check before using the glibc-dynamic version, with the musl-static version being available as fallback if the version check fails at install-time.

However in situations like ours, where we control the build tasks, it doesn't have that metadata, and needs to either discard the check entirely or set a default glibc version to use for the check. This was bleeding-edge when I stopped working on it, so it's possible that logic has evolved, and I don't recall exactly how it plays out for astral... oh hah just below the line you're editing, ok cool we're good (these values are indeed reflected in the installer.sh):

```
workspace.metadata.dist.min-glibc-version]
# Override glibc version for specific target triplets.
aarch64-unknown-linux-gnu = "2.28"
# Override all remaining glibc versions.
"*" = "2.17"
```

---

_Comment by @zanieb on 2025-03-04 19:28_

Cool, thanks for confirming :)

---

_Marked ready for review by @zanieb on 2025-03-04 19:28_

---

_Merged by @zanieb on 2025-03-04 19:28_

---

_Closed by @zanieb on 2025-03-04 19:28_

---

_Branch deleted on 2025-03-04 19:28_

---

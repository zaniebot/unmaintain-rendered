```yaml
number: 3013
title: "Avoid calling `normalize_path` with relative paths that extend beyond the current directory"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/abs
created_at: 2024-04-12T14:25:11Z
updated_at: 2024-04-12T18:48:04Z
url: https://github.com/astral-sh/uv/pull/3013
synced_at: 2026-01-10T14:43:31Z
```

# Avoid calling `normalize_path` with relative paths that extend beyond the current directory

---

_Pull request opened by @charliermarsh on 2024-04-12 14:25_

## Summary

It turns out that `normalize_path` (sourced from Cargo) has a subtle bug. If you pass it a relative path that traverses beyond the root, it silently drops components. So, e.g., passing `../foo/bar`, it will just drop the leading `..` and return `foo/bar`.

This PR encodes that behavior as a `Result` and avoids using it in such cases.

Closes https://github.com/astral-sh/uv/issues/3012.


---

_Review requested from @konstin by @charliermarsh on 2024-04-12 14:25_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-12 14:25_

---

_Marked ready for review by @charliermarsh on 2024-04-12 14:25_

---

_Label `bug` added by @charliermarsh on 2024-04-12 14:25_

---

_@charliermarsh reviewed on 2024-04-12 14:26_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:497 on 2024-04-12 14:26_

@konstin - I think it's okay to cache under the canonical path here rather than the executable path... since we already use the canonical path for the timestamp. It could cause issues with executables that are symlinks (that's a common source of bugs here), but I think it's still fine since we're not using the canonical path to _query_ the executable, which is what tends to cause issues.

---

_@zanieb reviewed on 2024-04-12 14:27_

---

_Review comment by @zanieb on `crates/uv-fs/src/path.rs`:86 on 2024-04-12 14:27_

Why would we want to remove those? Should we add a debug assertion that they're not present at the beginning of the path?

---

_@charliermarsh reviewed on 2024-04-12 14:31_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:491 on 2024-04-12 14:31_

\cc @BurntSushi - I changed this to just hash the path directly. Not sure if it's equivalent?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/verbatim_url.rs`:40 on 2024-04-12 14:37_

Can we panic if it isn't?

---

_Review comment by @BurntSushi on `crates/uv-fs/src/path.rs`:88 on 2024-04-12 14:38_

Same here. Can we panic if it isn't absolute?

---

_Review comment by @BurntSushi on `crates/uv-interpreter/src/interpreter.rs`:491 on 2024-04-12 14:39_

It looks like it isn't: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=df4e201815ac56439741d3646c18ad70

---

_Review comment by @BurntSushi on `crates/uv-interpreter/src/interpreter.rs`:491 on 2024-04-12 14:40_

I'm trying to grok why you need the hashes to be the same here. I'm not understanding that park. Like, even if this generates a different hash, that seems okay, it just means you'll get a cache miss? Or is there something deeper here that I'm missing?

---

_@BurntSushi reviewed on 2024-04-12 14:40_

---

_@charliermarsh reviewed on 2024-04-12 14:52_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:491 on 2024-04-12 14:52_

They don't need to be the same!

---

_@charliermarsh reviewed on 2024-04-12 14:54_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:40 on 2024-04-12 14:54_

Yeah.

---

_@charliermarsh reviewed on 2024-04-12 14:54_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/path.rs`:88 on 2024-04-12 14:54_

Yeah.

---

_@charliermarsh reviewed on 2024-04-12 14:54_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/path.rs`:86 on 2024-04-12 14:54_

Why would we want to remove `..`?

---

_@charliermarsh reviewed on 2024-04-12 14:55_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/path.rs`:86 on 2024-04-12 14:55_

Is that what you're asking?

---

_@charliermarsh reviewed on 2024-04-12 14:58_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:491 on 2024-04-12 14:58_

It was mostly a Chesterton's Fence of trying to understand why we did this in the first place.

---

_Comment by @charliermarsh on 2024-04-12 15:15_

I guess `normalize_path` doesn't actually _require_ an absolute path. It's just an error case that you could have more `..` segments than you have parents. I'll change this to return `Result` if we try to pop in that case.

---

_Converted to draft by @charliermarsh on 2024-04-12 16:01_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-12 18:30_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-12 18:30_

---

_Renamed from "Avoid calling `normalize_path` on possibly-relative paths" to "Avoid calling `normalize_path` with relative paths that extend beyond the current directory" by @charliermarsh on 2024-04-12 18:30_

---

_Marked ready for review by @charliermarsh on 2024-04-12 18:31_

---

_Comment by @charliermarsh on 2024-04-12 18:31_

Okay, could use another review here.

---

_@BurntSushi approved on 2024-04-12 18:40_

LGTM

---

_Merged by @charliermarsh on 2024-04-12 18:48_

---

_Closed by @charliermarsh on 2024-04-12 18:48_

---

_Branch deleted on 2024-04-12 18:48_

---

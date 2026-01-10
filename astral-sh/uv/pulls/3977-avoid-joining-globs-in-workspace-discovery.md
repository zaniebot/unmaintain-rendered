```yaml
number: 3977
title: Avoid joining globs in workspace discovery
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/universal-lock
created_at: 2024-06-03T01:39:00Z
updated_at: 2024-07-06T19:32:47Z
url: https://github.com/astral-sh/uv/pull/3977
synced_at: 2026-01-10T13:48:28Z
```

# Avoid joining globs in workspace discovery

---

_Pull request opened by @charliermarsh on 2024-06-03 01:39_

## Summary

The current approach feels not-quite-right to me. We already have a valid glob, and then we join it onto the path and have to re-parse the pattern. This PR instead uses `WalkDir` to manually recurse over the entries and find matches. We can also avoid testing against files, since we know we're only looking for directories.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-03 01:39_

---

_Marked ready for review by @charliermarsh on 2024-06-03 01:40_

---

_Label `internal` added by @charliermarsh on 2024-06-03 01:40_

---

_Review requested from @konstin by @charliermarsh on 2024-06-03 01:40_

---

_@charliermarsh reviewed on 2024-06-03 01:44_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-03 01:44_

I have a vague feeling that this isn't correct for some recursive patterns? Like if you had `members = ["crates/**"]`, then we'd only return the top-level directory? But that might be nonsensical for workspaces anyway?

I'm not sure.

But if I don't do this, then we discover files like `crates/uv/src` in addition to `crates/uv`, given `members = ["crates/*"]`.

---

_@konstin reviewed on 2024-06-03 10:15_

---

_Review comment by @konstin on `crates/uv-distribution/src/workspace.rs`:838 on 2024-06-03 10:15_

What if there's let say a docker `root:root` dir in my project directory (not included in `members`), would we error here?

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:838 on 2024-06-03 18:40_

Yeah. For recursive directory traversal, you probably just want to log errors from `Result<DirEntry, walkdir::Error>` and otherwise move on.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:813 on 2024-06-03 18:41_

It looks like this iterator _only_ returns directory paths? If so, I think that'd be good to document here.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:845 on 2024-06-03 18:44_

I think you _only_ want the second condition here. That is, you always want to match the paths with globs _relative_ to the project root. And every path returned by `walkdir` is, I believe, guaranteed to always have the [root path prefix](https://docs.rs/walkdir/latest/walkdir/struct.DirEntry.html#method.path).

Stated differently, I think the first `self.glob.matches_path(entry.path())` should be removed here.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-03 18:47_

Hmmm. Should there be more detection logic than just "matches glob"? So yeah, this iterator would return `crates/uv/src`, but if `crates/uv/src` doesn't have a `Cargo.toml` in it, then it can't be a package directory right? I'm not sure if that filtering logic should be here or not, but it seems like it's definitely wrong to skip recursion here.

---

_@BurntSushi reviewed on 2024-06-03 18:47_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-04 00:09_

We do check that elsewhere, but I think we require that any discovered directories include a `Cargo.toml`, rather than _using_ `Cargo.toml` to determine if it's a package. Cargo has the same behavior. E.g., changing uv to use `members = ["crates/*", "python/*"]` gives:

```
❯ cargo check
error: failed to load manifest for workspace member `/Users/crmarsh/workspace/uv/python/uv`
referenced by workspace at `/Users/crmarsh/workspace/uv/Cargo.toml`

Caused by:
  failed to read `/Users/crmarsh/workspace/uv/python/uv/Cargo.toml`

Caused by:
  No such file or directory (os error 2)
```

---

_@charliermarsh reviewed on 2024-06-04 00:09_

---

_@charliermarsh reviewed on 2024-06-04 00:10_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-04 00:10_

If I could check whether the glob was recursive, this would be easy. `glob` does that, but it doesn't expose `is_recursive` on `glob`.

---

_@charliermarsh reviewed on 2024-06-04 00:27_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-04 00:27_

I guess the other option (implemented here) is `require_literal_separator` which means `members = ["crates/*"]` only matches at the top-level anyway.

---

_@BurntSushi reviewed on 2024-06-04 10:45_

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-04 10:45_

Yeah setting `require_literal_separator` is probably a good idea anyway, especially when recursive globs are supported.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/workspace.rs`:806 on 2024-06-04 10:47_

You might be able to just use `PathBuf` here? It looks like `next` below can never return an error. (This is what I would expect I think.)

---

_@BurntSushi approved on 2024-06-04 10:47_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:849 on 2024-06-04 19:45_

IDK I'm really torn on what the right behavior is here. I feel like I'm unnecessarily recursing in the common case. Glob has:
```rust
if idx == self.dir_patterns.len() - 1 {
    // it is not possible for a pattern to match a directory
    // *AND* its children so we don't need to check the
    // children

    if !self.require_dir || is_dir(&path) {
        return Some(Ok(path));
    }
}
```

But that only applies if they don't have `self.dir_patterns[idx].is_recursive`? Maybe this whole change is dumb.

---

_@charliermarsh reviewed on 2024-06-04 19:45_

---

_Closed by @charliermarsh on 2024-07-06 19:32_

---

_Comment by @charliermarsh on 2024-07-06 19:32_

Can revisit later.

---

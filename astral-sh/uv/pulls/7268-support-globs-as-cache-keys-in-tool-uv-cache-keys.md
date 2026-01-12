```yaml
number: 7268
title: "Support globs as cache keys in `tool.uv.cache-keys`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - configuration
assignees: []
merged: true
base: main
head: charlie/glob
created_at: 2024-09-10T18:37:56Z
updated_at: 2024-09-11T19:31:01Z
url: https://github.com/astral-sh/uv/pull/7268
synced_at: 2026-01-12T16:07:45Z
```

# Support globs as cache keys in `tool.uv.cache-keys`

---

_@charliermarsh_

## Summary

This has been asked for a few times. There are risks that these checks could be slow, but they're buyer-beware.

Closes https://github.com/astral-sh/uv/issues/7246.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-10 18:37_

---

_Review requested from @konstin by @charliermarsh on 2024-09-10 18:37_

---

_Marked ready for review by @charliermarsh on 2024-09-10 18:39_

---

_Label `enhancement` added by @charliermarsh on 2024-09-10 18:39_

---

_Label `configuration` added by @charliermarsh on 2024-09-10 18:39_

---

_Review comment by @konstin on `crates/uv-cache-info/src/cache_info.rs`:87 on 2024-09-10 18:39_

What's the motivation for these?

---

_@konstin approved on 2024-09-10 18:43_

---

_@charliermarsh reviewed on 2024-09-10 18:49_

---

_Review comment by @charliermarsh on `crates/uv-cache-info/src/cache_info.rs`:87 on 2024-09-10 18:49_

I think `require_literal_separator` is the only one that doesn't match the default (I was wrong, I need to change `case_sensitive` to `true`). Without `require_literal_separator`, `*.py` matches subdirectories, which I think is wrong?

---

_@charliermarsh reviewed on 2024-09-10 18:49_

---

_Review comment by @charliermarsh on `crates/uv-cache-info/src/cache_info.rs`:87 on 2024-09-10 18:49_

@BurntSushi would know though. Without `require_literal_separator`, I think `**` and `*` are identical.

---

_Comment by @charliermarsh on 2024-09-10 18:54_

Honestly, we might want to have some set of directories that we manually exclude from search, like Ruff's `exclude` feature... `.git`, etc.

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/cache_info.rs`:87 on 2024-09-11 18:40_

I think that since `**` is supported, making `*` not match through a `/` makes sense. Because users can use `**/*.py` to get the behavior of "match any `.py` file in any sub-directory."

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-11 18:41_

How come you ended up going this route instead of `globwalk`?

---

_Review comment by @BurntSushi on `crates/uv-settings/src/settings.rs`:65 on 2024-09-11 18:41_

:+1: 

---

_@BurntSushi approved on 2024-09-11 18:41_

---

_@charliermarsh reviewed on 2024-09-11 18:45_

---

_Review comment by @charliermarsh on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-11 18:45_

It seemed a bit simpler given that we tend to have one glob if any. What do you think?

---

_@BurntSushi reviewed on 2024-09-11 19:00_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-11 19:00_

Aside from this needing to do a directory traversal for each glob (which would be mitigated I suppose if only one glob is used, but I'm not sure if we know that's the common case), my main gripe would be that `glob` doesn't support curly brackets. And switching to another glob implementation could have subtle breaking changes in the future.

---

_@BurntSushi reviewed on 2024-09-11 19:01_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-11 19:01_

These are probably super rare cases, but e.g., `foo{a,b}bar` is interpreted literally by `glob` (I believe), where as `globset` will treat it as `fooabar OR `foobbar`.

---

_@charliermarsh reviewed on 2024-09-11 19:30_

---

_Review comment by @charliermarsh on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-11 19:30_

Ok I'm gonna switch it up. But probably in a separate PR.

---

_Merged by @charliermarsh on 2024-09-11 19:31_

---

_Closed by @charliermarsh on 2024-09-11 19:31_

---

_Branch deleted on 2024-09-11 19:31_

---

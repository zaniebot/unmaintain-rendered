```yaml
number: 10361
title: "Don't revalidate Python interpreter cache entry with `--upgrade`"
type: pull_request
state: closed
author: konstin
labels:
  - performance
assignees: []
base: main
head: konsti/dont-revalidate-python
created_at: 2025-01-07T13:52:08Z
updated_at: 2025-03-10T23:06:19Z
url: https://github.com/astral-sh/uv/pull/10361
synced_at: 2026-01-10T11:10:34Z
```

# Don't revalidate Python interpreter cache entry with `--upgrade`

---

_Pull request opened by @konstin on 2025-01-07 13:52_

We were always revalidating the Python interpreter cache entry when using `--upgrade`, since the cache timestamp is too recent. Instead, we ignore the usual caching semantics for packages and only compare the timestamp of the interpreter with the recorded timestamp. This avoids the cost for querying the Python interpreter, at the expense of being slightly inconsistent with `Cache` behaving different for python alone (which is imho acceptable since interpreter metadata is already different from package metadata).

Most of the diff is an indentation change.

---

_Label `performance` added by @konstin on 2025-01-07 13:52_

---

_Review requested from @charliermarsh by @konstin on 2025-01-07 13:52_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-07 13:54_

Don't we need to respect `--refresh` though?

---

_@charliermarsh reviewed on 2025-01-07 13:54_

---

_@konstin reviewed on 2025-01-07 14:04_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-07 14:04_

Is `--refresh` intended to also revalidate the python interpreter querying? Querying Python is kinda costly, so I'd like to find a way to not do it unless we have, e.g. not query python with just `--upgrade`.

---

_@charliermarsh reviewed on 2025-01-07 14:08_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-07 14:08_

Yeah. I think it's the _only_ way to revalidate it, right?

---

_@konstin reviewed on 2025-01-07 14:11_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-07 14:11_

We have the internal timestamp check, so when the file changes we revalidate. It's different from packages because python is local, so we're always performing the "revalidation request" by checking the timestamp on the file. Are there scenarios where the python interpreter metadata changes while the timestamp would indicate it's fresh?

---

_@charliermarsh reviewed on 2025-01-07 14:16_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-07 14:16_

I... guess not. I mean, for symlinks yes, but we already don't cache for symlinks.

---

_@konstin reviewed on 2025-01-08 08:34_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-08 08:34_

We could add an option to refresh python manually like `uv lock --upgrade --refresh-package python`, but i think not revalidating python by default and saving those extra 20ms is a better default.

---

_@zanieb reviewed on 2025-01-15 19:06_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-15 19:06_

To clarify: does this change also avoid revalidation when `--refresh` is used? Or just `--upgrade`?

---

_@charliermarsh reviewed on 2025-01-15 19:20_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-15 19:20_

Both

---

_@zanieb reviewed on 2025-01-15 19:34_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-15 19:34_

I agree that's surprising and we should continue to refresh this on `--refresh` since that's an explicit request to refresh the cache. On `--upgrade` it seems fine / better to avoid revalidation.

---

_@konstin reviewed on 2025-01-16 11:51_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-16 11:51_

This does not avoid revalidation with `--refresh`, since we're also revalidating the python interpreter by checking the timestamp. It does however not update if the python interpreter changes without the timestamp changing (i don't know how that would be possible except for the user manually changing timestamps, breaking our caching assumptions). This is mirroring the behavior for packages, where we're also trusting the remote with `If-Modified-Since` and only reload the full information with `--refresh` when either the revalidation request fails or the user clears the cache.

---

_@charliermarsh reviewed on 2025-01-16 14:59_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-16 14:59_

I'm confused. On main, `--refresh` will force us to re-query here always. On this branch, `--refresh` will no longer re-query here unless the timestamp has changed. So it does avoid refetching the interpreter data.

---

_@zanieb reviewed on 2025-01-16 15:49_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-16 15:49_

Couldn't the interpreter depend on something else that changes?

> This is mirroring the behavior for packages, where we're also trusting the remote with If-Modified-Since and only reload the full information with --refresh when either the revalidation request fails or the user clears the cache.

I think this is a reasonable argument.

---

_@konstin reviewed on 2025-01-16 16:11_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-16 16:11_

> Couldn't the interpreter depend on something else that changes?

If something like that exists, it would break this PR and i'd change strategies to more aggressively invalidating the python interpreter cache instead. I'm not aware of any such cases, if you have any let me know.

---

_@zanieb reviewed on 2025-01-21 23:48_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:766 on 2025-01-21 23:48_

Here's a great example https://github.com/astral-sh/uv/issues/10832

---

_Comment by @konstin on 2025-03-10 23:06_

We want an option to refresh Python interpreters when using `sys.path` or something similar for package discovery.

---

_Closed by @konstin on 2025-03-10 23:06_

---

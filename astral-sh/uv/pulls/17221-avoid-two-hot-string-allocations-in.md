```yaml
number: 17221
title: Avoid two hot String allocations in deserialization
type: pull_request
state: merged
author: woodruffw
labels:
  - performance
assignees: []
merged: true
base: main
head: ww/alloc
created_at: 2025-12-22T21:20:24Z
updated_at: 2026-01-05T20:26:25Z
url: https://github.com/astral-sh/uv/pull/17221
synced_at: 2026-01-10T05:49:14Z
```

# Avoid two hot String allocations in deserialization

---

_Pull request opened by @woodruffw on 2025-12-22 21:20_

## Summary

Was reading this code while looking at adding PEP 792 support and noticed these: by using the borrowing deserializer instead of the owning one, we can probably save a good number of small-ish String allocations in the `PypiFile` and `PyxFile` deserializers.

(These deserializers in turn are probably pretty hot, since large installations/resolutions require pulling lots of candidates from indices.)

## Test Plan

No functional change expected; I'm not sure what the best way to microbenchmark this is. I'll try and contrive something locally.

---

_Review requested from @charliermarsh by @woodruffw on 2025-12-22 21:20_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-22 21:20_

---

_Label `performance` added by @woodruffw on 2025-12-22 21:20_

---

_@charliermarsh approved on 2025-12-22 21:27_

---

_Merged by @woodruffw on 2025-12-22 21:38_

---

_Closed by @woodruffw on 2025-12-22 21:38_

---

_Branch deleted on 2025-12-22 21:38_

---

_Comment by @woodruffw on 2025-12-25 18:26_

Note: I realized after-the-fact that this _could_ be subtly incorrect. For example, sometimes serde can't borrow a string because it needs to de-escape it from its JSON or other serialized form.

In _practice_ that's probably fine in this context, since we don't really expect any weird escaping to happen on these key names. But it means that we can't just go around swapping `<String>` for `<&str>` with full generality.

Ref: https://github.com/serde-rs/serde/issues/1413

Ref: https://users.rust-lang.org/t/solved-serde-deserialize-str-containig-special-chars/27218

---

_Comment by @konstin on 2026-01-05 18:51_

Can we use `Cow<str, _>` here, which is both correct and doesn't allocate in the regular case?

---

_Comment by @woodruffw on 2026-01-05 20:25_

> Can we use `Cow<str, _>` here, which is both correct and doesn't allocate in the regular case?

Yeah, that would be the most correct/general solution! I think we could probably do the same for every callsite of `String::deserialize` as well, although some of those may not meaningfully benefit from saving a copy if we need an owned String anyways.

For example, `impl<'a> serde::Deserialize<'a> for PythonRequest` can probably be cow'd.

---

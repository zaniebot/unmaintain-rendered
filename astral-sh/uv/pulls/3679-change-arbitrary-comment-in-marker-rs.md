```yaml
number: 3679
title: "Change arbitrary comment in `marker.rs`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - documentation
assignees: []
base: main
head: charlie/true
created_at: 2024-05-20T23:16:32Z
updated_at: 2024-05-20T23:30:33Z
url: https://github.com/astral-sh/uv/pull/3679
synced_at: 2026-01-10T14:32:20Z
```

# Change arbitrary comment in `marker.rs`

---

_Pull request opened by @charliermarsh on 2024-05-20 23:16_

## Summary

My read of the code is that these are always evaluated to `true`, such that they're effectively ignored?


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-20 23:16_

---

_Review requested from @konstin by @charliermarsh on 2024-05-20 23:16_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-20 23:16_

---

_Label `documentation` added by @charliermarsh on 2024-05-20 23:16_

---

_Comment by @charliermarsh on 2024-05-20 23:17_

I don't know if this is the right behavior though. Is it intended?

---

_Comment by @zanieb on 2024-05-20 23:20_

Agree with your read of the code. Also unsure which behavior makes sense. Motivated by https://github.com/astral-sh/uv/issues/3675 I presume?

---

_Comment by @charliermarsh on 2024-05-20 23:22_

Yeah. There's another issue though, that marker shouldn't be parsing to `Arbitrary` (it's valid) but it is currently.

---

_Comment by @ibraheemdev on 2024-05-20 23:24_

I think this is a bug introduced by https://github.com/astral-sh/uv/pull/3520. The old behavior was to evaluate to `false`: https://github.com/astral-sh/uv/blob/0.1.43/crates/pep508-rs/src/marker.rs#L939, except in `evaluate_extras_and_python_version`. I think @konstin pointed the documentation mistake out but I didn't fix the behavior to match the previous implementation.

---

_Comment by @ibraheemdev on 2024-05-20 23:25_

Was https://github.com/astral-sh/uv/issues/3675 a bug introduced by https://github.com/astral-sh/uv/pull/3520?

---

_Comment by @charliermarsh on 2024-05-20 23:25_

Oh ok. I can change it then unless you want to?

---

_Comment by @charliermarsh on 2024-05-20 23:26_

https://github.com/astral-sh/uv/issues/3675 may have been inadvertently triggered by #3520, but there's a real limitation -- we don't respect `in` with version comparators (which is technically allowed). So it's evaluating to `Arbitrary`. Perhaps this previously evaluated to `false` but now evaluates to `true`.

---

_Comment by @charliermarsh on 2024-05-20 23:27_

I was gonna add support for `in` with version operators.

---

_Closed by @charliermarsh on 2024-05-20 23:29_

---

_Comment by @ibraheemdev on 2024-05-20 23:30_

Fine with you making the change. Can also add a little comment to https://github.com/ibraheemdev/uv/blob/26e4805f1c631cc236718bbf8a2b85570da8e695/crates/uv-resolver/src/marker.rs#L38 that explains that `Arbitrary` expressions always evaluate to `true` and so cannot be disjoint.

---

_Comment by @charliermarsh on 2024-05-20 23:30_

Sure thing. Will tag you on review.

---

_Branch deleted on 2024-05-20 23:30_

---

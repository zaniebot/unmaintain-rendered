```yaml
number: 6182
title: Avoid panicking when the resolver thread encounters a closed channel
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/solver-unwrap
created_at: 2024-08-18T14:21:09Z
updated_at: 2024-08-19T13:34:38Z
url: https://github.com/astral-sh/uv/pull/6182
synced_at: 2026-01-10T13:09:50Z
```

# Avoid panicking when the resolver thread encounters a closed channel

---

_Pull request opened by @zanieb on 2024-08-18 14:21_

Closes https://github.com/astral-sh/uv/issues/6167

We've been seeing intermittent failures in CI, which we thought were unexpected HTTP 401s but it actually looks like a panic when handling an expected HTTP error. I believe the problem is that an early client error can cause the channel to close and we crash when we unwrap the `send`.

---

_Label `bug` added by @zanieb on 2024-08-18 14:21_

---

_Review requested from @BurntSushi by @zanieb on 2024-08-18 14:23_

---

_Comment by @zanieb on 2024-08-18 14:25_

Presumably this is a user-facing bug, though we've only seen it in CI so far.

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:284 on 2024-08-18 14:26_

It's possible we need to do this before joining above?

---

_@zanieb reviewed on 2024-08-18 14:26_

---

_@zanieb reviewed on 2024-08-18 14:27_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:284 on 2024-08-18 14:27_

Nope, it deadlocks.

---

_@charliermarsh reviewed on 2024-08-18 16:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:287 on 2024-08-18 16:50_

But, will the error message here be different when we hit this case?

---

_@charliermarsh approved on 2024-08-18 17:01_

Makes sense (though it may not fix the flake per my comment -- I could be wrong though.)

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:287 on 2024-08-18 17:02_

I'm not sure — but the only error case that can be raised here is `send` failing. I'm hoping that the panic was just interrupting proper handling of the HTTP error in another task / thread.

---

_@zanieb reviewed on 2024-08-18 17:02_

---

_@zanieb reviewed on 2024-08-18 17:03_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:287 on 2024-08-18 17:03_

We could test this perhaps by manually throwing an HTTP error? I couldn't reproduce the flake locally though.

---

_@ibraheemdev reviewed on 2024-08-18 18:08_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:287 on 2024-08-18 18:08_

I don't really understand why this would help. A send error should only occur if the `try_join` already returned with an error above.

---

_@ibraheemdev reviewed on 2024-08-18 18:10_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:274 on 2024-08-18 18:10_

I think just ignoring the send error here without the `join` would be an equivalent fix. We do a [similar thing here](https://github.com/astral-sh/uv/blob/main/crates/uv-installer/src/compile.rs#L112).

```suggestion
                let _ = tx.send(result);
```

---

_@zanieb reviewed on 2024-08-18 18:41_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:274 on 2024-08-18 18:41_

I don't have a strong feelings. Also seems fine to join and check for an error? Perhaps vaguely better in case something changes in the future?

---

_@ibraheemdev reviewed on 2024-08-18 18:48_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:274 on 2024-08-18 18:48_

I don't think the `join` is really meaningful because a send error implies that the main thread failed already, so it should never even reach the call to `join`. We shouldn't really be calling `join` anyways because it's blocking, that's why we use the channel for communication.

---

_@zanieb reviewed on 2024-08-18 20:57_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:274 on 2024-08-18 20:57_

Alright.

---

_Merged by @zanieb on 2024-08-18 21:04_

---

_Closed by @zanieb on 2024-08-18 21:04_

---

_Branch deleted on 2024-08-18 21:04_

---

_@BurntSushi reviewed on 2024-08-19 13:34_

Nice find. :-) I think this is a good fix for now, but I generally think the `unwrap()` is correct here, and that the "right" way to fix this is to synchronize and do graceful shutdown when an error occurs. But I suppose the trade-offs for doing this in the case of a CLI tool that is going to stop anyway are somewhat muted.

---

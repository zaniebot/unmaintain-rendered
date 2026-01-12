```yaml
number: 12706
title: Add CRC mode to make CRC checks opt-in by default
type: pull_request
state: closed
author: samypr100
labels: []
assignees: []
base: main
head: crc_mode
created_at: 2025-04-07T03:12:42Z
updated_at: 2025-04-07T19:06:03Z
url: https://github.com/astral-sh/uv/pull/12706
synced_at: 2026-01-12T16:10:21Z
```

# Add CRC mode to make CRC checks opt-in by default

---

_@samypr100_

## Summary

This is a POC quick fix as proposed in https://github.com/astral-sh/uv/issues/12618#issuecomment-2781715588

This adds a `UV_CRC_MODE` env var that allows `enforce` or `lax` to be set or default to `none` which does nothing.

Avoided reworking out the interfaces as that led to a larger change across uv-extract and uv-metadata, but longer term that's probably a better solution.

~~Looking at the dependency graph, uv-distribution-filename was sort-off the best place for CRC Mode, but I'd be happy to move it.~~

## Test Plan

Updated existing crc test.


---

_@charliermarsh reviewed on 2025-04-07 13:56_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:105 on 2025-04-07 13:56_

Should this be `warn_user` so that it appears without verbose logs?

---

_Review requested from @Gankra by @charliermarsh on 2025-04-07 13:57_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-07 13:57_

---

_Comment by @charliermarsh on 2025-04-07 13:57_

LGTM.

---

_@charliermarsh reviewed on 2025-04-07 13:58_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/crc.rs`:21 on 2025-04-07 13:58_

I think I'd put this in `uv-configuration` and have both crates depends on that.

---

_@samypr100 reviewed on 2025-04-07 14:31_

---

_Review comment by @samypr100 on `crates/uv-configuration/src/lib.rs`:32 on 2025-04-07 14:31_

```suggestion
mod crc;
```

---

_@samypr100 reviewed on 2025-04-07 14:34_

---

_Review comment by @samypr100 on `crates/uv-extract/src/stream.rs`:105 on 2025-04-07 14:34_

Fixed in 379fd77689a71dc1faab8bdc88d0e90458f3b1d1

---

_@samypr100 reviewed on 2025-04-07 14:34_

---

_Review comment by @samypr100 on `crates/uv-distribution-filename/src/crc.rs`:21 on 2025-04-07 14:34_

Fixed in 379fd77689a71dc1faab8bdc88d0e90458f3b1d1

---

_Marked ready for review by @samypr100 on 2025-04-07 14:49_

---

_@charliermarsh approved on 2025-04-07 16:18_

---

_Comment by @charliermarsh on 2025-04-07 16:20_

We should probably make this a global setting so it shows up in settings, etc. But I know that's more work, can be a TODO.

---

_@zanieb reviewed on 2025-04-07 16:22_

---

_Review comment by @zanieb on `crates/uv-configuration/src/crc.rs`:10 on 2025-04-07 16:22_

I'm curious why not "Error" and "Warn" here which feel more canonical to me?

---

_@zanieb reviewed on 2025-04-07 16:22_

---

_Review comment by @zanieb on `crates/uv-configuration/src/crc.rs`:10 on 2025-04-07 16:22_

(which would also be "Ignore" instead of "None", I think)

---

_@zanieb reviewed on 2025-04-07 16:24_

---

_Review comment by @zanieb on `crates/uv-configuration/src/crc.rs`:10 on 2025-04-07 16:24_

I wonder if we'd then want a different name? `UV_CRC_CHECK = error | warn | ignore`? I feel less strongly about that.

---

_Comment by @Gankra on 2025-04-07 17:10_

I'm gonna put up a PR for an alternative approach.

---

_@samypr100 reviewed on 2025-04-07 17:31_

---

_Review comment by @samypr100 on `crates/uv-configuration/src/crc.rs`:10 on 2025-04-07 17:31_

Happy to do the change once I'm back to personal laptop

---

_Comment by @Gankra on 2025-04-07 18:56_

I'm going to close this in favour of https://github.com/astral-sh/uv/pull/12722, but if that fix proves insufficient this is a more robust hammer we should revisit.

---

_Closed by @Gankra on 2025-04-07 18:56_

---

_Comment by @zanieb on 2025-04-07 18:58_

Thanks for hoping on this anyway Sam.

---

_Branch deleted on 2025-04-07 19:06_

---

```yaml
number: 14299
title: Fix reachable panic in CLI flag handling
type: pull_request
state: closed
author: gesslerpd
labels: []
assignees: []
base: main
head: feature/flag-panic
created_at: 2025-06-26T21:37:51Z
updated_at: 2025-06-27T10:12:05Z
url: https://github.com/astral-sh/uv/pull/14299
synced_at: 2026-01-12T16:11:08Z
```

# Fix reachable panic in CLI flag handling

---

_@gesslerpd_

## Summary

Previously it was possible to reach panic condition in CLI flag handling when opposing flags were passed at different subcommand levels.

For example `uv self --offline version --no-offline` panics with the following `RUST_BACKTRACE`.

```
thread 'main2' panicked at crates\uv-cli\src\options.rs:17:17:
internal error: entered unreachable code: Clap should make this impossible
stack backtrace:
   0:     0x7ff6ee2a2ffc - <unknown>
   1:     0x7ff6ede968da - <unknown>
   2:     0x7ff6ee2a2557 - <unknown>
   3:     0x7ff6ee2a2e54 - <unknown>
   4:     0x7ff6ee2a2195 - <unknown>
   5:     0x7ff6ee2c96d2 - <unknown>
   6:     0x7ff6ee2c965f - <unknown>
   7:     0x7ff6ee2ccd4e - <unknown>
   8:     0x7ff6f031b941 - <unknown>
   9:     0x7ff6ee4e434a - <unknown>
  10:     0x7ff6ee674184 - <unknown>
  11:     0x7ff6ee65ebde - <unknown>
  12:     0x7ff6ef495290 - <unknown>
  13:     0x7ff6edcc6622 - <unknown>
  14:     0x7ff6ee2c40bd - <unknown>
  15:     0x7ff852477374 - BaseThreadInitThunk
  16:     0x7ff8527dcc91 - RtlUserThreadStart

thread 'main' panicked at C:\a\uv\uv\crates\uv\src\lib.rs:2275:10:
Tokio executor failed, was there a panic?: Any { .. }
stack backtrace:
   0:     0x7ff6ee2a2ffc - <unknown>
   1:     0x7ff6ede968da - <unknown>
   2:     0x7ff6ee2a2557 - <unknown>
   3:     0x7ff6ee2a2e54 - <unknown>
   4:     0x7ff6ee2a2195 - <unknown>
   5:     0x7ff6ee2c9709 - <unknown>
   6:     0x7ff6ee2c965f - <unknown>
   7:     0x7ff6ee2ccd4e - <unknown>
   8:     0x7ff6f031b941 - <unknown>
   9:     0x7ff6f031bd10 - <unknown>
  10:     0x7ff6edda51a2 - <unknown>
  11:     0x7ff6ef494186 - <unknown>
  12:     0x7ff6edda5f92 - <unknown>
  13:     0x7ff6f02f544c - <unknown>
  14:     0x7ff852477374 - BaseThreadInitThunk
  15:     0x7ff8527dcc91 - RtlUserThreadStart
```

## Test Plan

The panic condition is removed in favor of `None` so should be impossible to reach condition now.

Based on the unreachable message, not sure if this is some issue with Clap or uv's usage of it but seems reasonable they would cancel out to `None`.


---

_Comment by @zanieb on 2025-06-26 21:57_

Hm, but this _should_ be unreachable since these use `overrides_with`?

https://github.com/astral-sh/uv/blob/5b2c3595a7afe4539a41702aaaf0409fe571f3ff/crates/uv-cli/src/lib.rs#L239-L244

---

_Comment by @zanieb on 2025-06-26 21:59_

I guess this is a Clap bug?

---

_Comment by @gesslerpd on 2025-06-26 22:02_

Yeah it could be a Clap issue, seems reasonable for UV to make them cancel out for now. Purposely used a non-destructive example to show it but think it has to do with when opposing flags exist at different subcommand levels.

---

_Comment by @zanieb on 2025-06-26 22:11_

Yeah that makes sense. I don't think they should cancel out though, the latter one should "win". If they cancel out, we'll use the default value instead so I think this is a correctness bug if we don't panic.

---

_Comment by @gesslerpd on 2025-06-26 22:30_

I'm not familiar with Clap, since these options are shared across levels I think that following should be equivalent (using harmless repro command)

- `uv --offline --no-offline self version`
- `uv --offline self version --no-offline`

Possibly the top example is not currently a cancel out scenario though, when clap is doing its normal overrides_with behavior?

It's a whatever is last wins?

In that case, I guess then only way to workaround this would be fixing in Clap.


---

_Comment by @gesslerpd on 2025-06-26 23:49_

Closing in favor of issue/pr in https://github.com/clap-rs/clap

---

_Closed by @gesslerpd on 2025-06-26 23:49_

---

_Comment by @konstin on 2025-06-27 10:12_

Filed upstream: https://github.com/clap-rs/clap/issues/6049

---

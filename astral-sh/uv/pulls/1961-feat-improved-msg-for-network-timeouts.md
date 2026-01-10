```yaml
number: 1961
title: "feat: improved msg for network timeouts"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: hint-http-timeout
created_at: 2024-02-25T00:29:39Z
updated_at: 2024-03-01T01:54:48Z
url: https://github.com/astral-sh/uv/pull/1961
synced_at: 2026-01-10T14:54:43Z
```

# feat: improved msg for network timeouts

---

_Pull request opened by @samypr100 on 2024-02-25 00:29_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #1922

When a timeout occurs, it hints to the user to configure the `UV_HTTP_TIMEOUT` env var.

Before
```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: torch==2.2.0 
  Caused by: Failed to extract source distribution
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
```

After
```
error: Failed to download distributions
  Caused by: Failed to fetch wheel: torch==2.2.0 
  Caused by: Failed to extract source distribution
  Caused by: Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT.
```

## Test Plan

<!-- How was it tested? -->
Wasn't sure if we'd want a test. If we do, is there a existing mechanism or preferred approach to force a timeout to occur in tests? Maybe set the timeout to 1 and add torch as an install check (although it's possible that could become flaky)?

---

_@charliermarsh approved on 2024-02-25 02:58_

---

_@charliermarsh reviewed on 2024-02-25 02:59_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:50 on 2024-02-25 02:59_

It looks like Zanie suggested `HTTP_TIMEOUT`. Do you mind changing to `HTTP_TIMEOUT`, and displaying the current value too? This is great!

---

_@samypr100 reviewed on 2024-02-25 16:02_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/distribution_database.rs`:50 on 2024-02-25 16:02_

Good point, I decided to use `UV_HTTP_TIMEOUT` for consitency with `registry_client.rs` first check (which seems to prefer `UV_HTTP_TIMEOUT` and `HTTP_TIMEOUT` last). I could also update that `warn_user_once` as part of this PR if we want to be fully consistent.

```rust
            let timeout = env::var("UV_HTTP_TIMEOUT").or_else(|_| env::var("UV_REQUEST_TIMEOUT")).or_else(|_| env::var("HTTP_TIMEOUT")).and_then(|value| {
                value.parse::<u64>()
                    .or_else(|_| {
                        // On parse error, warn and  use the default timeout
                        warn_user_once!("Ignoring invalid value from environment for UV_REQUEST_TIMEOUT. Expected integer number of seconds, got \"{value}\".");
                        Ok(default_timeout)
                    })
            }).unwrap_or(default_timeout);
```

---

_@zanieb reviewed on 2024-02-25 16:05_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:50 on 2024-02-25 16:05_

I think `UV_HTTP_TIMEOUT` is good, it seems better not to recommend changing a value that other clients will respect unless you need to.

---

_@zanieb reviewed on 2024-02-25 16:07_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:50 on 2024-02-25 16:07_

Can we say how long the timeout is currently though? 

---

_Comment by @zanieb on 2024-02-25 16:09_

Thanks for contributing!

I'd really like to explore addressing this at its root too, it seems like our timeout is not correctly configured.

Regarding testing, maybe we'd spin up a temporary webserver that just doesn't respond? That feels beyond the scope of this change though. A manual test should suffice for now.

---

_@samypr100 reviewed on 2024-02-25 17:33_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/distribution_database.rs`:50 on 2024-02-25 17:33_

Added in 65a5c3542357aa1eb69950cc0b4baa316ae82ab2 and 783a4196d09f3ba10a0294529a85528c621ee8fd. I didn't see reqwest allowing us to look at config.timeout, so I ended up exposing our timeout to the RegistryClient struct instead.

---

_Comment by @charliermarsh on 2024-02-25 21:05_

Thanks!

---

_Merged by @charliermarsh on 2024-02-25 21:13_

---

_Closed by @charliermarsh on 2024-02-25 21:13_

---

_Branch deleted on 2024-03-01 01:54_

---

```yaml
number: 1694
title: Control pip timeout duration via environment variable
type: pull_request
state: merged
author: Di-Is
labels:
  - configuration
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-19T14:23:59Z
updated_at: 2024-02-20T04:37:57Z
url: https://github.com/astral-sh/uv/pull/1694
synced_at: 2026-01-10T15:33:24Z
```

# Control pip timeout duration via environment variable

---

_Pull request opened by @Di-Is on 2024-02-19 14:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add the environment variable `UV_REQUEST_TIMEOUT` to allow control over pip timeouts.

Closes #1549 

## Test Plan

I built uv in the repository top Dockerfile, set the timeout to 3 seconds, and ran `uv pip install torch`.
I measured the execution time with the time command and confirmed that the process finished at a value close to the timeout we set.

```bash
root@037c69228cdc:~# time UV_REQUEST_TIMEOUT=3 /uv pip install torch
Resolved 22 packages in 25ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: nvidia-cusolver-cu12==11.4.5.107
  Caused by: Failed to extract source distribution
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out

real    0m3.064s
user    0m0.225s
sys     0m0.240s
```

---

_@zanieb reviewed on 2024-02-19 15:39_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:85 on 2024-02-19 15:39_

Will we silently ignore invalid values here? Should we warn if it can't be parsed?

---

_@zanieb reviewed on 2024-02-19 15:39_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:85 on 2024-02-19 15:39_

Can we log the timeout we're going to use e.g. with `debug!`?

---

_@zanieb reviewed on 2024-02-19 15:40_

Thanks for contributing! I have a couple minor questions.

---

_Label `configuration` added by @zanieb on 2024-02-19 15:40_

---

_Review comment by @Di-Is on `crates/uv-client/src/registry_client.rs`:85 on 2024-02-19 22:19_

> Will we silently ignore invalid values here? Should we warn if it can't be parsed?
> Can we log the timeout we're going to use e.g. with `debug!`?

@zanieb 
Thanks for your review.
Yes, I think it would be more user-friendly if you suggested it.
I have fixed the implementation and would appreciate a check.

---

_@Di-Is reviewed on 2024-02-19 22:19_

---

_@zanieb reviewed on 2024-02-20 00:15_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:89 on 2024-02-20 00:15_

We have a `warn_user_once!` macro that we should use instead here.

---

_@zanieb reviewed on 2024-02-20 00:16_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:89 on 2024-02-20 00:16_

Would it make sense to use `.map_err` instead of a `match` here?

---

_@zanieb reviewed on 2024-02-20 00:19_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:89 on 2024-02-20 00:19_

I'd also probably be a little clearer here, maybe:

> Ignoring invalid value for UV_REQUEST_TIMEOUT. Expected integer number of seconds, got "{value}".

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:96 on 2024-02-20 02:00_

We should probably not say "Pip" this is our general registry client 
```suggestion
            debug!("Using registry request timeout of {}s", timeout);
```

---

_@zanieb reviewed on 2024-02-20 02:00_

---

_Comment by @Di-Is on 2024-02-20 02:22_

@zanieb
Sorry, I deleted some commits because I had the wrong user to push.
I have re-committed it and would appreciate it if you could check it.

---

_@zanieb approved on 2024-02-20 04:37_

Thanks!

---

_Merged by @zanieb on 2024-02-20 04:37_

---

_Closed by @zanieb on 2024-02-20 04:37_

---

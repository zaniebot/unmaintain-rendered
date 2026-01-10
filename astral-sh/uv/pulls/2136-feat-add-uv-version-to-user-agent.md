```yaml
number: 2136
title: "feat: add uv version to user agent"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: version-in-user-agent
created_at: 2024-03-03T02:55:12Z
updated_at: 2024-03-04T21:12:39Z
url: https://github.com/astral-sh/uv/pull/2136
synced_at: 2026-01-10T14:54:43Z
```

# feat: add uv version to user agent

---

_Pull request opened by @samypr100 on 2024-03-03 02:55_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #1977

This allows us to send uv's version in the `uv-client` User Agent header.

Here's how request headers look like to a server now:
```
...
Accept: application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01
User-Agent: uv/0.1.13
...
```

~~I went for a mix of Option 1 and 2 from #1977.~~ Open to alternative naming as well, not tied too strongly here to the names picked.

~~Another possibility for this new crate is that we can use it to consolidate metadata that exists across crates to ultimately be able to create linehaul information described in #1958, but I haven't looked into what those changes might look like.~~

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Added initial tests in the new crate to exercise its public API and added a new test to uv-client to validate the headers using a 1-time disposable server.

---

_Marked ready for review by @samypr100 on 2024-03-03 03:01_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-03 18:01_

---

_@charliermarsh reviewed on 2024-03-03 20:05_

---

_Review comment by @charliermarsh on `crates/uv-config/Cargo.toml`:3 on 2024-03-03 20:05_

Honestly, I'd be okay to just keep this crate's version in-sync with `uv`, and use the env var directly to reduce some complexity. What do you think?

---

_@samypr100 reviewed on 2024-03-03 20:51_

---

_Review comment by @samypr100 on `crates/uv-config/Cargo.toml`:3 on 2024-03-03 20:51_

Here's how it would look e33a19b2cfd6c01d15d456135a411c5ad9ee4fba, it would require a manual bump before a release (for now) but I believe that's automatable.

---

_@charliermarsh reviewed on 2024-03-04 01:32_

---

_Review comment by @charliermarsh on `crates/uv-config/Cargo.toml`:3 on 2024-03-04 01:32_

Our upgrade infra can handle this (I added it to `pyproject.toml`).

---

_@charliermarsh reviewed on 2024-03-04 01:33_

---

_Review comment by @charliermarsh on `crates/uv-client/Cargo.toml`:52 on 2024-03-04 01:33_

Can we write the tests to use `hyper v0.14.28`? `reqwest` isn't on v1.0, and this adds a lot of dependencies which hurts compile times etc.

---

_@samypr100 reviewed on 2024-03-04 02:24_

---

_Review comment by @samypr100 on `crates/uv-client/Cargo.toml`:52 on 2024-03-04 02:24_

Good point, will do.

---

_@samypr100 reviewed on 2024-03-04 03:40_

---

_Review comment by @samypr100 on `crates/uv-config/Cargo.toml`:3 on 2024-03-04 03:40_

TIL about [rooster](https://github.com/zanieb/rooster)

---

_@samypr100 reviewed on 2024-03-04 03:43_

---

_Review comment by @samypr100 on `crates/uv-client/Cargo.toml`:52 on 2024-03-04 03:43_

Done in 35eee6d53bdaecea9777635ac74421af42bf9012 I was surprised about how different some things are in 1.x vs 0.14.x.

---

_Review comment by @charliermarsh on `crates/uv-client/Cargo.toml`:52 on 2024-03-04 19:37_

Thank you!

---

_@charliermarsh reviewed on 2024-03-04 19:37_

---

_@charliermarsh approved on 2024-03-04 19:38_

---

_@charliermarsh reviewed on 2024-03-04 19:39_

---

_Review comment by @charliermarsh on `crates/uv-config/Cargo.toml`:3 on 2024-03-04 19:39_

Yeah, @zanieb built it for Ruff and we use it here too.

---

_Merged by @charliermarsh on 2024-03-04 19:48_

---

_Closed by @charliermarsh on 2024-03-04 19:48_

---

_Branch deleted on 2024-03-04 21:12_

---

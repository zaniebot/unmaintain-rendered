```yaml
number: 4729
title: Enable Registry Client Builder to be created from Base Client Builder
type: pull_request
state: merged
author: danielenricocahall
labels:
  - internal
assignees: []
merged: true
base: main
head: issue-4330-registry-client-builder
created_at: 2024-07-02T12:52:53Z
updated_at: 2024-07-04T15:53:06Z
url: https://github.com/astral-sh/uv/pull/4729
synced_at: 2026-01-12T16:06:25Z
```

# Enable Registry Client Builder to be created from Base Client Builder

---

_@danielenricocahall_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Addresses https://github.com/astral-sh/uv/issues/4330, to reduce duplication in the client creation logic.
## Test Plan

<!-- How was it tested? -->
https://github.com/astral-sh/uv/pull/4729#issuecomment-2204681655


---

_Assigned to @zanieb by @zanieb on 2024-07-02 14:52_

---

_@zanieb reviewed on 2024-07-02 23:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:102 on 2024-07-02 23:28_

Nice! Would you mind auditing for additional cases where this could be used? I think there should be more.

---

_@zanieb reviewed on 2024-07-02 23:30_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-02 23:30_

I was surprised this needed to be `mut` still. Should the client, markers, and platform be set on the base client builder too (e.g., the same as `native_tls`)?

---

_Comment by @zanieb on 2024-07-02 23:30_

I don't think you'll need a specific test plan for this one, it should be well covered by our existing tests.

---

_@danielenricocahall reviewed on 2024-07-03 02:13_

---

_Review comment by @danielenricocahall on `crates/uv/src/commands/project/add.rs`:102 on 2024-07-03 02:13_

Good call! Found 5 more places actually!

---

_@danielenricocahall reviewed on 2024-07-03 02:16_

---

_Review comment by @danielenricocahall on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-03 02:16_

Oh yeah we could definitely do that - would we want to change where each of these `RegistryClientBuilder`s are created to have the `BaseClientBuilder` set those fields?

---

_@zanieb reviewed on 2024-07-03 04:22_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-03 04:22_

I'm not sure I follow.

---

_@danielenricocahall reviewed on 2024-07-03 12:15_

---

_Review comment by @danielenricocahall on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-03 12:15_

Ah I'm probably not explaining myself well - what I mean is would we need to revise each place we're creating a `RegistryClientBuilder` to something like:
```rust
    let client_builder = BaseClientBuilder::new()
        .connectivity(connectivity)
        .native_tls(native_tls)
        .keyring(settings.keyring_provider)
        .markers(markers) // < - added
        .retries(retries); // < - added

...

    let client = RegistryClientBuilder::from(client_builder)
            .index_urls(settings.index_locations.index_urls())
            .index_strategy(settings.index_strategy); // no longer have base client fields set
```

then, in the `build` function, we would omit the conditionals. Is that what you're thinking?

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-03 12:47_

Ah I don't think we need to do that. The `RegistryClientBuilder` can still provide methods to mutate the `base_client_builder` it has internally — it just shouldn't be mutated during `build` instead it should happen each time an option is changed.

---

_@zanieb reviewed on 2024-07-03 12:47_

---

_@danielenricocahall reviewed on 2024-07-04 15:46_

---

_Review comment by @danielenricocahall on `crates/uv-client/src/registry_client.rs`:124 on 2024-07-04 15:46_

Done!

---

_Review requested from @zanieb by @danielenricocahall on 2024-07-04 15:46_

---

_@zanieb approved on 2024-07-04 15:48_

---

_Label `internal` added by @zanieb on 2024-07-04 15:51_

---

_Comment by @zanieb on 2024-07-04 15:52_

Thank you!

---

_Merged by @zanieb on 2024-07-04 15:53_

---

_Closed by @zanieb on 2024-07-04 15:53_

---

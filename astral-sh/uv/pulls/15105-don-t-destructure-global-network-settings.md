```yaml
number: 15105
title: "Don't destructure global network settings"
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/from-network-settings
created_at: 2025-08-06T11:00:12Z
updated_at: 2025-08-29T18:30:53Z
url: https://github.com/astral-sh/uv/pull/15105
synced_at: 2026-01-12T16:11:35Z
```

# Don't destructure global network settings

---

_@konstin_

From reviewing #14687, I was reminded that every base client construction requires manually destructuring the `NetworkSettings`. We can avoid that by passing the global network settings directly.

---

_Label `internal` added by @konstin on 2025-08-06 11:00_

---

_@zanieb reviewed on 2025-08-06 13:11_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:192 on 2025-08-06 13:11_

I feel like these might be used by downstream consumers, these client APIs are a pretty common touch point.

---

_@zanieb reviewed on 2025-08-06 13:11_

---

_Review comment by @zanieb on `crates/uv-requirements/src/upgrade.rs`:40 on 2025-08-06 13:11_

What does this mean?

---

_@zanieb reviewed on 2025-08-06 13:12_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:161 on 2025-08-06 13:12_

Why does this go from being a method to a function?

---

_@zanieb reviewed on 2025-08-06 13:13_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:161 on 2025-08-06 13:13_

Ah I see, because of the types. Hm..

---

_Comment by @zanieb on 2025-08-06 13:14_

I'm not a huge fan of this honestly. I think it breaks the usual structure of our abstractions around settings, which is why it's this way in the first place?

I think I'd rather just reduce the amount of times we construct a client builder? We should be able to do that ~once, right?

---

_Comment by @konstin on 2025-08-06 14:54_

We construct three different kinds of base clients: Without keyring, with keyring and for the registry client. With the exception of the keyring, all builder methods are either network settings, or only used by the registry client (of which there are only two calls, that we could remove by moving from `RegistryClientBuilder::new` to `RegistryClientBuilder::try_from`) [^1].

The `NetworkSettings` struct is effectively the part that we can construct once and share across the codebase. We can construct a single `BaseClientBuilder` with it once and pass it around, but we do need to modify depending on whether we're in a location that has and uses a keyring (which would be the PR two after this one). We can't pass around a single instance of an (immutable) `BaseClient` alone without an option to control the keyring behavior.

[^1]: Ignoring `uv publish` here, which builds a custom client that has to ignore our common patterns already, due to being the only POST client, two reqwest bugs and having different auth.

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:192 on 2025-08-06 14:55_

These are still accessible, the API moved to passing a `NetworkSettings` instance.

---

_@konstin reviewed on 2025-08-06 14:55_

---

_@konstin reviewed on 2025-08-06 14:58_

---

_Review comment by @konstin on `crates/uv-requirements/src/upgrade.rs`:40 on 2025-08-06 14:58_

This is the one location that breaks our pattern because we're actually reading from a file, not doing a network request so having a network client is odd, but I didn't want to change `RequirementsTxt::parse` too

---

_Comment by @zanieb on 2025-08-06 15:09_

I think it seems reasonable to have a single construction of `BaseClientBuilder` (minus some of the special cases above) that includes keyring then _remove_ keyring in the places that shouldn't use it for authentication? I don't actually know why we wouldn't use the keyring in some places, what's the use-case for that?

---

_Comment by @konstin on 2025-08-06 15:21_

I'm not sure about the keyring usage, I would return the question since your more familiar with the `KeyringProvider`.

One difference is that `NetworkSettings` (plus retries) are global CLI settings, while the keyring isn't, so e.g. `uv python install` uses a client but no keyring. Being a global setting, we have the `NetworkSettings` for the single construction of `BaseClientBuilder`, but at that point we don't have the keyring setting yet, we need to match over the CLI subcommand branches for that.

---

_Closed by @zanieb on 2025-08-29 18:30_

---

```yaml
number: 1874
title: Retain authentication attached to URLs when making requests to the same host
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-auth
created_at: 2024-02-22T15:37:51Z
updated_at: 2024-02-22T17:56:39Z
url: https://github.com/astral-sh/uv/pull/1874
synced_at: 2026-01-10T14:54:43Z
```

# Retain authentication attached to URLs when making requests to the same host

---

_Pull request opened by @zanieb on 2024-02-22 15:37_

Closes https://github.com/astral-sh/uv/issues/1860


In https://github.com/astral-sh/uv/pull/1816, we started using the URL attached to a response instead of the request URL for subsequent requests — this fixes various bugs but has the side-effect of dropping credentials from the URL. Here, we transfer credentials from the request URL to the response URL. We perform RFC compliant checks for safety.

---

_Label `bug` added by @zanieb on 2024-02-22 15:37_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-22 15:39_

---

_@MichaReiser reviewed on 2024-02-22 15:41_

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:254 on 2024-02-22 15:41_

We should check the protocol too, to avoid potential downgrades. 


This seems uhm, dangerous 

---

_@zanieb reviewed on 2024-02-22 17:22_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:254 on 2024-02-22 17:22_

Should be resolved by https://github.com/astral-sh/uv/pull/1874/commits/c57ef0b4fe3744965c7222a290bb49b775e11a60

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:530 on 2024-02-22 17:24_

Can we emit the error as part of the warning message log it with `debug` to ease debugging when copying fails for some reason?

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:545 on 2024-02-22 17:24_

Thanks for linking to the relevant sections

---

_@zanieb reviewed on 2024-02-22 17:24_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:575 on 2024-02-22 17:24_

I found this confusing when written in the negative

---

_@zanieb reviewed on 2024-02-22 17:25_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:575 on 2024-02-22 17:25_

I guess I could wrap the whole thing with a `!` but for some reason this _feels_ safer to me :)

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:571 on 2024-02-22 17:26_

I find the `check the port` comment not that useful. I can see that we check something with the port but I'm having difficulty understanding what's being checked. Maybe we can explain that instead?

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:571 on 2024-02-22 17:28_

Not sure if that's the same

```suggestion
            if !request_url.port().is_some_and(|port| port != default_port)
                && !response_url.port().is_some_and(|port| != default_port) 
```

or you change the condition and do an early return instead


```suggestion
            if request_url.port().is_some_and(|port| != default_port)
                || response_url.port().is_some_and(|port| != default_port) {
                return false
          } 
```

What case does this cover: Is it mainly to cover the case where the `request` has the port `None` but the response comes with e.g. port `80`?

---

_@charliermarsh approved on 2024-02-22 17:29_

---

_@MichaReiser approved on 2024-02-22 17:30_

---

_@zanieb reviewed on 2024-02-22 17:32_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:530 on 2024-02-22 17:32_

The error is empty, just `()` — annoying but yep.

---

_@MichaReiser reviewed on 2024-02-22 17:34_

---

_Review comment by @MichaReiser on `crates/uv-client/src/registry_client.rs`:530 on 2024-02-22 17:34_

Oh, that's unfortunate

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:571 on 2024-02-22 17:34_

This is slightly different... I can try to make it better again. But yeah if you look at the test cases and the specification we are only supposed to check the port if it differs from the default port.

---

_@zanieb reviewed on 2024-02-22 17:34_

---

_@zanieb reviewed on 2024-02-22 17:35_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:571 on 2024-02-22 17:35_

This comment just matches all the previous ones saying which part we're checking, the inner comment is supposed to explain this. I can try to do better though.

---

_@zanieb reviewed on 2024-02-22 17:43_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:571 on 2024-02-22 17:43_

Okay https://github.com/astral-sh/uv/pull/1874/commits/477e18b88d9f825204dbcdb9f3a98a8f4803b159 should be a lot better

---

_Comment by @zanieb on 2024-02-22 17:53_

Fyi I tested this against AWS CodeArtifact.

---

_Merged by @zanieb on 2024-02-22 17:56_

---

_Closed by @zanieb on 2024-02-22 17:56_

---

_Branch deleted on 2024-02-22 17:56_

---

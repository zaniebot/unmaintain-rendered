```yaml
number: 12920
title: Properly handle authentication for 302 redirect URLs
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/redirects
created_at: 2025-04-16T15:23:38Z
updated_at: 2025-04-18T12:56:19Z
url: https://github.com/astral-sh/uv/pull/12920
synced_at: 2026-01-10T11:10:40Z
```

# Properly handle authentication for 302 redirect URLs

---

_Pull request opened by @jtfmumm on 2025-04-16 15:23_

uv was failing to authenticate on 302 redirects when credentials were available. This was because it was relying on `reqwest_middleware`'s default redirect behavior which bypasses the middleware pipeline when trying the redirect request (and hence bypasses our authentication middleware). This PR updates uv to retrigger the middleware pipeline when handling a 302 redirect, correctly using credentials from the URL, the keyring, or `.netrc`. 

Closes #5595
Closes #11097 



---

_Label `bug` added by @jtfmumm on 2025-04-16 15:23_

---

_Review requested from @konstin by @jtfmumm on 2025-04-16 15:29_

---

_@zanieb reviewed on 2025-04-16 16:17_

Awesome that you added tests with a server! This makes sense to me, but we probably want another reviewer here? (@konstin) I'm not sure what the implications are.

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:487 on 2025-04-16 21:43_

Is there a specific reason for making this a struct instead of a function call?

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:536 on 2025-04-16 21:45_

Where does that number come from, can we import it from requests instead of defining it ourselves?

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:534 on 2025-04-16 21:54_

A pattern I like to avoid panicking branches is a

```rust
loop {
    ...
    if result.is_redirect() && redirects < max_redirects {
        redirects += 1;
        continue;
    } else {
        return Err(result);
    }
}
```

E.g.: https://github.com/astral-sh/uv/blob/c4fd34f0631d42d77e26667a496cc0651ba00328/crates/uv-publish/src/lib.rs#L388-L452

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:508 on 2025-04-16 21:57_

(*) We should avoid this unwrap with pattern matching (let-else or match)

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:511 on 2025-04-16 22:01_

(*) Should this apply to other 30x redirect codes too, or only 302?

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:454 on 2025-04-16 22:12_

(*) How did you decide which callers use `execute_with_redirect_handling` and which use `for_host`?

---

_@konstin approved on 2025-04-16 22:15_

Looks good!

I've added some personal taste style comments and some real review comment, i've put a star on the latter ones to differentiate.

---

_@zanieb reviewed on 2025-04-16 22:22_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:508 on 2025-04-16 22:22_

(generally we've very adverse to unwraps)

---

_@jtfmumm reviewed on 2025-04-17 06:58_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:536 on 2025-04-17 06:58_

This is the reqwest default, so I chose it because it is the number we have been implicitly using. We could make it configurable, but I thought that probably falls under YAGNI.

---

_@jtfmumm reviewed on 2025-04-17 07:06_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:511 on 2025-04-17 07:06_

I was trying to limit this to the proxy-redirecting case that motivated the change (in case there are other edge cases we'd have to handle). But I suppose it probably can't hurt to re-run the middleware pipeline on any redirect. Would that be your preference?

---

_@jtfmumm reviewed on 2025-04-17 09:25_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:454 on 2025-04-17 09:25_

I was trying to avoid covering too much of the `reqwest` surface area in this PR, but on reflection I think it is too easy to do the wrong thing when building requests (in particular, to accidentally circumvent the redirect policy). I've refactored to store `RedirectClientWithMiddleware` on `BaseClient` and to wrap `reqwest_middleware::RequestBuilder` to ensure that the configured redirect policy is applied consistently.

---

_@konstin reviewed on 2025-04-17 09:26_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:511 on 2025-04-17 09:26_

I'd opportunistically handle all applicable redirect codes, it feels more coherent and helps users that encounter other redirect codes in the future.

---

_@jtfmumm reviewed on 2025-04-17 09:27_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:487 on 2025-04-17 09:27_

See this [comment](https://github.com/astral-sh/uv/pull/12920#discussion_r2048583057). I had originally intended the more ambitious refactor, which is why this struct exists. I've now implemented it.

---

_Closed by @jtfmumm on 2025-04-17 13:52_

---

_Reopened by @jtfmumm on 2025-04-17 13:52_

---

_@konstin reviewed on 2025-04-17 15:16_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:536 on 2025-04-17 15:16_

Can you add a comment that this number comes from reqwest?


---

_@jtfmumm reviewed on 2025-04-17 15:33_

---

_Review comment by @jtfmumm on `crates/uv-client/src/base_client.rs`:536 on 2025-04-17 15:33_

Added

---

_Merged by @jtfmumm on 2025-04-18 12:56_

---

_Closed by @jtfmumm on 2025-04-18 12:56_

---

_Branch deleted on 2025-04-18 12:56_

---

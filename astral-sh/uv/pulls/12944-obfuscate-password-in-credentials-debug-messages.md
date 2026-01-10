```yaml
number: 12944
title: Obfuscate password in credentials debug messages
type: pull_request
state: merged
author: jtfmumm
labels:
  - security
assignees: []
merged: true
base: main
head: jtfm/obfuscate-password
created_at: 2025-04-17T13:18:03Z
updated_at: 2025-04-20T16:01:15Z
url: https://github.com/astral-sh/uv/pull/12944
synced_at: 2026-01-10T11:10:40Z
```

# Obfuscate password in credentials debug messages

---

_Pull request opened by @jtfmumm on 2025-04-17 13:18_

I noticed in the trace output that we weren't obfuscating the `Credentials` password in a trace message. This PR creates a `Password` newtype with a custom `Debug` implementation.


---

_Label `security` added by @jtfmumm on 2025-04-17 13:18_

---

_Renamed from "Obfuscate password in credentials" to "Obfuscate password in credentials debug messages" by @jtfmumm on 2025-04-17 13:18_

---

_Comment by @zanieb on 2025-04-17 13:47_

See also https://github.com/astral-sh/uv/issues/1714

---

_Comment by @jtfmumm on 2025-04-17 14:09_

> See also #1714

Thanks, I'll address that here as well.

---

_Comment by @jtfmumm on 2025-04-17 14:54_

> See also #1714

I've addressed the first example in your linked issue (`Updating ...`) but I think it's worth a more thorough audit of how we're handling URLs. I suggest we merge this PR first because it's critical and then I can do the audit separately.

---

_@zanieb reviewed on 2025-04-17 16:54_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 16:54_

I feel like we may want to introduce a wrapper type for this, like `ObfuscatedUrl` with a different display? It feels weird both to:

- Mutate the given URL
- Return a type that does not encode that there's obfuscation

It feels like too much of a footgun.

---

_@zanieb reviewed on 2025-04-17 16:55_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 16:55_

(The approach implemented here is quite different from the one you did in the middleware, perhaps we should just separate it entirely?)

---

_@charliermarsh reviewed on 2025-04-17 17:02_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 17:02_

Agreed, I think this should somehow be enforced in the type system (or some kind of `Sensitive` trait).

---

_@jtfmumm reviewed on 2025-04-17 17:04_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 17:04_

Yeah my intention after this PR is to audit the codebase and come up with a unified solution. 

---

_@jtfmumm reviewed on 2025-04-17 17:06_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 17:06_

That was just a quick fix for the issue you linked in the meantime. 

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 17:11_

But the real solution will use the type system. 

---

_@jtfmumm reviewed on 2025-04-17 17:12_

---

_@jtfmumm reviewed on 2025-04-17 17:38_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/credentials.rs`:320 on 2025-04-17 17:38_

I've reverted these changes and just left the main `Credentials` fix. 

---

_Closed by @jtfmumm on 2025-04-17 17:38_

---

_Reopened by @jtfmumm on 2025-04-17 17:38_

---

_@zanieb approved on 2025-04-18 16:10_

As noted in https://github.com/astral-sh/uv/pull/12969#issuecomment-2815718797, we'll probably want a `UV_LOG_SHOW_SECRETS` variable or something to toggle this back on for people that are debugging if their secrets are correct. I don't know if that needs to block this change.

---

_Comment by @jtfmumm on 2025-04-18 17:37_

> As noted in [#12969 (comment)](https://github.com/astral-sh/uv/pull/12969#issuecomment-2815718797), we'll probably want a `UV_LOG_SHOW_SECRETS` variable or something to toggle this back on for people that are debugging if their secrets are correct. I don't know if that needs to block this change.

I'd argue we shouldn't provide the option to log secrets, though admittedly that's pretty strict. That could be part of a bigger conversation. Since this PR doesn't mask usernames (avoiding the other question you raise in your linked comment), I'd say we merge it as is.

---

_Comment by @zanieb on 2025-04-18 19:48_

> I'd argue we shouldn't provide the option to log secrets, though admittedly that's pretty strict. 

Why? It's very helpful for debugging and it seems quite reasonable to allow people to opt-in to?

---

_Comment by @jtfmumm on 2025-04-18 19:57_

> > I'd argue we shouldn't provide the option to log secrets, though admittedly that's pretty strict.
> 
> Why? It's very helpful for debugging and it seems quite reasonable to allow people to opt-in to?

Yeah I agree it’s helpful. But it’s the kind of option that can be accidentally left on. Do other tools in the space provide flags to log secrets?

For debugging purposes we could consider opt in logging of the salted hash of the secret. 


---

_Comment by @zanieb on 2025-04-18 21:01_

I'm not sure about other tools in the space. I looked around a little, but didn't find clear answers. However, I don't really feel concerned about the situation where someone explicitly enables showing secrets in order to debug something then leaves it on by accident? It feels fairly contrived. We could even call it `UV_UNSAFE_SHOW_SECRETS` or whatever, but authentication is a complicated part of uv and we have spent a great deal of time debugging it — I'm very hesitant about losing the ability to get any insight into what credentials are used. I think a salted hash sounds pretty overkill for something like this, I worry it'd just be more confusing.

---

_Comment by @jtfmumm on 2025-04-18 21:42_

A simpler way to put it is that this option allows careless developers to log secrets in production when debugging a problem. I can write something up separately. Is this blocking the PR or should I merge in the meantime?

---

_Comment by @zanieb on 2025-04-18 21:46_

No I don't think it needs to block this change.

If careless developers want to log secrets in production, they'll do so regardless of uv's security posture. We usually design to be secure by default, but trust users to make insecure choices if they need to, e.g., `--insecure-host` and `--index-strategy`.

---

_Merged by @jtfmumm on 2025-04-18 22:01_

---

_Closed by @jtfmumm on 2025-04-18 22:01_

---

_Branch deleted on 2025-04-18 22:01_

---

_Comment by @topher96 on 2025-04-19 11:31_

Regarding password logging - what about a password masking option instead?  Logs are nearly impossible get rid of at a big company due to centralized logging with long retention policies and shared widely to aid multiple levels of support.   I suggest instead of an option to log passwords at any log level (outside of some extreme trace level)  you add an option to only log the first N characters and the total length.  You could add some logic to reduce N further when the length is short.   Also add something grep-able in the string so centralized loggers could mask it out.  The benefit is companies could just leave the option on in at least a quality environment.   For example if my secret was PASSWORD123 it could show {SEC:11}'PA'   - that's enough to be useful for debugging in a broad set of cases.  

---

_Comment by @zanieb on 2025-04-20 16:01_

I've been down that route before :) https://github.com/PrefectHQ/prefect/issues/6707#issuecomment-1248344263

But it's a nice middle-ground for opt-in.

---

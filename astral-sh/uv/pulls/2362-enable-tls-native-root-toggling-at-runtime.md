```yaml
number: 2362
title: Enable TLS native root toggling at runtime
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - breaking
assignees: []
merged: true
base: main
head: charlie/tls
created_at: 2024-03-11T19:31:36Z
updated_at: 2024-03-13T01:55:42Z
url: https://github.com/astral-sh/uv/pull/2362
synced_at: 2026-01-12T16:04:59Z
```

# Enable TLS native root toggling at runtime

---

_@charliermarsh_

## Summary

It turns out that on macOS, reading the native certificates can add hundreds of milliseconds to client initialization. This PR makes `--native-tls` a command-line flag, to toggle (at runtime) the choice of the `webpki` roots or the native system roots.

You can't accomplish this kind of configuration with the `reqwest` builder API, so instead, I pulled out the heart of that logic from the crate (https://github.com/seanmonstar/reqwest/blob/e3192638518d577759dd89da489175b8f992b12f/src/async_impl/client.rs#L498), and modified it to allow toggling a choice of root.

Note that there's an open PR for this in reqwest (https://github.com/seanmonstar/reqwest/pull/1848), along with an issue (https://github.com/seanmonstar/reqwest/issues/1843), which I may ping, but it's been around for a while and I believe reqwest is focused on its next major release.

Closes https://github.com/astral-sh/uv/issues/2346.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-11 20:00_

---

_Marked ready for review by @charliermarsh on 2024-03-11 20:00_

---

_@zanieb reviewed on 2024-03-11 23:08_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:94 on 2024-03-11 23:08_

Should we say a little more about why the system store would be used?

---

_@zanieb reviewed on 2024-03-11 23:09_

---

_Review comment by @zanieb on `README.md`:426 on 2024-03-11 23:09_

I guess I would rephrase since this isn't quite "out of the box" anymore?

---

_@zanieb reviewed on 2024-03-11 23:09_

---

_Review comment by @zanieb on `README.md`:426 on 2024-03-11 23:09_

I'd say... "By default, we use Mozilla's...." as you do in the docstring as well

---

_@zanieb approved on 2024-03-11 23:09_

Great work!

---

_Comment by @zanieb on 2024-03-11 23:10_

Technically this is a breaking change, should we create a label for that and highlight accordingly?

---

_Label `breaking` added by @charliermarsh on 2024-03-11 23:56_

---

_Label `performance` added by @charliermarsh on 2024-03-11 23:56_

---

_Merged by @charliermarsh on 2024-03-12 04:05_

---

_Closed by @charliermarsh on 2024-03-12 04:05_

---

_Branch deleted on 2024-03-12 04:05_

---

_Comment by @samypr100 on 2024-03-13 01:41_

@charliermarsh Is it worth checking if `SSL_CERT_FILE` is set and make it behave as-if `--native-tls` was used?
I think that would keep this change backwards compatible with users leveraging `SSL_CERT_FILE`

e.g. something like

```diff
-let tls = tls::load(if self.native_tls {
+let tls = tls::load(if self.native_tls || env::var("SSL_CERT_FILE").is_ok() {
    Roots::Native
} else {
    Roots::Webpki
})
```

---

_Comment by @charliermarsh on 2024-03-13 01:43_

We can support `SSL_CERT_FILE` even without `--native-tls`, I think. I'm happy to add that. It would effectively be this PR: https://github.com/astral-sh/uv/pull/2351.

---

_Comment by @samypr100 on 2024-03-13 01:55_

Per this updated statement in the docs

> If a direct path to the certificate is required (e.g., in CI), set the `SSL_CERT_FILE` environment
variable to the path of the certificate bundle (alongside the `--native-tls` flag), to instruct uv
to use that file instead of the system's trust store.

It sounds like reqwest will honor still `SSL_CERT_FILE` (if it's set), even if the loading is happening via on uv's end via `tls::load/use_preconfigured_tls`.
Assuming this, the suggested code snippet above would work to keep this change backwards compatible.

---

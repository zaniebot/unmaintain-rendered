```yaml
number: 16773
title: Disable always-authenticate when running under Dependabot
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/authenticate
created_at: 2025-11-18T19:18:39Z
updated_at: 2025-11-19T02:53:01Z
url: https://github.com/astral-sh/uv/pull/16773
synced_at: 2026-01-12T16:12:26Z
```

# Disable always-authenticate when running under Dependabot

---

_@zanieb_

Dependabot appears to run a proxy which intercepts all requests and adds credentials — credentials are _not_ provided via the CLI or environment variables and there's no way for a user to do so. This means that when `authenticate = "always"` is used (or when the index URL is on a pyx domain), uv will fail even though Dependabot may intercept the request and add credentials.

See https://github.com/dependabot/dependabot-core/#private-registry-credential-management

---

_Renamed from "Disable always-authenticate when running under `DEPENDABOT`" to "Disable always-authenticate when running under Dependabot" by @zanieb on 2025-11-18 19:30_

---

_Marked ready for review by @zanieb on 2025-11-18 21:03_

---

_Review requested from @charliermarsh by @zanieb on 2025-11-18 21:03_

---

_@woodruffw approved on 2025-11-18 21:42_

Distressing, but makes sense.

---

_Merged by @zanieb on 2025-11-18 21:43_

---

_Closed by @zanieb on 2025-11-18 21:43_

---

_Branch deleted on 2025-11-18 21:43_

---

_@charliermarsh reviewed on 2025-11-18 21:52_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:364 on 2025-11-18 21:52_

Should we avoid doing re-reading this in this hot path?

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:364 on 2025-11-18 23:26_

Uh we could. I assumed it was very cheap. I guess we're validating that the value is utf-8 and allocating a string?

I'm a little wary of the indirection of moving it out, I'm not sure where I'd put it.

---

_@zanieb reviewed on 2025-11-18 23:26_

---

_@charliermarsh reviewed on 2025-11-18 23:33_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:364 on 2025-11-18 23:33_

Make it a once cell?

---

_@woodruffw reviewed on 2025-11-19 02:53_

---

_Review comment by @woodruffw on `crates/uv-auth/src/middleware.rs`:364 on 2025-11-19 02:53_

Yeah, environment accesses are somewhat expensive in Rust, since Rust acquires a lock on the entire environment (at least on macOS/Linux) for individual variable accesses. That wouldn't matter much in a lot of workloads, but I can imagine we'd have environment read contention in our setting because of work-splitting across multiple tokio-managed threads.

(It's probably still not a _ton_, but a OnceCell or LazyLock would avoid it as a concern.)

Ref: https://doc.rust-lang.org/src/std/sys/env/unix.rs.html

---

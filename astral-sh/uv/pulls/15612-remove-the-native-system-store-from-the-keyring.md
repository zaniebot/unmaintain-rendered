```yaml
number: 15612
title: Remove the native system store from the keyring providers
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/auth-not-provider
created_at: 2025-08-31T20:06:12Z
updated_at: 2025-09-02T13:37:14Z
url: https://github.com/astral-sh/uv/pull/15612
synced_at: 2026-01-12T16:11:51Z
```

# Remove the native system store from the keyring providers

---

_@zanieb_

We're not sure what the best way to expose the native store to users is yet and it's a bit weird that you can use this in the `uv auth` commands but can't use any of the other keyring provider options. The simplest path forward is to just not expose it to users as a keyring provider, and instead frame it as a preview alternative to the plaintext uv credentials store. We can revisit the best way to expose configuration before stabilization.

Note this pull request retains the _internal_ keyring provider implementation — we can refactor it out later but I wanted to avoid a bunch of churn here.

---

_Marked ready for review by @zanieb on 2025-08-31 20:17_

---

_@charliermarsh approved on 2025-09-01 02:51_

---

_@charliermarsh reviewed on 2025-09-01 02:53_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:35 on 2025-09-01 02:53_

I think this could have some surprising effects... For example, if you run with `--preview`, suddenly all the creds you stored with the default backend are now gone, and auth will fail. How can we avoid that? Make this a dedicated setting? Make it tiered?

---

_@zanieb reviewed on 2025-09-01 13:27_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:35 on 2025-09-01 13:27_

I think it'd need a dedicated setting, which I was wary to add here but we should definitely figure out. I think we'll actually still always _read_ from the plaintext store in the middleware? but, e.g., `logout` wouldn't work.

---

_@charliermarsh reviewed on 2025-09-01 14:03_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:35 on 2025-09-01 14:03_

(Wary because it expands the scope of the PR? Or for another reason?)

---

_@zanieb reviewed on 2025-09-01 14:31_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:35 on 2025-09-01 14:31_

Yeah not necessarily because of the scope of the pull request, but wary because we then need to choose a name for a flag and be fairly certain that's what we want. Part of the intent of this pull request was to defer that decision.

---

_Merged by @zanieb on 2025-09-02 13:37_

---

_Closed by @zanieb on 2025-09-02 13:37_

---

_Branch deleted on 2025-09-02 13:37_

---

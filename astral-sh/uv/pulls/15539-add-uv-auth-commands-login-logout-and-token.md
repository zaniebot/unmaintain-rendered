```yaml
number: 15539
title: "Add `uv auth` commands (`login`, `logout`, and `token`)"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/uv-auth-ii
created_at: 2025-08-26T17:20:10Z
updated_at: 2025-08-28T14:56:51Z
url: https://github.com/astral-sh/uv/pull/15539
synced_at: 2026-01-10T06:44:33Z
```

# Add `uv auth` commands (`login`, `logout`, and `token`)

---

_Pull request opened by @zanieb on 2025-08-26 17:20_

Picks up the work from

- #14559 
- https://github.com/astral-sh/uv/pull/14896

There are some high-level changes from those pull requests

1. We do not stash seen credentials in the keyring automatically, that can be future work
2. We use `auth login` and `auth logout` (for future consistency)
3. We add a `token` command for showing the credential that will be used

As well as many smaller changes to API, messaging, testing, etc.

---

_Renamed from "Add `uv auth` commands (`login`, `logout`, and `show`)" to "Add `uv auth` commands (`login`, `logout`, and `token`)" by @zanieb on 2025-08-27 17:20_

---

_@charliermarsh reviewed on 2025-08-28 01:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/auth/login.rs`:54 on 2025-08-28 01:17_

Do we need to enable users to specify a keyring provider type on a per-URL basis? Like, would it ever be the case that a user wants to use (e.g.) the AWS keyring plugin via subprocess, but then store credentials for some URLs via the native plugin? (This is, in a vague way, coming from me being confused that `--keyring-provider` is respected here at all, since it _only_ ever works for `native`.)

---

_@charliermarsh reviewed on 2025-08-28 01:18_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/auth/login.rs`:59 on 2025-08-28 01:18_

I am also wondering if we should _not_ accept credentials over plaintext on the CLI, hmm.

---

_@charliermarsh reviewed on 2025-08-28 01:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/auth/login.rs`:74 on 2025-08-28 01:19_

Again a vague thing where I'm wondering if we should be using `Credentials::Bearer` for tokens, rather than basic.

---

_@charliermarsh reviewed on 2025-08-28 01:22_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/auth/logout.rs`:21 on 2025-08-28 01:22_

If you wanted to get fancy, you could make this `Cow`... Like `username.map(Cow::Owned).unwrap_or(Cow::Borrowed("__token__"))`.

(You def don't need to do this, just thought you mind find it interesting.)

---

_@charliermarsh reviewed on 2025-08-28 01:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/auth/token.rs`:20 on 2025-08-28 01:24_

Should we move this into the `else` on line 41?

---

_@zanieb reviewed on 2025-08-28 13:12_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/login.rs`:54 on 2025-08-28 13:12_

> (This is, in a vague way, coming from me being confused that --keyring-provider is respected here at all, since it only ever works for native.)

I think we actually can support the subprocess keyring here — I just didn't want to add support for that yet.

> Do we need to enable users to specify a keyring provider type on a per-URL basis? Like, would it ever be the case that a user wants to use (e.g.) the AWS keyring plugin via subprocess, but then store credentials for some URLs via the native plugin? 

This is probably a realistic use-case, yeah. I think we'd need to add a concept like `keyring-provider-service` which maps a service URL to a keyring provider.

---

_@zanieb reviewed on 2025-08-28 13:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/login.rs`:59 on 2025-08-28 13:13_

As in, not accept it via a CLI flag and require stdin?

---

_@zanieb reviewed on 2025-08-28 13:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/login.rs`:74 on 2025-08-28 13:14_

Ah that's complicated. Our current semantics for tokens is that it's a shortcut for `__token__:<token>` because that's a common pattern for allowing tokens over HTTP basic authentication. This matches, e.g., the `publish` API we have. I think we want to allow Bearer authentication too, but I think it'd be another dimension of options?

---

_@zanieb reviewed on 2025-08-28 13:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/logout.rs`:21 on 2025-08-28 13:15_

👍 I thought about using Cow but figured it wasn't worth the time right now.

---

_@zanieb reviewed on 2025-08-28 13:20_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/token.rs`:20 on 2025-08-28 13:20_

Fine with me — I must have refactored something so it was only needed once.

---

_@zanieb reviewed on 2025-08-28 13:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/login.rs`:59 on 2025-08-28 13:25_

I don't think it's quite right to outright ban this, e.g., Git allows credentials interactively and we already allow passwords in plaintext in URLs.

---

_Marked ready for review by @zanieb on 2025-08-28 13:31_

---

_Merged by @zanieb on 2025-08-28 13:34_

---

_Closed by @zanieb on 2025-08-28 13:34_

---

_Branch deleted on 2025-08-28 13:34_

---

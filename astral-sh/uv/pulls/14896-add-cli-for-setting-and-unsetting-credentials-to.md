```yaml
number: 14896
title: Add cli for setting and unsetting credentials to system keyring
type: pull_request
state: closed
author: jtfmumm
labels:
  - enhancement
  - do-not-merge
assignees: []
draft: true
base: jtfm/keyring-exploration
head: jtfm/credentials-cli
created_at: 2025-07-25T14:10:05Z
updated_at: 2025-09-02T11:42:07Z
url: https://github.com/astral-sh/uv/pull/14896
synced_at: 2026-01-10T06:44:33Z
```

# Add cli for setting and unsetting credentials to system keyring

---

_Pull request opened by @jtfmumm on 2025-07-25 14:10_

This is a work in progress that depends on #14559. There are still open design questions.

There are a couple of FIXMEs in this draft PR. Some are for more doc details, but a few are about adding support for accepting passwords over stdin and providing a security warning if the user passes a plaintext password to a `--password` option.

Also left to do:
* Automatically remove a trailing `/simple` from a service before persisting credentials.
* Possibly put automatic storage of credentials behind a `--preview` flag, unless we think it's enough that you must explicitly configure a `native` keyring provider (we've discussed making it the default, but that's not the behavior here)
* Determine how best to automate testing
* Add documentation


---

_Label `enhancement` added by @jtfmumm on 2025-07-25 14:10_

---

_Label `do-not-merge` added by @jtfmumm on 2025-07-25 14:10_

---

_@jtfmumm reviewed on 2025-07-25 14:10_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:116 on 2025-07-25 14:10_

We could warn here if this is not set.

---

_@jtfmumm reviewed on 2025-07-25 14:12_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:5334 on 2025-07-25 14:12_

This needs to be fleshed out more once we've decided how we're presenting the service

---

_@jtfmumm reviewed on 2025-07-25 14:12_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:5353 on 2025-07-25 14:12_

This needs to be fleshed out more once we've decided how we're presenting the service

---

_@jtfmumm reviewed on 2025-07-25 14:13_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/auth/mod.rs`:1 on 2025-07-25 14:13_

At first I thought this would need to be split out, but I'll probably update this to use just `commands/auth.rs`

---

_Comment by @jtfmumm on 2025-09-02 11:42_

Closed in favor of #15539 

---

_Closed by @jtfmumm on 2025-09-02 11:42_

---

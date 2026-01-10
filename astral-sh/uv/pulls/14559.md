```yaml
number: 14559
title: "Enable system keyring integration via `--keyring-provider native`"
type: pull_request
state: closed
author: jtfmumm
labels: []
assignees: []
base: main
head: jtfm/keyring-exploration
created_at: 2025-07-11T08:26:24Z
updated_at: 2025-09-02T11:41:41Z
url: https://github.com/astral-sh/uv/pull/14559
synced_at: 2026-01-10T06:44:33Z
```

# Enable system keyring integration via `--keyring-provider native`

---

_Pull request opened by @jtfmumm on 2025-07-11 08:26_

This PR uses the `uv-keyring` crate vendored in #14725 to automatically integrate with the system keyring when the keyring provider is set to "native". 

When this backend is enabled, uv auth middleware will attempt to retrieve missing credentials from the system keyring. It will also store index credentials in the system keyring upon successful authentication, enabling users to provide their credentials for an index once and successfully authenticate on future invocations (though you must currently still provide a username on future invocations, see #10866).

Credentials are stored in the system keyring for a "service"/username pair. For the service, this currently prefixes the index URL with `uv-credentials:`. This prefix could help prevent collisions but would also be useful for determining which keyring credentials have been set by uv. Note that this is using the index URL rather than the index name for the service.

Left to do:
* Possibly put automatic storage of credentials behind a `--preview` flag, unless we think it's enough that you must explicitly configure a `native` keyring provider (we've discussed making it the default, but that's not the behavior here)

Depends on #14725.


---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:56 on 2025-07-11 08:27_

We could extend this to the Python `keyring` as well, but for this PR I've limited the scope to the new `native` backend.

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:401 on 2025-07-11 08:39_

This is currently only storing credentials on successful authentication if this is an index URL.

---

_@jtfmumm reviewed on 2025-07-11 08:39_

---

_Marked ready for review by @jtfmumm on 2025-08-07 17:13_

---

_Comment by @jtfmumm on 2025-09-02 11:41_

Closed in favor of #15539 

---

_Closed by @jtfmumm on 2025-09-02 11:41_

---

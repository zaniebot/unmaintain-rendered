```yaml
number: 15456
title: Add keyring integration for git credentials
type: pull_request
state: open
author: jtfmumm
labels: []
assignees: []
draft: true
base: jtfm/keyring-exploration
head: jtfm/git-keyring-integration
created_at: 2025-08-22T15:33:20Z
updated_at: 2025-08-22T18:39:21Z
url: https://github.com/astral-sh/uv/pull/15456
synced_at: 2026-01-12T16:11:45Z
```

# Add keyring integration for git credentials

---

_@jtfmumm_

This PR adds native keyring integration for persisting and fetching git credentials. On successful authentication for a git repo, credentials are persisted to the system keyring if `keyring-provider = "native"`. Future invocations will be able to find these credentials even in the absence of a git credential helper (though you must currently still provide a username on future invocations, see https://github.com/astral-sh/uv/issues/10866). 

Credentials are stored in the system keyring with a `uv-credentials:` prefix (see #14559), e.g., the keyring service for a GitHub repo would be named `uv-credentials:https://github.com/<org>/<repo>`. 

TODO:
* Add a test for `uv sync` (there is currently only a new test for `uv add`). I've successfully tested `uv sync` locally but ideally we should have an automated test.

Depends on #14559.


---

_Review comment by @jtfmumm on `crates/uv-git/src/credentials.rs`:24 on 2025-08-22 15:38_

This method was switched to `async` for calling `KeyringProvider::store_if_native`, as was `store_credentials_from_url` below. 

---

_@jtfmumm reviewed on 2025-08-22 15:38_

---

_@jtfmumm reviewed on 2025-08-22 15:41_

---

_Review comment by @jtfmumm on `crates/uv-git/src/source.rs`:75 on 2025-08-22 15:41_

This method was switched to `async` in order to call `KeyringProvider::fetch_if_native`. As a result, a immediately invoked closure below was converted to a `spawn_blocking` call. It's worth noting that this change holds whether or not the keyring provider is configured to be native.

---

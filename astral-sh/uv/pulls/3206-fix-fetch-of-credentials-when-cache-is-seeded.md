```yaml
number: 3206
title: Fix fetch of credentials when cache is seeded with username
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-fetch
created_at: 2024-04-23T13:48:50Z
updated_at: 2024-04-23T14:02:31Z
url: https://github.com/astral-sh/uv/pull/3206
synced_at: 2026-01-12T16:05:29Z
```

# Fix fetch of credentials when cache is seeded with username

---

_@zanieb_

Fixes the failure to lookup credentials in https://github.com/astral-sh/uv/issues/3205

The issue is that we seed the cache with the index URL which includes a username but no password. We did not ensure that a password was present in the cached credentials before attempting a request with them. Now, the cache will not return credentials when a username is provided and the cached credentials have no password — the cached credentials are useless in that case.

Tested with a Google Artifact Registry and keyring

```
RUST_LOG=uv=trace cargo run -q -- pip install requests --index-url https://oauth2accesstoken@us-central1-python.pkg.dev/<project>/pypi/simple/ --no-cache --keyring-provider subprocess -v
```

---

_Label `bug` added by @zanieb on 2024-04-23 13:48_

---

_Review requested from @BurntSushi by @zanieb on 2024-04-23 13:56_

---

_Comment by @jenshnielsen on 2024-04-23 13:57_

@zanieb I can confirm that this restores the behaviour of 0.1.35 for Azure artifacts. 

---

_@BurntSushi approved on 2024-04-23 13:58_

I buy what you're selling.

---

_Merged by @zanieb on 2024-04-23 14:02_

---

_Closed by @zanieb on 2024-04-23 14:02_

---

_Branch deleted on 2024-04-23 14:02_

---

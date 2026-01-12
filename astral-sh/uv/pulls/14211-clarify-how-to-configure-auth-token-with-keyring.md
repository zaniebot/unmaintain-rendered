```yaml
number: 14211
title: Clarify how to configure auth token with keyring
type: pull_request
state: open
author: rafalkrupinski
labels:
  - documentation
assignees: []
base: main
head: docs/keyring-service
created_at: 2025-06-23T09:06:43Z
updated_at: 2026-01-06T08:25:18Z
url: https://github.com/astral-sh/uv/pull/14211
synced_at: 2026-01-12T16:11:05Z
```

# Clarify how to configure auth token with keyring

---

_@rafalkrupinski_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The documentation on authentication with `keyring` refers to the tool documentation, without explaining how to configure it. `keyring` expects `service` when adding a password - should I use index name, domain or URL?

## Test Plan

N/A - one line of text added to the documentation.


---

_Review requested from @zanieb by @konstin on 2025-06-23 10:45_

---

_Label `documentation` added by @konstin on 2025-06-23 10:45_

---

_@zanieb reviewed on 2025-06-27 23:24_

---

_Review comment by @zanieb on `docs/concepts/authentication.md`:86 on 2025-06-27 23:24_

I don't think we actually want to recommend using `keyring` to store passwords, we're mostly suggesting keyring based authentication to retrieve passwords from third-party plugins, etc.

If we add something like this, I think we'd need to provide more details, but I'm wary of `keyring` in general and we intend to build something better around this, so I think my preference is to just leave this as-is?

---

_@rafalkrupinski reviewed on 2025-06-28 09:26_

---

_Review comment by @rafalkrupinski on `docs/concepts/authentication.md`:86 on 2025-06-28 09:26_

Correct me if I'm wrong, but using keyring with the system wallet is the only way to securely store tokens locally. So it would be great if it was fully supported and documented.

---

_@mi-volodin reviewed on 2026-01-06 08:25_

---

_Review comment by @mi-volodin on `docs/concepts/authentication.md`:86 on 2026-01-06 08:25_

Excuse me for interjecting, was just walking by...

> ...keyring with the system wallet is the only way...

Just to note, that actually not anymore. The `uv auth` now can cover the storage in the system keychain. Right now it also requires `UV_PREVIEW_FEATURES=native-auth`, but hopefully it will be enabled by default at some point.

In my company we still recommend `keyring` as an org-wide happy path for `uv` configuration. The `keyring` configuration (providers) is flaky in Linux, but we keep it, because for `uv auth` without `native-auth` flag (which user can "forget" to set) we land into unencrypted local storage.

---

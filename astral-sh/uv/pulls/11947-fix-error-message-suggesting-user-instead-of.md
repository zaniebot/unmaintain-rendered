```yaml
number: 11947
title: "Fix error message suggesting `--user` instead of `--username`"
type: pull_request
state: merged
author: LewisGaul
labels:
  - error messages
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-03-04T12:07:58Z
updated_at: 2025-03-04T13:58:09Z
url: https://github.com/astral-sh/uv/pull/11947
synced_at: 2026-01-10T11:10:39Z
```

# Fix error message suggesting `--user` instead of `--username`

---

_Pull request opened by @LewisGaul on 2025-03-04 12:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fix error message suggesting `--user` instead of `--username`:
```
   > uv publish --publish-url ... ... --password $(cat ~/.token)
Publishing 1 file to ...
error: Attempted to publish with a password, but no username. Either provide a username with `--user` (`UV_PUBLISH_USERNAME`), or use `--token` (`UV_PUBLISH_TOKEN`) instead of a password.

   > uv publish --publish-url ... ... --user lewis --password $(cat ~/.token)
error: unexpected argument '--user' found

  tip: a similar argument exists: '--username'

Usage: uv publish <FILES|--index <INDEX>|--username <USERNAME>|--password <PASSWORD>|--token <TOKEN>|--trusted-publishing <TRUSTED_PUBLISHING>|--keyring-provider <KEYRING_PROVIDER>|--publish-url <PUBLISH_URL>|--check-url <CHECK_URL>|--skip-existing>

For more information, try '--help'.
```

## Test Plan
I have not tested manually, I'm hoping this isn't necessary and there will be sufficient CI coverage.

---

_@charliermarsh approved on 2025-03-04 13:57_

Thanks!

---

_Label `error messages` added by @charliermarsh on 2025-03-04 13:57_

---

_Merged by @charliermarsh on 2025-03-04 13:58_

---

_Closed by @charliermarsh on 2025-03-04 13:58_

---

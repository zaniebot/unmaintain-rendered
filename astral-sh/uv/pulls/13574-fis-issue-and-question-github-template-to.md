```yaml
number: 13574
title: Fis ISSUE and QUESTION github template to correctly suggest to user on how to get the uv version
type: pull_request
state: closed
author: paulefoe
labels: []
assignees: []
base: main
head: main
created_at: 2025-05-21T12:16:56Z
updated_at: 2025-05-21T12:43:08Z
url: https://github.com/astral-sh/uv/pull/13574
synced_at: 2026-01-10T11:10:41Z
```

# Fis ISSUE and QUESTION github template to correctly suggest to user on how to get the uv version

---

_Pull request opened by @paulefoe on 2025-05-21 12:16_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

There's no command `uv self version` 
There's a `uv self --version`, but I thought the `uv --version` might be more useful. Let me know if I got it wrong

```bash
uv self version  
error: unrecognized subcommand 'version'

Usage: uv self [OPTIONS] <COMMAND>

For more information, try '--help'.
```

## Test Plan

N/A


---

_Renamed from "Fis ISUSE and QUESTION github template to correctly suggest to user on how to get the uv version" to "Fis ISSUE and QUESTION github template to correctly suggest to user on how to get the uv version" by @paulefoe on 2025-05-21 12:28_

---

_Comment by @konstin on 2025-05-21 12:33_

The command exists in the 0.7.x releases, are you running an old version?

---

_Comment by @paulefoe on 2025-05-21 12:43_

uv 0.6.5


---

_Closed by @paulefoe on 2025-05-21 12:43_

---

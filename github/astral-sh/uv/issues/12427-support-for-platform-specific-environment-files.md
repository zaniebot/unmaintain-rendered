---
number: 12427
title: "Support for Platform-Specific Environment Files in ``.python-version``"
type: issue
state: open
author: meitham
labels:
  - question
assignees: []
created_at: 2025-03-24T11:47:01Z
updated_at: 2025-04-01T18:20:45Z
url: https://github.com/astral-sh/uv/issues/12427
synced_at: 2026-01-07T13:12:18-06:00
---

# Support for Platform-Specific Environment Files in ``.python-version``

---

_Issue opened by @meitham on 2025-03-24 11:47_

### Summary

# Feature Request: Support for Platform-Specific Environment Files (e.g. `uv-version.toml`)

I’m a heavy user of [pyenv](https://github.com/pyenv/pyenv) which works well on Linux but not as well on Windows. As I migrate from pyenv to uv (starting with Windows), I’ve encountered a conflict due to uv’s reuse of the `.python-version` file.

## The Issue

- **pyenv Usage:** pyenv standardised the `.python-version` file to store the name of the virtual environment.
- **uv Expectation:** uv expects the same file to contain a semver for Python.
- **Resulting Conflict:** This difference means that both tools cannot coexist in the same working directory without interference.

## Proposed Solution

It would be beneficial to allow uv to use a separate configuration file—say, `uv-version.toml`—that supports different environment definitions per platform. For example, the file might look like this:

```toml
[versions]
windows = ".win-env"
linux = ".linux-env"
```

This approach would allow a single git checkout (shared between Windows and WSL) to support distinct environments for each platform, resolving the current conflict between uv and pyenv.

## Questions

- Is this feature feasible within uv’s design?
- Alternatively, is there an existing uv feature or workaround that addresses cross-platform environment configuration?  Perhaps ``uv.toml`` can be extended to cover this use case, removing the need for ``.python-version`` and ``.env`` files?

Thank you,
Meitham

---

_Label `enhancement` added by @meitham on 2025-03-24 11:47_

---

_Comment by @zanieb on 2025-04-01 18:20_

> pyenv standardised the .python-version file to store the name of the virtual environment.

To be clear, this is `pyenv-virtualenv` which is an _extension_ of `pyenv`. This is not a part of the standard use of the file, and I think is arguably a misuse. See also https://github.com/astral-sh/uv/pull/7935

> It would be beneficial to allow uv to use a separate configuration file—say, uv-version.toml—that supports different environment definitions per platform.

I'm not sure I follow. You want a different environment path per platform? How does this interact with the `.python-version` file? Are you talking about `.env` files?

> Perhaps uv.toml can be extended to cover this use case, removing the need for .python-version and .env files?

See https://github.com/astral-sh/uv/issues/4970

---

_Label `question` added by @zanieb on 2025-04-01 18:20_

---

_Label `enhancement` removed by @zanieb on 2025-04-01 18:20_

---

```yaml
number: 17179
title: Shell-escape Python request in manual install hint
type: pull_request
state: open
author: cls-oi
labels:
  - enhancement
assignees: []
base: main
head: cls/shell-escape-request
created_at: 2025-12-18T17:11:31Z
updated_at: 2025-12-19T09:54:24Z
url: https://github.com/astral-sh/uv/pull/17179
synced_at: 2026-01-10T05:49:14Z
```

# Shell-escape Python request in manual install hint

---

_Pull request opened by @cls-oi on 2025-12-18 17:11_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

When printing a hint that Python downloads are set to 'manual' and suggesting that the user "use `uv python install >=3.11, <3.12`", the request is now quoted as appropriate for a shell command: "`uv python install '>=3.11, <3.12'`".

This change uses the `shell-escape` crate, which is already a transitive dependency of uv.

Closes #17051.

## Test Plan

<!-- How was it tested? -->

I've added a unit test `python_install_manual` covering the default case and the case of `>=3.11, <3.12`.

---

_Label `enhancement` added by @konstin on 2025-12-18 17:34_

---

_Comment by @zanieb on 2025-12-18 17:44_

I know I suggested this in https://github.com/astral-sh/uv/issues/17051#issuecomment-3633659514 but I'm wondering if it makes more sense to just suggest the concrete version that'd be installed?

Like I think for this example we should just hint

> A managed Python download is available for Python >=3.11, <3.12, but Python downloads are set to 'manual', use `uv python install 3.11` to install the required version

Since we know which version we'd select?

I think we recently added some logic which lets us "simplify" a Python install key to drop default values.

---

_Comment by @cls-oi on 2025-12-18 18:31_

That makes sense to me - though I could only find existing 'simplification' functions for `PythonDownloadRequest`, not for `PythonInstallationKey`?

---

_Comment by @zanieb on 2025-12-18 22:43_

You'd use https://github.com/astral-sh/uv/blob/9949f0801f57a386279b067bae94606e20f60f6f/crates/uv-python/src/downloads.rs#L623-L645 first

---

_Comment by @cls-oi on 2025-12-19 09:47_

Ah, thanks - I've done a variant of that where I add `impl From<&ManagedPythonDownload> for PythonDownloadRequest`, similar to that already there for `&ManagedPythonInstallation`, to avoid having to deal with the fail case (assuming the same assumption holds). It felt cleaner this way, though I can go through the existing `TryFrom` if preferred.

One concern I have with this is I'm not sure whether it's valid to do `without_patch()` as the version range could restrict the patch version, but unless we special-case that then we're being very specific about the install version and I'm not sure how much better it is than just displaying the range. For now I have used it, anyway, to avoid including an arbitrary patch number in the test snapshot, and to match your suggested output.

---

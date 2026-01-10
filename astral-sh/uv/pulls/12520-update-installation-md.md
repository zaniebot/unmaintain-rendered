```yaml
number: 12520
title: Update installation.md
type: pull_request
state: open
author: copeland3300
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-03-28T00:37:44Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/12520
synced_at: 2026-01-10T11:10:40Z
```

# Update installation.md

---

_Pull request opened by @copeland3300 on 2025-03-28 00:37_

Fixed command blocks to copy to clipboard correctly

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-03-28 02:36_

Why does this fix things?

---

_Comment by @copeland3300 on 2025-03-28 05:59_

As it stands right now, if you click "copy to clipboard" for 1. Clean up stored data, you will paste the following:

uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"

Great, this is easy to drop into a terminal.

The section 2. Remove the uv and uvx binaries will give the following for Windows:
$ rm $HOME\.local\bin\uv.exe
$ rm $HOME\.local\bin\uvx.exe

The $ at the beginning of lines breaks copy and paste.

I believe this change will fix.

EDIT: testing is showing this change isn't sufficient to fix the problem, so I'm playing with additional changes.

---

_Comment by @copeland3300 on 2025-03-28 06:56_

It seems there is no good solution that I can easily find to keep the $ on the webpage but have to copy/paste function correctly.

I think we could remove the $, as other examples of powershell do not have lines starting with $. Updating the PR to reflect this suggestion.

---

_Comment by @zanieb on 2025-04-01 19:39_

The leading `$` works elsewhere, we have some JS to strip it on copy. The solution is probably to find out why that's not working here instead of just removing it from this one spot.

---

_Comment by @copeland3300 on 2025-04-02 04:14_

I've tried to copy in Firefox, Chrome (fresh install) and Edge, and in all cases the Windows lines were copied with the `$`

![image](https://github.com/user-attachments/assets/b82dcf9b-6dcc-42b1-b149-bae585a8dc63)

Are we thinking there's something going on with the JS?

---

_Comment by @zanieb on 2025-04-02 13:49_

That's my thinking, yeah

---

```yaml
number: 17461
title: Add actionable hint to unmanaged version error message
type: pull_request
state: open
author: originell
labels: []
assignees: []
base: main
head: fix/python-download-hint
created_at: 2026-01-14T11:59:17Z
updated_at: 2026-01-14T12:02:14Z
url: https://github.com/astral-sh/uv/pull/17461
synced_at: 2026-01-14T12:44:12Z
```

# Add actionable hint to unmanaged version error message

---

_@originell_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

When a project requests a newer Python version than the current UV version knows about, the existing error message isn't all that useful. Suggesting updating `uv` seems like a good hint for humans (and future LLM overlords) alike.

I just ran into this with some colleagues of mine. I upgraded a project to 3.14. They pulled and did a `uv sync`, but it failed. It turned out that their uv installation was at 0.5. Running a `uv self update` to update to 0.9.x made it work again.

Before:

> error: No interpreter found for Python ==3.14.* in managed installations or search path

After:

> error: No interpreter found for Python ==3.14.* in managed installations or search path
>
> hint: No Python download is available for Python 3.14. This version might be newer than your uv. Update uv and retry.

The current idea for the hint does not explicitly mention running `uv self update`. That's because I felt like this wouldn't be a good idea due to the various ways uv could be installed. If you say `uv self update` is always safe, I'd be happy to adjust the hint and explicitly mention that command.

## Test Plan

<!-- How was it tested? -->
`cargo nextest` as laid out in the contributing docs.


---

_Review requested from @zanieb by @konstin on 2026-01-14 12:00_

---

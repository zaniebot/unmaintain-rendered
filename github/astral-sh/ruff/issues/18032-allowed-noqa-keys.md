---
number: 18032
title: Allowed noqa keys
type: issue
state: closed
author: CharlesPerrotMinot
labels: []
assignees: []
created_at: 2025-05-12T06:14:39Z
updated_at: 2025-05-12T07:18:04Z
url: https://github.com/astral-sh/ruff/issues/18032
synced_at: 2026-01-07T13:12:16-06:00
---

# Allowed noqa keys

---

_Issue opened by @CharlesPerrotMinot on 2025-05-12 06:14_

### Summary

I searched, but didn't find any specific issue / discussion related to this. In short, no matter what the configuration, en engineer can always noqa the lines. It "should" get caught in the Code Review, but what if... ruff had a rule to only allow a certain set of rules to be noqa, and would fail if any rule outside of this set was to be added?

---

_Comment by @MichaReiser on 2025-05-12 06:52_

I think this is the same as https://github.com/astral-sh/ruff/issues/13341 in that it asks for a way to disallow the use of `noqa` for some rules.

---

_Closed by @MichaReiser on 2025-05-12 06:52_

---

_Comment by @CharlesPerrotMinot on 2025-05-12 06:57_

This isn't quite the same though, since the issue you mention is about preventing the suppression of a specific rule, while my request was to only allow the suppression of some rules from the start.

They do address the handling of noqa comments, but I do believe that the approach is fundamentally different - one would need to suppress every rule except the whitelisted ones with the issue you mentioned.

As a result I think both could be covered, and would be different features. So can you please reopen @MichaReiser ?

---

_Comment by @MichaReiser on 2025-05-12 07:03_

I see, there's a difference in approach where one asks for allowlisting the rules and the other asks for a denylist. 

I don't think this warrants two different issues. The main feature we need is a more fine granular control for allowing/disallowing noqa usage on a per rule basis

---

_Comment by @CharlesPerrotMinot on 2025-05-12 07:17_

But wouldn't it be a nightmare to maintain disallowing noqa usage for all but a specific list of rules? Especially since one would need to update it based on new rules at every ruff update. While having a base set of allowed noqa rules would be self-maintained, since new rules would be automatically disallowed for noqa usage

---

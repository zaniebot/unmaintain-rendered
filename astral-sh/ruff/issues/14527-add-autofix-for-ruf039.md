```yaml
number: 14527
title: Add autofix for RUF039
type: issue
state: closed
author: AlexWaygood
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2024-11-22T13:00:31Z
updated_at: 2024-11-22T21:52:59Z
url: https://github.com/astral-sh/ruff/issues/14527
synced_at: 2026-01-10T11:09:56Z
```

# Add autofix for RUF039

---

_Issue opened by @AlexWaygood on 2024-11-22 13:00_

The newly added preview rule RUF039 doesn't have an autofix. It would be very convenient if it had one, since it's otherwise quite a noisy rule.

---

_Label `fixes` added by @AlexWaygood on 2024-11-22 13:00_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-22 13:00_

---

_Comment by @MichaReiser on 2024-11-22 14:25_

At least for the common cases. Conversions to raw strings can be tricky if backslashes are involved

---

_Comment by @MichaReiser on 2024-11-22 14:28_

Or a fix involving backslashes at least needs to be unsafe because

```
>>> re.compile("\n\r\t").pattern == re.compile(r"\n\r\t").pattern
False
>>> re.compile("\n\r\t").pattern
'\n\r\t'
>>> re.compile(r"\n\r\t").pattern
'\\n\\r\\t'
```

---

_Comment by @AlexWaygood on 2024-11-22 14:30_

Yeah. But even an autofix just for strings that don't have any backslashes in them would make things much more ergonomic. E.g. none of the strings that I had to manually edit in https://github.com/python-trio/trio/pull/3143 had any backslashes in them.

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-22 16:15_

---

_Closed by @dylwil3 on 2024-11-22 21:09_

---

_Closed by @dylwil3 on 2024-11-22 21:09_

---

_Comment by @dylwil3 on 2024-11-22 21:12_

@AlexWaygood feel free to reopen if you'd like to encourage a second pass on this with more extensive auto-fixing!

---

_Comment by @AlexWaygood on 2024-11-22 21:52_

Thanks! 😃 I think this is going to take care of most of the noise as-is, so I'm happy to leave it there for now unless we get requests from users for fixes in more cases 👍

---

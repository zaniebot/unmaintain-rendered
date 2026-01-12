```yaml
number: 13297
title: "Feature: Check for unintended non-printing characters"
type: issue
state: open
author: jaraco
labels:
  - rule
assignees: []
created_at: 2024-09-09T21:52:42Z
updated_at: 2024-09-15T11:24:46Z
url: https://github.com/astral-sh/ruff/issues/13297
synced_at: 2026-01-12T15:54:52Z
```

# Feature: Check for unintended non-printing characters

---

_@jaraco_

In https://github.com/jaraco/zipp/pull/125, I learned that users can, intentionally or not, sneak non-printable characters into the code through otherwise innocuous pull requests. This particular one was caught by `mach lint` in a downstream system. It would be nice if ruff could catch such changes. I'm not confident it's possible, but I wanted to flag it as a feature for consideration.

---

_Renamed from "Fature: Check for unintended non-printing characters" to "Feature: Check for unintended non-printing characters" by @AlexWaygood on 2024-09-10 02:16_

---

_Label `rule` added by @MichaReiser on 2024-09-10 16:23_

---

_Comment by @MichaReiser on 2024-09-10 16:23_

I'm rather surprised that we don't already have that. There's an existing rule to catch [ambiguous unicode characters](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-docstring/), but it doesn't cover invisible characters. 

I'm very much in favor of adding the rule. We would have to think about if it should be a new rule or be added to the above mentioned rule

---

_Comment by @xyb on 2024-09-11 09:27_

I suggest add `--show-all-characters` option to show non-printable characters like [bat](https://github.com/sharkdp/bat) also:

![Screenshot 2024-09-11 at 17 26 12](https://github.com/user-attachments/assets/0442d1ee-a24f-4204-a5f6-874e90d877dd)


---

_Comment by @MichaReiser on 2024-09-11 19:16_

@xyb, this issue is different. It isn't about showing invisible characters; it's about linting for invisible characters.

---

_Comment by @xyb on 2024-09-15 11:24_

@MichaReiser I agree this is a separate request, but it's related because it would be confusing if Ruff reports an invisible character without helping the user fix it.  If you agree, I'll go ahead and create a new issue to track this requirement.

---

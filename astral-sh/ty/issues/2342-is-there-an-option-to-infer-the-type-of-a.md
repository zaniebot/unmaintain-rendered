```yaml
number: 2342
title: "Is there an option to infer the type of a variable without including a union with type `Unknown`?"
type: issue
state: closed
author: LiamWhitenack
labels:
  - question
assignees: []
created_at: 2026-01-05T14:12:03Z
updated_at: 2026-01-05T14:15:00Z
url: https://github.com/astral-sh/ty/issues/2342
synced_at: 2026-01-12T15:54:26Z
```

# Is there an option to infer the type of a variable without including a union with type `Unknown`?

---

_@LiamWhitenack_

### Question

<img width="396" height="118" alt="Image" src="https://github.com/user-attachments/assets/58e78b71-2880-4748-b7bc-4533d8bacfde" />

In this example, I would like the `ty` interpreter to infer the type of `test1` and `test2` to not include `Unknown`s anywhere. I know that I could subvert this problem by specifying `self.msg`'s type on line 3 and telling the linter that test2 is type `list[A]`, but I've found some situations where this is unsightly and Pylance would use the behavior I'm talking about anyhow.

Thank you for the hard work on this project! I'm very excited to see where this goes!

### Version

astlral-sh ty 2026.0.0 (VSCode extension)

---

_Label `question` added by @LiamWhitenack on 2026-01-05 14:12_

---

_Closed by @MichaReiser on 2026-01-05 14:15_

---

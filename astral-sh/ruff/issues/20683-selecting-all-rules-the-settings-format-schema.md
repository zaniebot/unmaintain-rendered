---
number: 20683
title: "Selecting `ALL` rules - the settings format/schema type"
type: issue
state: closed
author: paduszyk
labels:
  - question
assignees: []
created_at: 2025-10-02T16:53:19Z
updated_at: 2025-10-03T06:20:55Z
url: https://github.com/astral-sh/ruff/issues/20683
synced_at: 2026-01-10T01:23:01Z
---

# Selecting `ALL` rules - the settings format/schema type

---

_Issue opened by @paduszyk on 2025-10-02 16:53_

### Question

I have a short question regarding the configuration style/semantics. I always like to start with:

```toml
[lint]
select = ["ALL"]
```

to follow with ignoring certain rules with `ignore` setting. But, personally I would find:

```toml
[lint]
select = "all"
```

more precise. Given the current style, i.e. the list or rule names, suggest that one can add something to it. But adding something to the list activating `ALL` the rules has basically no effect, right?

Again: thanks for creating, developing, and maintaining this amazing project!

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Label `question` added by @paduszyk on 2025-10-02 16:53_

---

_Comment by @MichaReiser on 2025-10-03 06:20_

Thanks for the suggestions.

While it is certainly cleaner in this very specific instance, I think it's more confusing because it is less clear on how to refine the list starting with `select = "all"` (which might be something you copied from a tutorial). 

---

_Closed by @MichaReiser on 2025-10-03 06:20_

---

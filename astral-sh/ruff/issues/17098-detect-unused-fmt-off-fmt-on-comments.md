```yaml
number: 17098
title: "Detect unused `# fmt: off` & `# fmt: on` comments"
type: issue
state: closed
author: jack-mcivor
labels:
  - rule
  - wish
assignees: []
created_at: 2025-03-31T17:02:39Z
updated_at: 2025-04-01T15:36:54Z
url: https://github.com/astral-sh/ruff/issues/17098
synced_at: 2026-01-12T15:54:55Z
```

# Detect unused `# fmt: off` & `# fmt: on` comments

---

_@jack-mcivor_

### Summary

From the format documentation https://docs.astral.sh/ruff/formatter/#format-suppression

> As such, adding # fmt: on and # fmt: off comments within expressions will have no effect. In the following example, both list entries will be formatted, despite the # fmt: off:
> ```python
> [
>     # fmt: off
>     '1',
>     # fmt: on
>     '2',
> ]
> ```

Should there be a new rule to detect such cases? I think it's in the spirit of an unused noqa comment, for which there is RUF100 https://docs.astral.sh/ruff/rules/unused-noqa/.

---

_Comment by @MichaReiser on 2025-03-31 17:10_

That's an interesting idea. Not quiet sure how I'd go about implementing it. 

---

_Label `rule` added by @MichaReiser on 2025-03-31 17:11_

---

_Label `wish` added by @MichaReiser on 2025-03-31 17:11_

---

_Comment by @jack-mcivor on 2025-04-01 15:12_

Ah this is implemented already in RUF028 https://docs.astral.sh/ruff/rules/invalid-formatter-suppression-comment/ https://github.com/astral-sh/ruff/pull/9899


---

_Closed by @jack-mcivor on 2025-04-01 15:12_

---

_Comment by @MichaReiser on 2025-04-01 15:14_

I think that's different in that it detects suppression comments in places where they aren't valid. It doesn't detect cases where the suppression comment is unused (because the formatted statement would be exactly the same). But glad to hear that we already support what you were looking for :)

---

_Comment by @jack-mcivor on 2025-04-01 15:19_

Yeah got you. I'm migrating a large codebase from black->ruff. Actually both would be useful for me so that I could do two steps:

- Remove unused suppression comments (many are invalid but the code is formatted fine)
- For all invalid suppression comments, decide what to do (either lift into the nearest statement, or remove the suppression)

I guess finding unused & invalid suppression comments is harder than finding unused & valid suppression comments. By unused & invalid I mean even if the suppression statement were valid, it would still not result in a change to the code.

So not a burning desire to have this, but feel free to reopen

---

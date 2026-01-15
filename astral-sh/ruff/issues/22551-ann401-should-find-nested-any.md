```yaml
number: 22551
title: "ANN401 should find nested `Any`"
type: issue
state: closed
author: MartinBernstorff
labels:
  - question
assignees: []
created_at: 2026-01-13T14:04:28Z
updated_at: 2026-01-15T11:08:17Z
url: https://github.com/astral-sh/ruff/issues/22551
synced_at: 2026-01-15T11:54:30Z
```

# ANN401 should find nested `Any`

---

_@MartinBernstorff_

### Summary

The original `flake8-annotations` does not support it: https://pypi.org/project/flake8-annotations/#dynamic-typing-caveats

But it would be awesome to support in ruff! Alternatively, we/I could update the [ruff docs](https://docs.astral.sh/ruff/rules/any-type/) to document the caveat.

---

_Comment by @dylwil3 on 2026-01-13 14:36_

Thanks for the suggestion, that's an interesting idea! If I'm understanding it correctly, It sounds a bit out of scope for `ANN401`.

The justification given for `ANN401` is that an absent annotation or an annotation of `Any` doesn't provide any information. But an annotation of, say, `tuple[Any, Any]` _does_ provide information. Is that the sort of annotation you would want to lint with this rule? Or am I misunderstanding "nesting"?

---

_Label `question` added by @dylwil3 on 2026-01-13 14:41_

---

_Comment by @MartinBernstorff on 2026-01-14 09:40_

Sorry about the lack of clarity. Yes, it's exactly `tuple[Any, Any]` I would want to lint with a (potentially new) rule. 

Any use of `Any` in my codebase is _way_ too little information.

---

_Comment by @MichaReiser on 2026-01-15 09:57_

I think this is https://github.com/astral-sh/ruff/issues/5871

---

_Comment by @MartinBernstorff on 2026-01-15 10:08_

100%, sorry about the duplicate! Closing.

---

_Closed by @MartinBernstorff on 2026-01-15 10:08_

---

_Comment by @MichaReiser on 2026-01-15 11:08_

> 100%, sorry about the duplicate! Closing.

Don't worry. It can be really hard to find those. GitHub isn't really a great help here. I only found it thanks to claude.

---

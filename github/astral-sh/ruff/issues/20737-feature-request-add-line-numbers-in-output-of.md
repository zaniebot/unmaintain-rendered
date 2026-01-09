---
number: 20737
title: "[feature request] Add line numbers in output of `ruff analyze graph`"
type: issue
state: open
author: gwdekker
labels:
  - wish
  - needs-decision
  - analyze
assignees: []
created_at: 2025-10-07T06:43:33Z
updated_at: 2025-10-07T10:50:14Z
url: https://github.com/astral-sh/ruff/issues/20737
synced_at: 2026-01-07T13:12:16-06:00
---

# [feature request] Add line numbers in output of `ruff analyze graph`

---

_Issue opened by @gwdekker on 2025-10-07 06:43_

Would it be useful to add line numbers to the output of `ruff analyze graph`? This would make it possible to fully replace the functionality of `tach check` (tach is deprecated) by having all information needed to create error messages like [this](https://docs.gauge.sh/usage/commands/#dependency-errors):

```
> tach check
❌ tach/check.py[L8]: Cannot import 'tach.filesystem'. Module 'tach' cannot depend on 'tach.filesystem'. 
```

For reference: this issue came up in https://github.com/astral-sh/ruff/issues/20701


---

_Label `wish` added by @MichaReiser on 2025-10-07 07:47_

---

_Label `analyze` added by @MichaReiser on 2025-10-07 07:47_

---

_Comment by @MichaReiser on 2025-10-07 07:48_

I think that should be possible, unless it breaks some "import-grouping" that we're doing. 

Is `analyze graph` reporting failed imports? 

---

_Comment by @MichaReiser on 2025-10-07 10:26_

I think the main challenge here is that it breaks the current output format where the imports per module are just a set of paths. We would need to make this an object instead:

```
{
	path: ".../module.py",
	lines: [8, 10]
}
```

or similar. 



---

_Label `needs-decision` added by @MichaReiser on 2025-10-07 10:26_

---

_Comment by @gwdekker on 2025-10-07 10:50_

> Is `analyze graph` reporting failed imports?

I don't know - if your question is related to the example I gave: what `tach` does is it will let you check in a `tach.toml` that records the dependencies in the repo, and then with `tach check` you would get the referenced error message if there is a mismatch between `tach.toml` and what was found in the code. I can in a few tens of lines of code recreate this core functionality of `tach` it seems, the only thing I am missing to do so is the line number showcased in that message. Hence my request.

---

_Referenced in [astral-sh/ruff#20736](../../astral-sh/ruff/issues/20736.md) on 2025-11-04 06:48_

---

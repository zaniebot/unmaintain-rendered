```yaml
number: 16495
title: ruff-mutable-default (RUF008) not not catching mutable default passed to attrs.field.
type: issue
state: open
author: ashb
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-03-04T10:29:19Z
updated_at: 2025-03-04T11:21:14Z
url: https://github.com/astral-sh/ruff/issues/16495
synced_at: 2026-01-10T11:09:57Z
```

# ruff-mutable-default (RUF008) not not catching mutable default passed to attrs.field.

---

_Issue opened by @ashb on 2025-03-04 10:29_

Simple repro case
```
❯ cat tst.py
import attrs

@attrs.define
class B :
    mutable_default: list[int] = []
    mutable_default2: list[int] = attrs.field(default=[])

❯ uvx ruff@0.9.9 check --extend-select RUF008,B006 --preview -n --isolated  tst.py
All checks passed!
```

_Originally posted by @ashb in https://github.com/astral-sh/ruff/issues/14327#issuecomment-2696968093_

Output:

```
tst.py:5:34: RUF008 Do not use mutable default values for dataclass attributes
  |
3 | @attrs.define
4 | class B :
5 |     mutable_default: list[int] = []
  |                                  ^^ RUF008
6 |     mutable_default2: list[int] = attrs.field(default=[])
  |
```

I was hoping it would catch the same error on `mutable_default2`

---

_Comment by @MichaReiser on 2025-03-04 10:33_

Can you tell me more about what diagnostics you'd expect?

E.g. this raises RUF012 for the `mutable_default` field https://play.ruff.rs/98850596-5ef1-4a1e-a3e5-fa07ed757a81

---

_Label `needs-info` added by @MichaReiser on 2025-03-04 10:33_

---

_Comment by @AlexWaygood on 2025-03-04 10:35_

I think this might only check `attrs` classes if preview mode is enabled. @ashb do you have `preview` mode enabled in your ruff config?

---

_Comment by @MichaReiser on 2025-03-04 10:36_

> I think this might only check `attrs` classes if preview mode is enabled. [@ashb](https://github.com/ashb?rgh-link-date=2025-03-04T10%3A35%3A03.000Z) do you have `preview` mode enabled in your ruff config?

Preview mode is enabled, see the CLI command

---

_Comment by @MichaReiser on 2025-03-04 10:37_

Shouldn't it be `@attrs.define` instead of `@attrs.deinfe`? 


---

_Comment by @ashb on 2025-03-04 10:41_

🤦🏻 Okay, so that fixes the first one, but not the second. I guess it's up for debate if the second should be caught or not.

Updated top post

---

_Renamed from "ruff-mutable-default (RUF008) not working for attrs classes" to "ruff-mutable-default (RUF008) not not catching mutable default passed to attrs.field." by @ashb on 2025-03-04 10:42_

---

_Label `rule` added by @MichaReiser on 2025-03-04 10:44_

---

_Label `help wanted` added by @MichaReiser on 2025-03-04 10:44_

---

_Label `needs-info` removed by @MichaReiser on 2025-03-04 10:44_

---

_Label `no-test` added by @MichaReiser on 2025-03-04 10:44_

---

_Comment by @MichaReiser on 2025-03-04 10:45_

Thanks for updating the summary. Adding support for `attrs.field` (in preview) does make sense to me, considering that https://github.com/astral-sh/ruff/pull/14327#issuecomment-2696968093 introduced attrs support.

---

_Label `no-test` removed by @MichaReiser on 2025-03-04 10:45_

---

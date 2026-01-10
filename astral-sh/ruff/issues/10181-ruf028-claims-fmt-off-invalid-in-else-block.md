```yaml
number: 10181
title: "RUF028 claims `fmt: off` invalid in `else:` block"
type: issue
state: closed
author: mrcljx
labels:
  - bug
  - rule
assignees: []
created_at: 2024-03-01T09:59:27Z
updated_at: 2024-03-05T03:12:33Z
url: https://github.com/astral-sh/ruff/issues/10181
synced_at: 2026-01-10T11:09:52Z
```

# RUF028 claims `fmt: off` invalid in `else:` block

---

_Issue opened by @mrcljx on 2024-03-01 09:59_

RUF028 claims that the second `fmt: off` is illegal (the first one is fine). Both are actually respected by the formatter though.

Expecation: RUF028 should not trigger.

```
def f(a, b):
    if not a:
        # fmt: off
        a = b or bytes(
            [
                0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
                0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
            ]
        )
    else:
        # fmt: off
        a = b or bytes(
            [
                0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
                0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 
            ]
        )
```

![CleanShot 2024-03-01 at 11 00 00](https://github.com/astral-sh/ruff/assets/56807/19eefcaa-08f5-4b90-80e2-8a20c30c2551)


https://play.ruff.rs/719df52a-6744-4230-8cf8-ed64a512143c

Ruff: 0.3.0



---

_Comment by @MichaReiser on 2024-03-01 10:01_

Thanks for reporting. That's a bug. I think https://github.com/astral-sh/ruff/pull/10178 fixed it but I let @snowsignal confirm

---

_Label `bug` added by @MichaReiser on 2024-03-01 10:01_

---

_Label `rule` added by @MichaReiser on 2024-03-01 10:01_

---

_Assigned to @snowsignal by @MichaReiser on 2024-03-01 10:01_

---

_Comment by @mrcljx on 2024-03-01 10:03_

Ah, nice. I only saw the title of the issue and not the PR summary that suggests this was fixed indeed.

---

_Comment by @snowsignal on 2024-03-05 03:12_

This should be fixed by https://github.com/astral-sh/ruff/pull/10178 in the next release. I confirmed that your code passes without issue on the latest version of the Ruff playground.

---

_Closed by @snowsignal on 2024-03-05 03:12_

---

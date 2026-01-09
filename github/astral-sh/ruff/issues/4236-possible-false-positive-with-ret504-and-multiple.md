---
number: 4236
title: possible false positive with RET504 and multiple-assignements
type: issue
state: closed
author: Borda
labels:
  - bug
assignees: []
created_at: 2023-05-05T07:22:55Z
updated_at: 2023-06-10T04:05:03Z
url: https://github.com/astral-sh/ruff/issues/4236
synced_at: 2026-01-07T13:12:14-06:00
---

# possible false positive with RET504 and multiple-assignements

---

_Issue opened by @Borda on 2023-05-05 07:22_

I have the following code sample, and here I do think it is correct:
```py
    def consume_result(self) -> T:
        if self._result is None:
            raise MisconfigurationException()
        result, self._result = self._result, None  # free memory
        return result
```


---

_Referenced in [Lightning-AI/pytorch-lightning#17540](../../Lightning-AI/pytorch-lightning/pulls/17540.md) on 2023-05-05 07:23_

---

_Label `bug` added by @charliermarsh on 2023-05-05 16:58_

---

_Comment by @charliermarsh on 2023-05-05 17:09_

Yeah makes sense. This rule is so unreliable, it's just really hard to avoid false positives while retaining much utility.

---

_Comment by @MichaReiser on 2023-05-08 10:42_

> Yeah makes sense. This rule is so unreliable, it's just really hard to avoid false positives while retaining much utility.

I agree that this code per se is correct and we should, if possible, detect and support it. But this can also be a case where a slight restructuring of the code fixes the false-positive (by splitting the code into two assignments)

---

_Comment by @Borda on 2023-05-08 13:47_

> But this can also be a case where a slight restructuring of the code fixes the false-positive (by splitting the code into two assignments)

Yes, but then you drop the beauty of puthon and memory efficiency too... :)
(in the end all can be just C code :chipmunk:)

---

_Referenced in [astral-sh/ruff#4997](../../astral-sh/ruff/pulls/4997.md) on 2023-06-10 02:42_

---

_Closed by @charliermarsh on 2023-06-10 04:05_

---

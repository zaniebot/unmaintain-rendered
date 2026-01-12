```yaml
number: 12559
title: "Docs: Clarify uvx on PyPI is unofficial."
type: pull_request
state: closed
author: FishAlchemist
labels: []
assignees: []
base: main
head: doc_PyPI_uvx
created_at: 2025-03-30T16:34:55Z
updated_at: 2025-04-16T14:59:10Z
url: https://github.com/astral-sh/uv/pull/12559
synced_at: 2026-01-12T16:10:19Z
```

# Docs: Clarify uvx on PyPI is unofficial.

---

_@FishAlchemist_

## Summary

Due to recent instances in https://github.com/astral-sh/uv/issues/12552#issuecomment-2764503857 mistakenly used the PyPI package ``uvx`` as the official package, it's worth mentioning in the documentation.
uvx on PyPI: https://pypi.org/project/uvx/

The README currently includes pip installation instructions. However, due to uncertainty regarding the most concise way to present this information, it will be omitted for the time being.
## Test Plan
Run docs server in local
![image](https://github.com/user-attachments/assets/b0474b11-0b60-414a-9332-65fdf8024444)

<!-- How was it tested? -->


---

_Comment by @basnijholt on 2025-04-01 17:10_

I opened an issue in https://github.com/robinvandernoord/uvenv/issues/5 to ask @robinvandernoord to transfer ownership of the PyPI `uvx` package to the `uv` maintainers. Hopefully, this issue can then be resolved in a more intuitive way 😄 

---

_Assigned to @zanieb by @zanieb on 2025-04-01 17:20_

---

_Comment by @zanieb on 2025-04-01 17:21_

Thanks @basnijholt

Let's see what the best path forward is here. I'm hesitant to add a warning to the install instructions like this, but it might be necessary.

---

_Comment by @robinvandernoord on 2025-04-01 19:46_

Hi, thanks for mentioning me here. Because of this naming collision, I changed the name from `uvx` to `uvenv` a few months ago.
I still had the package up on pypi during the transition but I've had adeprecation warning (both on pypi and in the tool itself) for a while so I think it would be fine to yank the package to prevent confusion.

Would you prefer I just take the pypi project down or should I transfer it to someone?

---

_Comment by @zanieb on 2025-04-01 19:53_

@robinvandernoord it seems best to transfer it so we can guard against dependency confusion attacks in the future. Want to send an email to `zanie at astral.sh` and we can coordinate?

---

_Comment by @FishAlchemist on 2025-04-16 13:16_

Progress update:
![image](https://github.com/user-attachments/assets/746a672d-3d1f-48e5-8659-cceb98ba60b3)

I think this PR can be put on the agenda to decide if it's needed.

---

_Comment by @zanieb on 2025-04-16 14:57_

We still need to publish a dummy package, but yeah we don't need the docs change.

---

_Closed by @zanieb on 2025-04-16 14:57_

---

_Branch deleted on 2025-04-16 14:59_

---

```yaml
number: 4303
title: No solution found when resolving dependencies (torch==2.3.1)
type: issue
state: closed
author: mariomeissner
labels:
  - question
assignees: []
created_at: 2024-06-13T04:51:57Z
updated_at: 2024-06-13T23:41:20Z
url: https://github.com/astral-sh/uv/issues/4303
synced_at: 2026-01-10T05:31:37Z
```

# No solution found when resolving dependencies (torch==2.3.1)

---

_Issue opened by @mariomeissner on 2024-06-13 04:51_

UV Version: `0.2.11`

Code snippet:
```
pip install torch==2.3.1 --index-url https://download.pytorch.org/whl/cpu # works as expected

uv pip install torch==2.3.1 --index-url https://download.pytorch.org/whl/cpu # fails
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.3.1 has no wheels are available with a matching Python implementation and you require torch==2.3.1, we can conclude
      that the requirements are unsatisfiable.
```

I saw #4252, #4253 which seemed very much related, but it seems to have been merged and included in the `0.2.11`. However, I am still facing the above issue, so I am guessing there is more to it.


---

_Comment by @FishAlchemist on 2024-06-13 11:09_

I don't know why, but if you use ``2.3.1+cpu`` or don't specify a version, it should install successfully.

update:
This seems to be the limitation:
https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers

---

_Comment by @charliermarsh on 2024-06-13 19:22_

Yeah, you may need to include the local version segment -- there's more detail in the doc that @FishAlchemist linked.

---

_Label `question` added by @charliermarsh on 2024-06-13 19:22_

---

_Comment by @mariomeissner on 2024-06-13 23:41_

Ah, I guess this excerpt covers it:
> If only local versions exist for a package foo at a given version (e.g., 1.2.3+local exists, but 1.2.3 does not), uv pip install foo==1.2.3 will fail, while pip install foo==1.2.3 will resolve to an arbitrary local version.

Adding `+cpu` sounds reasonable, let me proceed with that. Thank you both!

---

_Closed by @mariomeissner on 2024-06-13 23:41_

---

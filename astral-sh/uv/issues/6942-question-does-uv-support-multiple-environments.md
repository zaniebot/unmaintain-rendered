```yaml
number: 6942
title: "Question: Does `uv` support multiple environments?"
type: issue
state: closed
author: asmith26
labels: []
assignees: []
created_at: 2024-09-02T20:03:53Z
updated_at: 2025-03-15T01:37:54Z
url: https://github.com/astral-sh/uv/issues/6942
synced_at: 2026-01-12T15:59:09Z
```

# Question: Does `uv` support multiple environments?

---

_@asmith26_

Hi there,

Just wondering, does `uv` support multiple environments (I see I can specify "application dependencies" and "dev dependencies", but could I e.g. create an env for "cpu" and another for "gpu")?

Many thanks for any help, and for this lib!

---

_Comment by @zanieb on 2024-09-03 14:57_

No we don't have multiple environment support right now — the closest is probably to use different groups of `optional-dependencies`.

---

_Comment by @asmith26 on 2024-09-03 17:13_

Thanks for your help and info @zanieb 

---

_Closed by @asmith26 on 2024-09-03 17:13_

---

_Renamed from "Question: Does `uv` support multiple environements?" to "Question: Does `uv` support multiple environments?" by @asmith26 on 2024-09-03 17:14_

---

_Comment by @ameya98 on 2024-12-16 15:07_

Sorry for re-opening this, but is this still the case? If I want two environments in the same folder, I get this error: 
```bash
warning: `VIRTUAL_ENV=.venv-analysis` does not match the project environment path `.venv` and will be ignored
```
which doesn't make a lot of sense to me.

---

_Comment by @renardeinside on 2024-12-28 21:40_

Hi @zanieb , 
`hatch` provides quite a convenient way to actually separate envrionments, not just groups. 

This might be useful in scenarios when packages are incompatible and you need to use a couple of envs, or when you want cross-python testing. Would you consider this feature in future? 

---

_Comment by @sohang3112 on 2025-02-21 01:19_

@asmith26 Is there any way currently to have multiple environments (eg. gpu, cpu) in the same folder? If not, this issue should be re-opened - this is a very common use case. 

---

_Comment by @adrianloy on 2025-03-14 13:03_

I am also interested in this. I need a venv to run preprocessing and another one to run model training. There are package conflicts that prevent me from doing both in the same env, which is generally fine by me as I do not nee to run these entrypoints in the same environment. 

---

_Comment by @konstin on 2025-03-14 20:41_

@adrianloy While uv does not support multiple environments, it has support for declaring [conflicting dependencies](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies).

---

_Comment by @sohang3112 on 2025-03-15 01:37_

>While uv does not support multiple environments, it has support for declaring [conflicting dependencies](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies).

@konstin That sounds useful!



---

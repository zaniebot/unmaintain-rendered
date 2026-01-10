---
number: 2840
title: Error with multiple requirements files, one containing an --extra-index-url
type: issue
state: closed
author: davidhao3300
labels: []
assignees: []
created_at: 2024-04-05T19:15:12Z
updated_at: 2024-04-05T19:21:20Z
url: https://github.com/astral-sh/uv/issues/2840
synced_at: 2026-01-10T01:23:22Z
---

# Error with multiple requirements files, one containing an --extra-index-url

---

_Issue opened by @davidhao3300 on 2024-04-05 19:15_

Consider the following contents:
req1.txt: `filelock==3.12.4`
req2.txt: 
```
--extra-index-url https://download.pytorch.org/whl/cpu

torch==2.0.1
```

Running `uv pip install -r req1.txt -r req2.txt` fails with
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of filelock==3.12.4 and you require filelock==3.12.4, we can conclude that the requirements are
      unsatisfiable.
```

It appears to be the --extra-index-url that causes issues, removing fixes the issue.



---

_Comment by @charliermarsh on 2024-04-05 19:16_

Thanks, explained in detail here including ways to fix it: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes.

---

_Comment by @davidhao3300 on 2024-04-05 19:19_

ah, thank you very much, thanks for the info. From reading this, it appears that we expect uv to first check the extra index, before falling back to the default index?

Does this mean the torch index contains filelock in it, but not the version we need?

---

_Comment by @davidhao3300 on 2024-04-05 19:20_

Appears to be the case - https://download.pytorch.org/whl/filelock/

---

_Comment by @charliermarsh on 2024-04-05 19:20_

That's correct -- the Torch index only contains 3.9.0: https://download.pytorch.org/whl/filelock/

---

_Comment by @charliermarsh on 2024-04-05 19:20_

You should be able to pass `--index-strategy unsafe-any-match` if you want to allow `uv` to look at versions across any index. Or you can use 3.9.0 :)

---

_Comment by @davidhao3300 on 2024-04-05 19:21_

Okay! I think the workaround is sufficient, and swap over to https://github.com/astral-sh/uv/issues/171 when it's available, thank you for the incredibly fast response time!

---

_Closed by @davidhao3300 on 2024-04-05 19:21_

---

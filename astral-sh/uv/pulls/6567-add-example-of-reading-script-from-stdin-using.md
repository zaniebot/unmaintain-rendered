```yaml
number: 6567
title: Add example of reading script from stdin using echo
type: pull_request
state: merged
author: skykasko
labels:
  - documentation
assignees: []
merged: true
base: main
head: sky/echo-stdin
created_at: 2024-08-24T03:52:57Z
updated_at: 2024-08-25T17:14:23Z
url: https://github.com/astral-sh/uv/pull/6567
synced_at: 2026-01-10T13:09:51Z
```

# Add example of reading script from stdin using echo

---

_Pull request opened by @skykasko on 2024-08-24 03:52_

As a non-shell-wizard, I was unfamiliar with the `EOF` syntax used in the existing example (just above the one I added). I thought including an example where the output of `echo` is piped to `uv run` might be more accessible. As a bonus, it should work across more shells: the `EOF` example doesn't work in fish because fish [doesn't support heredocs](https://fishshell.com/docs/current/fish_for_bash_users.html#heredocs), while the `echo` example does.

Feel free to ignore if unwanted.


---

_@zanieb reviewed on 2024-08-24 04:06_

---

_Review comment by @zanieb on `docs/guides/scripts.md`:71 on 2024-08-24 04:06_

Maybe we should just show this as a one-line, e.g. `$ echo 'print("hello world!")' | uv run -` with "console" instead of "bash" syntax? I'd even show this first, then say something like... "Or, if your shell supports heredocs:"

---

_@skykasko reviewed on 2024-08-24 04:15_

---

_Review comment by @skykasko on `docs/guides/scripts.md`:71 on 2024-08-24 04:15_

That sounds fine to me. My first thought had been to write it as a one-liner, but then I figured that the reason for the way the existing example was written was that most scripts users might want to run would have multiple lines, so the example should make it easy to add more lines to the script. But your suggestion is cleaner, and users can easily convert the one-liner to a multi-liner, so I'll make the change you suggested.

---

_Review comment by @zanieb on `docs/guides/scripts.md`:71 on 2024-08-24 04:24_

👍 thanks!

---

_@zanieb reviewed on 2024-08-24 04:24_

---

_Label `documentation` added by @zanieb on 2024-08-24 04:24_

---

_Review requested from @zanieb by @skykasko on 2024-08-24 04:33_

---

_@zanieb approved on 2024-08-24 05:03_

---

_Merged by @zanieb on 2024-08-24 05:04_

---

_Closed by @zanieb on 2024-08-24 05:04_

---

_Comment by @zanieb on 2024-08-24 05:04_

Welcome to the project :) thank you

---

_Branch deleted on 2024-08-24 05:06_

---

_Comment by @billy-doyle on 2024-08-25 17:14_

great addition @skykasko, totally didnt even think when adding the EOF example that echo pipe would also work!

---

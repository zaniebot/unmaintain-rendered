```yaml
number: 5381
title: "Allow `uv add` sources to be specified outside of the requirement"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - cli
  - tracking
assignees: []
created_at: 2024-07-23T22:19:18Z
updated_at: 2026-01-08T21:37:43Z
url: https://github.com/astral-sh/uv/issues/5381
synced_at: 2026-01-10T03:11:31Z
```

# Allow `uv add` sources to be specified outside of the requirement

---

_Issue opened by @zanieb on 2024-07-23 22:19_

e.g. 

```
uv add [<package>] --git <url> 
uv add [<package>] --path <path> 
uv add [<package>] --url <url> 
uv add [<package>] --workspace <path>
```

These user should be able to optionally include the package name and specifiers, e.g. `uv add "httpx>0.2.0" --git https://github.com/encode/httpx`

See:
- #5383
- #5384
- #5385
- #5386

---

_Label `tracking` added by @zanieb on 2024-07-23 22:19_

---

_Label `cli` added by @zanieb on 2024-07-23 22:24_

---

_Label `preview` added by @zanieb on 2024-07-23 22:24_

---

_Label `help wanted` added by @zanieb on 2024-07-23 22:27_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Comment by @LoukasPap on 2026-01-08 21:37_

@zanieb Is this resolved or does it still need to be done?

---

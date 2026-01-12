```yaml
number: 1490
title: activate.nu error
type: issue
state: closed
author: Arengard
labels:
  - bug
  - virtualenv
assignees: []
created_at: 2024-02-16T13:59:01Z
updated_at: 2024-04-29T20:33:19Z
url: https://github.com/astral-sh/uv/issues/1490
synced_at: 2026-01-12T15:58:28Z
```

# activate.nu error

---

_@Arengard_

Scripts\activate.nu:95:1]
 95 │ export alias pydoc = python -m pydoc
 96 │ export alias deactivate = overlay hide activate
    ·                           ───┬───
    ·                              ╰── not allowed in pipeline
    ╰────
  help: 'overlay' keyword is not allowed in pipeline. Use 'overlay' by itself, outside of a pipeline.

---

_Label `bug` added by @MichaReiser on 2024-02-16 14:13_

---

_Comment by @AucaCoyan on 2024-02-16 15:37_

Mmm this is a `virtualenv` issue, which is caused by upstream `nushell` shell script.
to better readability, I copy and paste in a monospaced font
```bash
  × Statement used in pipeline.
    ╭─[C:\Users\aucac\repos\exp-python\uv\.venv\Scripts\activate.nu:95:1]
 95 │ export alias pydoc = python -m pydoc
 96 │ export alias deactivate = overlay hide activate
    ·                           ───┬───
    ·                              ╰── not allowed in pipeline
    ╰────
  help: 'overlay' keyword is not allowed in pipeline. Use 'overlay' by itself, outside of a pipeline.
  ```
  
  I could track this issue [firstly in `virtualenv`](https://github.com/pypa/virtualenv/issues?q=is%3Aissue+activate.nu) and then [in the issues of nushell/nushell](https://github.com/nushell/nushell/issues?q=is%3Aissue+%27overlay%27+keyword+is+not+allowed+in+pipeline.+Use+%27overlay%27+by+itself%2C+outside+of+a+pipeline.)
  
 I think is solvable, but it has nothing to do with `uv`
  

---

_Comment by @zanieb on 2024-02-16 16:59_

Thank you! I think we vendor this script so we'd need to update once fixed upstream.

---

_Comment by @melMass on 2024-02-17 18:34_

Hi,

Nushell user here, and I partly wrote that script. What version of nushell are you on?

<img src="https://github.com/astral-sh/uv/assets/7041726/b1ce1bf8-0b66-4688-8905-d39368952e23" width=400/>


I'm currently refactoring the script but it should still work as is in nushell 0.89+

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @AucaCoyan on 2024-04-29 15:03_

Hi! I'm using uv and venv in nushell with no errors. Do you keep getting issues @Arengard ?

---

_Closed by @Arengard on 2024-04-29 20:33_

---

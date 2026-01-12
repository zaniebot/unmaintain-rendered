```yaml
number: 7765
title: "Ruff throws `SyntaxError` to 3.12 PEP701 f-string changes"
type: issue
state: closed
author: danparizher
labels: []
assignees: []
created_at: 2023-10-02T19:05:55Z
updated_at: 2023-10-02T19:23:37Z
url: https://github.com/astral-sh/ruff/issues/7765
synced_at: 2026-01-12T15:54:47Z
```

# Ruff throws `SyntaxError` to 3.12 PEP701 f-string changes

---

_@danparizher_

```py
songs = ["Take me back to Eden", "Alkaline", "Ascensionism"]
print(f"This is the playlist: {", ".join(songs)}")
```
![image](https://github.com/astral-sh/ruff/assets/105245560/8daea7fd-04e6-4152-8f25-70d4de7f51d6)


---

_Comment by @konstin on 2023-10-02 19:07_

What ruff version are you using? The example works for me on ruff 0.0.292:
```console
$ pipx run ruff==0.0.292 check --select D100 scratch.py
scratch.py:1:1: D100 Missing docstring in public module
Found 1 error.
```

---

_Comment by @danparizher on 2023-10-02 19:09_

> What ruff version are you using? The example works for me on ruff 0.0.292:
> 
> ```
> $ pipx run ruff==0.0.292 check --select D100 scratch.py
> scratch.py:1:1: D100 Missing docstring in public module
> Found 1 error.
> ```

I'm using `ruff-0.0.292`, but it may be that the VSC extension has not been updated yet, as it is on `v2023.39.12651833`

---

_Comment by @charliermarsh on 2023-10-02 19:23_

Yeah the VS Code extension hasn't been updated. You can configure it to use your local Ruff rather than the version that comes bundled with the extension (see settings like `path`: https://github.com/astral-sh/ruff-vscode#settings).

---

_Closed by @charliermarsh on 2023-10-02 19:23_

---

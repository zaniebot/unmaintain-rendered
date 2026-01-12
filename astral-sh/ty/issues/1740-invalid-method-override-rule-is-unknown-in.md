```yaml
number: 1740
title: invalid-method-override rule is unknown in pyproject.toml
type: issue
state: closed
author: f-schnabel
labels:
  - question
assignees: []
created_at: 2025-12-03T10:56:52Z
updated_at: 2025-12-03T11:25:42Z
url: https://github.com/astral-sh/ty/issues/1740
synced_at: 2026-01-12T15:54:25Z
```

# invalid-method-override rule is unknown in pyproject.toml

---

_@f-schnabel_

### Summary

Using the VS Code plugin and ignoring the `invalid-method-override` rule, I get a warning that this rule does not exist, even though it was being reported and is ignored via this line.
<img width="445" height="79" alt="Image" src="https://github.com/user-attachments/assets/1af5696b-e29b-44df-9951-4891b33d1e91" />

### Version

 2025.59.13322033

---

_Comment by @MichaReiser on 2025-12-03 11:14_

I suspect that the VS code extension picks up an older ty version that you've installed locally. 

Can you open the "Output" panel in VS code, then select "ty" from the dropdown (right next to the "Filter" input field)

The output should look like this:

```
2025-12-03 12:13:15.075 [info] Name: ty
2025-12-03 12:13:15.075 [info] Module: ty
2025-12-03 12:13:15.075 [info] Python extension loading
2025-12-03 12:13:15.075 [info] Waiting for interpreter from python extension.
2025-12-03 12:13:15.526 [info] Python extension loaded
2025-12-03 12:13:15.527 [info] Using interpreter: /Users/micha/astral/test/.venv/bin/python
2025-12-03 12:13:15.528 [info] Initialization options: {
    "logLevel": "debug"
}
2025-12-03 12:13:15.530 [info] Using 'path' setting: /Users/micha/astral/ruff/target/debug/ty
2025-12-03 12:13:15.532 [info] Found executable at /Users/micha/astral/ruff/target/debug/ty
2025-12-03 12:13:15.532 [info] Server run command: /Users/micha/astral/ruff/target/debug/ty server
2025-12-03 12:13:15.533 [info] Server: Start requested.
```

The important line is `Server run command: /Users/micha/astral/ruff/target/debug/ty server`. You can see how the extension uses a local ty version in my setup. 

---

_Label `question` added by @MichaReiser on 2025-12-03 11:15_

---

_Comment by @f-schnabel on 2025-12-03 11:17_

Hi sure, here is the output:
```
2025-12-03 10:41:33.348 [info] Name: ty
2025-12-03 10:41:33.348 [info] Module: ty
2025-12-03 10:41:33.348 [info] Python extension loading
2025-12-03 10:41:33.348 [info] Waiting for interpreter from python extension.
2025-12-03 10:41:33.348 [info] Python extension loaded
2025-12-03 10:41:33.348 [info] Using interpreter: e:\Programming\uni\IDP\coaching-agent-py\.venv\Scripts\python.exe
2025-12-03 10:41:33.348 [info] Initialization options: {}
2025-12-03 10:41:33.454 [info] Using the ty binary: e:\Programming\uni\IDP\coaching-agent-py\.venv\Scripts\ty.exe
2025-12-03 10:41:33.456 [info] Found executable at e:\Programming\uni\IDP\coaching-agent-py\.venv\Scripts\ty.exe
2025-12-03 10:41:33.456 [info] Server run command: e:\Programming\uni\IDP\coaching-agent-py\.venv\Scripts\ty.exe server
2025-12-03 10:41:33.457 [info] Server: Start requested.
```
It looks like it is using the same as in my .venv folder, but I run ty with `uvx ty check`, which doesnt use the one from the venv?


---

_Comment by @f-schnabel on 2025-12-03 11:23_

Oh yeah, now I see it. A dependency of mine "Instructor" has a dependency on ty, which polutes my .venv:
https://github.com/567-labs/instructor/blob/130c034c4b7ec9fe6b7d06ad836fa1318f78fd43/pyproject.toml#L19
They should probably mark that a dev dependency...

---

_Comment by @MichaReiser on 2025-12-03 11:24_

> Oh yeah, now I see it. A dependency of mine "Instructor" has a dependency on ty, which polutes my .venv:

Uhm yes, that's not great 😅  They should make it a dev dependency.

For now, you could specify the `ty.path` setting in VS Code and point it to the correct version.

---

_Closed by @f-schnabel on 2025-12-03 11:25_

---

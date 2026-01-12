```yaml
number: 10242
title: "Improve clarity of `PT006`'s error message"
type: issue
state: closed
author: trag1c
labels:
  - documentation
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2024-03-05T21:14:12Z
updated_at: 2024-03-20T18:22:03Z
url: https://github.com/astral-sh/ruff/issues/10242
synced_at: 2026-01-12T15:54:50Z
```

# Improve clarity of `PT006`'s error message

---

_@trag1c_

I was a bit startled by this when writing tests:
```py
@pytest.mark.parametrize(
    ("types",),
#   ^^^^^^^^^^  Wrong name(s) type in `@pytest.mark.parametrize`, expected `csv`
    []
)
def test_load_config_pass(...):
    ...
```
For the first few seconds I thought it was suggesting the name `"csv"` as a param name and that this was some sort of a buggy message :smile: I decided to take a look at [PT006's page](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/) but saw no mention of `csv` there. I then found a related [settings section](https://docs.astral.sh/ruff/settings/#lint_flake8-pytest-style_parametrize-names-type) that explained that this refers to listing parameters as a comma-separated string, but I was still a little confused.

What made me realize what this is all about was the applied autofix 😅
```diff
 @pytest.mark.parametrize(
-    ("types",),
+    "types",
     []
 )
```

I think this rule could use a more obvious error message, e.g. ``Use a string for one-parameter `@pytest.mark.parametrize`​`` :+1:

---

_Comment by @MichaReiser on 2024-03-06 09:10_

Agree, the message could be improved. It's not clear to me what I have to do from just reading the message.

---

_Label `rule` added by @MichaReiser on 2024-03-06 09:10_

---

_Label `help wanted` added by @MichaReiser on 2024-03-06 09:10_

---

_Label `documentation` added by @zanieb on 2024-03-11 17:15_

---

_Label `good first issue` added by @zanieb on 2024-03-11 17:15_

---

_Assigned to @zanieb by @zanieb on 2024-03-11 17:15_

---

_Comment by @zanieb on 2024-03-11 17:16_

I've assigned myself to this but please feel free to open a pull request if I have not fixed it yet and you're interested.

---

_Closed by @AlexWaygood on 2024-03-20 18:22_

---

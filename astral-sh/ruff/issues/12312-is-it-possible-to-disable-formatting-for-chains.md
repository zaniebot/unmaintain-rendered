```yaml
number: 12312
title: Is it possible to disable formatting for chains with line-breaking? 
type: issue
state: closed
author: VladimirPodolian
labels:
  - question
assignees: []
created_at: 2024-07-13T20:29:20Z
updated_at: 2024-09-09T12:05:17Z
url: https://github.com/astral-sh/ruff/issues/12312
synced_at: 2026-01-12T15:54:51Z
```

# Is it possible to disable formatting for chains with line-breaking? 

---

_@VladimirPodolian_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Ruff command:
```
ruff format
```

Default ruff.toml + these updates
```ruff.toml
line-length = 120
quote-style = "single"
```

Ruff version:
```
ruff 0.5.1
```

Hi there! I would like to use ruff on my work project and on myself project for selenium. But i'm facing an issue, that it formats lines like this 

```python
        self._action_chains\
            .move_to_element(self.element)\
            .move_by_offset(1, 1)\
            .move_to_element(self.element)\
            .perform()
```

To this 

```python
self._action_chains.move_to_element(self.element).move_by_offset(1, 1).move_to_element(self.element).perform()
```

Is it possible to disable formatting for chains with line-breaking on whole project ? 

---

_Label `question` added by @MichaReiser on 2024-07-15 06:32_

---

_Comment by @MichaReiser on 2024-07-15 06:35_

Hi @VladimirPodolian 

I'm sorry. There's currently no option to disable chain formatting other than using suppression comments. 

```python
self._action_chains
    .move_to_element(self.element)
    .move_by_offset(1, 1)
    .move_to_element(self.element)
    .perform() # fmt: skip
```

But that turns off all formatting and is rather verbose. 

There's a tracking issue for improving [call chain formatting](https://github.com/astral-sh/ruff/issues/8598) overall. Unfortunately, chain formatting is especially hard in Python because line breaks are only allowed in parenthesized context (line-continuation tokens are an option but not something that the majority of the community prefers). 

---

_Closed by @MichaReiser on 2024-09-09 12:05_

---

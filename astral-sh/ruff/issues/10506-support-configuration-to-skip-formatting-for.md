```yaml
number: 10506
title: Support configuration to skip formatting for certain lines.
type: issue
state: open
author: zen-xu
labels:
  - suppression
assignees: []
created_at: 2024-03-21T09:54:35Z
updated_at: 2024-03-21T10:20:50Z
url: https://github.com/astral-sh/ruff/issues/10506
synced_at: 2026-01-12T15:54:50Z
```

# Support configuration to skip formatting for certain lines.

---

_@zen-xu_

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

Ruff formatter is awesome, and I have completely migrated my project from Black.

In addition, I am very eager for it to support the configuration to not format certain matching lines, rather than manually specifying in the code. Because I have some DSLs implemented in Python, I don't want Ruff to format those related codes. It would be great if Ruff had a related configuration to set this globally.


---

_Label `configuration` added by @AlexWaygood on 2024-03-21 10:00_

---

_Label `formatter` added by @AlexWaygood on 2024-03-21 10:00_

---

_Comment by @zen-xu on 2024-03-21 10:08_

for example, I have a pytest BDD [plugin](https://github.com/zen-xu/spock) (It hasn't been maintained for a long time 😅), its DSL is similar to a table.

```python
import pytest


@pytest.mark.spock("maximum of {first} and {second} is {ret}")
def test_maximum():
    def expect(first, second, ret):
        assert max(first, second) == ret

    def where(_, first, second, ret):
        _ | first | second | ret
        _ | 3     | 7      | 7
        _ | 5     | 4      | 5
        _ | 9     | 9      | 9
```

ruff will format these code into
```python
import pytest


@pytest.mark.spock("maximum of {first} and {second} is {ret}")
def test_maximum():
    def expect(first, second, ret):
        assert max(first, second) == ret

    def where(_, first, second, ret):
        _ | first | second | ret
        _ | 3 | 7 | 7
        _ | 5 | 4 | 5
        _ | 9 | 9 | 9
```


---

_Comment by @zen-xu on 2024-03-21 10:14_

Additionally, it is also necessary to turn off lint for certain lines.


---

_Label `configuration` removed by @MichaReiser on 2024-03-21 10:14_

---

_Label `suppression` added by @MichaReiser on 2024-03-21 10:14_

---

_Label `formatter` removed by @MichaReiser on 2024-03-21 10:14_

---

_Comment by @MichaReiser on 2024-03-21 10:20_

> Ruff formatter is awesome, and I have completely migrated my project from Black.

Thank you!

I can see the need for a configuration option to exclude certain code patterns from linting and formatting, and AST Grep (or GritIo) patterns could be a nice way of specifying that. I would rather not do a string-based search because that is error-prone and imposes the challenge that Ruff would need to map string offsets back to AST nodes (it's unclear what should happen if the start doesn't match the node offsets directly). 

I'm afraid that this is probably a more distant feature because it requires building some infrastructure to support this and a very stable AST representation (changes to the AST would become breaking changes)

---

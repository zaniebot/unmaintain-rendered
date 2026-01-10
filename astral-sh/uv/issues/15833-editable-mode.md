---
number: 15833
title: editable mode
type: issue
state: closed
author: gilgamesh1111
labels:
  - question
assignees: []
created_at: 2025-09-14T06:12:42Z
updated_at: 2025-09-14T16:15:40Z
url: https://github.com/astral-sh/uv/issues/15833
synced_at: 2026-01-10T01:26:00Z
---

# editable mode

---

_Issue opened by @gilgamesh1111 on 2025-09-14 06:12_

### Question

I set my_project  use
```bash
uv init my_project
uv sync
```
and then ,the I set the project as src layout like this

<img width="317" height="570" alt="Image" src="https://github.com/user-attachments/assets/efebbeb2-eec8-420c-bee3-a070ffabb1fe" />

in `__init__.py` 
```python
# __init__.py
__version__="0.1.0"
```

in main.py
```python
# main.py
import my_project

print(my_project.__version__)
```

I use two ways to run it

<img width="1931" height="427" alt="Image" src="https://github.com/user-attachments/assets/8aab242c-5930-4b30-ae31-447945a0f9a7" />

I think it should be set editable mode by defult,, in the `uv.lock` file there is

```
version = 1
revision = 3
requires-python = ">=3.11"

[[package]]
name = "my-project"
version = "0.1.0"
source = { virtual = "." }
```
but why I can't import my_project
### Platform
Windows

### Version
uv 0.8.17 (10960bc13 2025-09-10

---

_Label `question` added by @gilgamesh1111 on 2025-09-14 06:12_

---

_Comment by @nick-voisin on 2025-09-14 08:58_

Hi,

I think what you want is to do `uv init --lib my_project`. My uv.lock looks like the following for `test-project`:

```
`version = 1
revision = 3
requires-python = ">=3.12"

[[package]]
name = "test-project"
version = "0.1.0"
source = { editable = "." }
```

And imports work fine.

---

_Comment by @charliermarsh on 2025-09-14 13:29_

Yeah @nick-voisin has it right.

---

_Comment by @gilgamesh1111 on 2025-09-14 15:52_

thanks for  your help. My problem has been solved.

> Hi,
> 
> I think what you want is to do `uv init --lib my_project`. My uv.lock looks like the following for `test-project`:
> 
> ```
> `version = 1
> revision = 3
> requires-python = ">=3.12"
> 
> [[package]]
> name = "test-project"
> version = "0.1.0"
> source = { editable = "." }
> ```
> 
> And imports work fine.



---

_Closed by @konstin on 2025-09-14 16:15_

---

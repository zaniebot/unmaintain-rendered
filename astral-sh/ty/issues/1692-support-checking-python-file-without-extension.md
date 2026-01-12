```yaml
number: 1692
title: Support checking python file without extension
type: issue
state: closed
author: bulletmark
labels:
  - cli
assignees: []
created_at: 2025-11-30T23:53:25Z
updated_at: 2025-12-10T16:47:43Z
url: https://github.com/astral-sh/ty/issues/1692
synced_at: 2026-01-12T15:54:25Z
```

# Support checking python file without extension

---

_@bulletmark_

### Summary

`ty` will not check a file unless it has a python extension, e.g:

```sh
$ ty check -v myscript 
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.13, platform: linux
INFO Indexed 0 file(s) in 0.003s
WARN No python files found under the given path(s)
All checks passed!
```

Whereas:

```sh
$ ruff check -v myscript 
[2025-12-01][09:48:38][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/mark/Data/comps/aoc/pyproject.toml
[2025-12-01][09:48:38][ignore::gitignore][DEBUG] opened gitignore file: /home/mark/.config/git/ignore
[2025-12-01][09:48:38][ruff::commands::check][DEBUG] Identified files to lint in: 3.846741ms
[2025-12-01][09:48:38][ruff::diagnostics][DEBUG] Checking: /home/mark/Data/comps/aoc/2024/myscript
[2025-12-01][09:48:38][ruff::commands::check][DEBUG] Checked 1 files in: 785.466µs
All checks passed!
```

### Version

ty 0.0.1-alpha.29

---

_Label `cli` added by @MichaReiser on 2025-12-01 07:17_

---

_Label `wish` added by @MichaReiser on 2025-12-01 07:17_

---

_Comment by @sharkdp on 2025-12-01 08:19_

I also wanted this feature several times. It would also give us the option to type-check code that's being produced by another process, even without adding a `-` special argument:
```bash
ty check <(echo "reveal_type(1)")
```

---

_Label `wish` removed by @MichaReiser on 2025-12-01 09:14_

---

_Added to milestone `Beta` by @MichaReiser on 2025-12-01 09:14_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-12-01 09:14_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-01 09:14_

---

_Comment by @Gankra on 2025-12-01 14:22_

Micha introduced `System::source_type` to handle a similar problem for notebooks. Sounds like we should be using it in more places.

---

_Comment by @refi64 on 2025-12-08 23:04_

It's worth noting that this *also affects the LSP*. e.g. if I have this in both `x` and `x.py`:

```python
#!/usr/bin/env python3
print("s" + 1)
```

then opening `x` shows no errors, but `x.py` works as expected:

<img width="1718" height="528" alt="Image" src="https://github.com/user-attachments/assets/d65ee2ce-5fff-4861-94f6-3f237777c2e8" />

That being said, operations like hover and autocomplete work in *both* files; it's only the checks that don't work.

---

_Closed by @MichaReiser on 2025-12-10 16:47_

---

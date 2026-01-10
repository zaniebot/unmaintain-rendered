```yaml
number: 13751
title: Every list element is on one line
type: issue
state: closed
author: xJrJackBlack
labels:
  - question
assignees: []
created_at: 2024-10-14T13:52:45Z
updated_at: 2024-10-14T14:05:17Z
url: https://github.com/astral-sh/ruff/issues/13751
synced_at: 2026-01-10T11:09:55Z
```

# Every list element is on one line

---

_Issue opened by @xJrJackBlack on 2024-10-14 13:52_

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

Hello

```
$ ruff --version
ruff 0.6.6
```

I have this list: 

```py
data = [
    0x9B, 0x02, 0x20, 0x02, 0x80, 0x00, 0x04, 0x80, 0x00, 0x01, 
    0x80, 0x00, 0x01, 0x02, 0x04, 0x10, 0x01, 0x04, 0x4B, 0x02, 
    0x40, 0x40, 0x80, 0x00, 0x45, 0x02, 0x40, 0x40, 0x80, 0x00
    ]
```

But ruff formatter makes it like this:

```py
data = [
    0x9B,
    0x02,
    0x20,
    0x02,
    0x80,
    0x00,
    0x04,
    0x80,
    0x00,
    0x01,
    0x80,
    0x00,
    0x01,
    0x02,
    0x04,
    0x10,
    0x01,
    0x04,
    0x4B,
    0x02,
    0x40,
    0x40,
    0x80,
    0x00,
    0x45,
    0x02,
    0x40,
    0x40,
    0x80,
    0x00,
]
```

**What I have done:**

* In `settings.json` file I have added this: `"ruff.lint.ignore": ["COM812"],` but it does not worked for me. I have tried this both on linux and windows.
* on windows under `C:\Users\buser\AppData\Local\Programs\Python\Python311-arm64\Lib\test` I have found `.ruff.toml` file. I have added this line (at the end) `ignore = ["COM812"]`; also this does not work!
* I have created `.ruff.toml` file in my linux machine's home directory and add those lines and nothing changes:
```
[lint]
ignore = ["COM812"]

[format]
# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = true
```

I have longer lists! like 1024 bytes. And it is very very annoying when all elements are on one line separately!

What is the solution? How can I make formatter not to put single element on a newline? Especially; can I fix this just by editing `settings.json`?

P.S. folks sorry for my bad English; not my mother tongue.

---

_Comment by @MichaReiser on 2024-10-14 13:55_

Hi @xJrJackBlack 

No, you can't change the formatting with a setting. But you can use `fmt: off` or `fmt: skip` for the few statements where you don't want this specific formatting.

Improving formatting for very long lists is something we want to tackle eventually. 

```python
data = [
    0x9B, 0x02, 0x20, 0x02, 0x80, 0x00, 0x04, 0x80, 0x00, 0x01, 
    0x80, 0x00, 0x01, 0x02, 0x04, 0x10, 0x01, 0x04, 0x4B, 0x02, 
    0x40, 0x40, 0x80, 0x00, 0x45, 0x02, 0x40, 0x40, 0x80, 0x00
] # fmt: skip
```

However, note that this will disable all formatting for that statement

---

_Label `question` added by @MichaReiser on 2024-10-14 13:55_

---

_Comment by @xJrJackBlack on 2024-10-14 14:03_

Great thanks for your immediate reply `# fmt: skip` works without any additional setting for me! 

I hope that feature will come sooner 😊 

close #13751

---

_Closed by @MichaReiser on 2024-10-14 14:05_

---

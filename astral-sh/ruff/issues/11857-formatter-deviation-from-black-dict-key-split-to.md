```yaml
number: 11857
title: "Formatter: deviation from black: dict key split to multiline"
type: issue
state: open
author: minusf
labels:
  - bug
  - breaking
  - formatter
assignees: []
created_at: 2024-06-13T13:00:51Z
updated_at: 2024-06-19T21:23:08Z
url: https://github.com/astral-sh/ruff/issues/11857
synced_at: 2026-01-12T15:54:51Z
```

# Formatter: deviation from black: dict key split to multiline

---

_@minusf_

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
ruff 0.4.8 python 3.10.14 macos 14.5

```
[tool.ruff]
line-length = 100
```

input:
```python
        self.fields["redirect_link"].help_text = "Please do not use Tiny URLs for links to other www.xxxx.xxx pages."
```

black==24.4.2:

```python
        self.fields["redirect_link"].help_text = (
            "Please do not use Tiny URLs for links to other www.xxxx.xxx pages."
        )
```

ruff==0.4.8

```python
        self.fields[
            "redirect_link"
        ].help_text = "Please do not use Tiny URLs for links to other www.xxx.xxx pages."
```




---

_Comment by @minusf on 2024-06-13 13:03_

I couldn't find this one on the list of known deviations, and this was quite disliked with black IIRC.

---

_Label `formatter` added by @MichaReiser on 2024-06-13 13:13_

---

_Label `formatter` added by @MichaReiser on 2024-06-13 13:13_

---

_Comment by @minusf on 2024-06-13 13:15_

also, `ruff` converted the `black` output (as well as the `input`) to the multiline version

---

_Comment by @MichaReiser on 2024-06-14 14:39_

I can confirm that Ruff formats this differently than Black. 

Black's behavior is more consistent here in that it always tries to break the right first, before breaking the left. Ruff tries to do the same but I think it doesn't because of `BestFitParanthesize`'s semantic. 

Ruff uses `BestFitParenthesize` for the right side, and the left uses regular groups. The default behavior of the `Printer` is to prefer breaking right over left, by only testing if what comes after a group contains a line break before exceeding the line width. The problem is, there is no line break according to our current implementation. There's an inline comment that I wrote... but I'm not sure I understand what I meant at that time. 

https://github.com/astral-sh/ruff/blob/5806bc915d9a6cd30ce78644b84ff884f26cf841/crates/ruff_formatter/src/printer/mod.rs#L1289-L1291

Unfortunately, my hope that this would be fixed by https://github.com/astral-sh/ruff/pull/8940 is unjustified. This only changes another small difference.

I'll need to take a closer look at this. Ultimately, I think we can only release this in a new minor version because it will break existing formatting.



---

_Label `bug` added by @MichaReiser on 2024-06-14 14:39_

---

_Comment by @minusf on 2024-06-19 20:10_

thank you for looking into this.  maybe it's worth adding it to the list of known deviations?  it is a quite common occurance...

---

_Label `breaking` added by @zanieb on 2024-06-19 21:23_

---

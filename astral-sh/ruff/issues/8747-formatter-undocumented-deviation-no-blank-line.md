```yaml
number: 8747
title: "Formatter undocumented deviation: No blank line added between decorated inner classes in pyi files"
type: issue
state: open
author: henribru
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-17T22:06:02Z
updated_at: 2024-02-23T21:23:07Z
url: https://github.com/astral-sh/ruff/issues/8747
synced_at: 2026-01-10T11:09:51Z
```

# Formatter undocumented deviation: No blank line added between decorated inner classes in pyi files

---

_Issue opened by @henribru on 2023-11-17 22:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
class Foo:
    @foo
    class Bar: ...
    @foo
    class Baz: ...
```
With default settings (no settings in `pyproject.toml`) `ruff format .` leaves this code unchanged. `black .` adds an empty line between the two inner classes. Reproduced with Ruff 0.1.6 and Black 23.11.0.

Unfortunately I can't give you a playground reproduction for Ruff since it assumes files are `.py`, but here's one for Black as its playground supports it: https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACqAGFdAD2IimZxl1N_WlXnON2nzNX9HFvHx6cBLwL3Qwwh1eRAPLFFFWljgvmpOKOffkrecPNUT9AKbB5TWcHByQZl7G4uT9jIT7lav7JhqiztyvUmJIr2-kI3P8tLXaxrmogstgAAAAAANPCqLcjD5SoAAX2rAQAAACADosuxxGf7AgAAAAAEWVo=

---

_Comment by @charliermarsh on 2023-11-17 22:08_

Interesting! They _don't_ add a newline between decorated _functions_, but _do_ add a newline between decorated _classes_. Thanks for reporting.

---

_Label `bug` added by @charliermarsh on 2023-11-17 22:08_

---

_Label `formatter` added by @charliermarsh on 2023-11-17 22:08_

---

_Comment by @charliermarsh on 2023-11-17 22:14_

I'm wondering if this is intentional or a bug in Black.

---

_Comment by @charliermarsh on 2023-11-17 22:19_

Mostly because it's not that consistent between functions and classes: https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFyAIFdAD2IimZxl1N_WlXnON2nzNX9HFvHx6cBLwL3Qwwh1eRAPLFFFWljgvmpPX9pLqTnCMqdh3N3EyMEBEzXO-2vBynVUJMtmULtassHbhRrn1oJcaBh0ocW84sbYX4q7g1ul3plgihpMni7ANfRx9SPRrj7QHiDkG4Yihi-PnL71L5GAAAAAACFb5eQO-sQawABnQHzAgAA15WayLHEZ_sCAAAAAARZWg==

---

_Label `needs-decision` added by @MichaReiser on 2023-11-27 10:19_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-27 10:19_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-13 11:39_

---

_Label `needs-decision` removed by @MichaReiser on 2024-02-23 21:23_

---

_Label `bug` removed by @MichaReiser on 2024-02-23 21:23_

---

_Label `style` added by @MichaReiser on 2024-02-23 21:23_

---

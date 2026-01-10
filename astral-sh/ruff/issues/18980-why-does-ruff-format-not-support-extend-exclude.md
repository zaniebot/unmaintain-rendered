```yaml
number: 18980
title: "Why does `ruff format` not support `--extend-exclude`?"
type: issue
state: open
author: PatrickJordanNutanix
labels:
  - cli
  - formatter
assignees: []
created_at: 2025-06-27T09:58:22Z
updated_at: 2025-07-07T08:53:48Z
url: https://github.com/astral-sh/ruff/issues/18980
synced_at: 2026-01-10T11:09:59Z
```

# Why does `ruff format` not support `--extend-exclude`?

---

_Issue opened by @PatrickJordanNutanix on 2025-06-27 09:58_

### Question

I am managing a code base where there are multiple versions of Python being used. The majority of the code base uses Python 3.12 but a handful of files use Python 3.9. My `Makefile` contains two invocations of `ruff check` and `ruff format`, where the `--target-version` changes depending on the files being validated.

Ideally, my `Makefile` would look something like this:

```Makefile
# These files are executed in a Python 3.9 environment, so should be linted
# and formatted separately from the rest of the codebase.
PYTHON_3_9_FILES := \
    {file_1} \
    {file_2}

.PHONY: check
check:
    ruff format --diff --target-version py312 --extend-exclude $(PYTHON_3_9_FILES)
    ruff format --diff --target-version py39 $(PYTHON_3_9_FILES)

    ruff check --target-version py312 --extend-exclude $(PYTHON_3_9_FILES)
    ruff check --target-version py39 $(PYTHON_3_9_FILES)

.PHONY: fix
fix:
    ruff format --target-version py312 --extend-exclude $(PYTHON_3_9_FILES)
    ruff format --target-version py39 $(PYTHON_3_9_FILES)

    ruff check  --target-version py312 --fix --show-fixes --extend-exclude $(PYTHON_3_9_FILES)
    ruff check  --target-version py39 --fix --show-fixes $(PYTHON_3_9_FILES)
```

This then establishes a pattern for being able to execute `ruff` against different versions of Python (should there be a need in the future).

However, it appears that `ruff format` does not support the `--extend-exclude` option:

```shell
ruff format --extend-exclude hello.py
error: unexpected argument '--extend-exclude' found

  tip: to pass '--extend-exclude' as a value, use '-- --extend-exclude'
```

It does, however, respect the same setting in the `ruff.toml` file (at least local testing shows it stops checking/formatting those files with that option present). This means I have to duplicate the `PYTHON_3_9_FILES` in the `ruff.toml` file (to add them to the `extend-exclude` option there) and to the `Makefile` (so they can be passed explicitly to the Python 3.9 checks).

Is there a reason for the discrepancy between `check` and `format` for `--extend-exclude`?

### Version

0.9.0

---

_Label `question` added by @PatrickJordanNutanix on 2025-06-27 09:58_

---

_Comment by @ntBre on 2025-06-27 16:05_

Hmm, the discrepancy in the CLI could still be a bug, but would the [per-file-target-version](https://docs.astral.sh/ruff/settings/#per-file-target-version) setting help with this use case too?

---

_Comment by @PatrickJordanNutanix on 2025-06-30 07:46_

Thanks @ntBre for the heads up on that option. I wasn't aware of it. It certainly helps in this situation, so I've updated `ruff` to `0.12.1` in our project and replaced the double invocation in the `Makefile`. Thanks!

Regarding the bug with the CLI, what are your plans to track it? Will you keep this issue open? I'm happy to close this given your solution if you'd rather track it elsewhere.

---

_Comment by @ntBre on 2025-06-30 12:48_

Awesome, glad that helped!

We can leave this issue open if you want, the title is pretty much exactly what I'd want to write in a new one :) But if you'd rather close it to avoid future notifications, that's totally fine too!

---

_Comment by @PatrickJordanNutanix on 2025-06-30 15:14_

More than happy to keep this open and receive notifications 👍 

---

_Label `question` removed by @MichaReiser on 2025-07-07 08:53_

---

_Label `cli` added by @MichaReiser on 2025-07-07 08:53_

---

_Label `formatter` added by @MichaReiser on 2025-07-07 08:53_

---

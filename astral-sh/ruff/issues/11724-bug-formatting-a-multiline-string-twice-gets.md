```yaml
number: 11724
title: "Bug: formatting a multiline string twice gets changing results"
type: issue
state: closed
author: jasikpark
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-06-03T15:26:07Z
updated_at: 2024-06-05T15:55:15Z
url: https://github.com/astral-sh/ruff/issues/11724
synced_at: 2026-01-12T15:54:51Z
```

# Bug: formatting a multiline string twice gets changing results

---

_@jasikpark_

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


This was found via the `fuzz/ruff_formatter_idempotency.rs` fuzz target; It should be reproducible with standard ruff format.

Ruff was built from source around (https://github.com/astral-sh/ruff/commit/2b28889ca9d7935488875b5c944a159a2db20a23)

Original code: 

```

'''
\u{15}

'''
```

Formatted once:

```
"""
u{15}
"""

```

Formatted twice:

```
"""
u{15}"""

```

Original fuzz crash error:

```
thread '<unnamed>' panicked at fuzz_targets/ruff_formatter_idempotency.rs:31:17:

Reformatting the code a second time resulted in formatting changes.
Input: "\n'''\n\u{15}\n\n'''"
First: "\"\"\"\n\u{15}\n\"\"\"\n"

Second: "\"\"\"\n\u{15}\"\"\"\n"
diff:
--- Formatted Once
+++ Formatted Twice
@@ -1,3 +1,2 @@
 """
-
-"""
+"""
```


---

_Label `bug` added by @charliermarsh on 2024-06-03 15:26_

---

_Label `formatter` added by @charliermarsh on 2024-06-03 15:26_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-03 15:29_

---

_Comment by @MichaReiser on 2024-06-04 06:28_

Hmm, I'm having a hard time reproducing this issue. The  original code mentioned in this issue isn't valid syntax and can't be formatted (not a valid unicode escape sequence) ([playground](https://play.ruff.rs/0e161383-1617-473c-86ac-fe2453019e77)). 

I then tried to replace `\u{15}` with the corresponding ascii character, but that gets formatted correctly and in a stable way https://play.ruff.rs/3199caf0-e3ae-4d36-ac18-da00822c6af7

---

_Comment by @jasikpark on 2024-06-04 14:51_

True original code:

```

'''


'''
```

Formatted once

<img width="583" alt="Screenshot 2024-06-04 at 9 50 31 AM" src="https://github.com/astral-sh/ruff/assets/10626596/45ffcc08-1556-4ac6-8a1d-1f929e1f67d6">

Formatted twice

<img width="579" alt="image" src="https://github.com/astral-sh/ruff/assets/10626596/17e9a42f-8f27-4e2a-87a8-8c30f607fc2e">

---

_Comment by @MichaReiser on 2024-06-04 16:11_

Hmm, I'm still struggling. Would you mind clicking on the Share button in the playground (of the original code) and share the link here?

---

_Comment by @jasikpark on 2024-06-04 18:45_

Original: https://play.ruff.rs/797a9277-6c6f-4c35-bc88-fa0d5da9d950

First format: https://play.ruff.rs/53a83f3e-a05e-4366-99f4-7de25b880266

Second format: https://play.ruff.rs/066f3b1c-4130-4590-9576-e025b147c689

---

_Closed by @MichaReiser on 2024-06-05 15:55_

---

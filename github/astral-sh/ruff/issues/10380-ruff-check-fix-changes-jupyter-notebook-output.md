---
number: 10380
title: ruff check --fix changes Jupyter notebook output data
type: issue
state: open
author: dionhaefner
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-03-13T10:39:18Z
updated_at: 2024-03-21T02:38:13Z
url: https://github.com/astral-sh/ruff/issues/10380
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff check --fix changes Jupyter notebook output data

---

_Issue opened by @dionhaefner on 2024-03-13 10:39_

Notebook to reproduce:

[ruff-bug.ipynb.json](https://github.com/astral-sh/ruff/files/14586302/ruff-bug.ipynb.json)

Script demonstrating the bug:

```bash
$ cp ruff-bug.ipynb ruff-bug-reformatted.ipynb
$ ruff check --isolated --fix ruff-bug-reformatted.ipynb
$ diff ruff-bug.ipynb ruff-bug-reformatted.ipynb
```

Output:

```diff
Found 1 error (1 fixed, 0 remaining).
9,10c9
<     "# unusued import, to give ruff something to fix\n",
<     "import os"
---
>     "# unusued import, to give ruff something to fix"
72c71
<           0.022936753928661346,
---
>           0.022936753928661343,
98c97
<           0.16423597931861877
---
>           0.16423597931861875
147c146
<           0.022936753928661346,
---
>           0.022936753928661343,
173c172
<           0.16423597931861877
---
>           0.16423597931861875
223c222
<           0.022936753928661346,
---
>           0.022936753928661343,
249c248
<           0.16423597931861877
---
>           0.16423597931861875
```

All fixes except the unused import are applied to cell *output* which should never be reformatted.

Tested with ruff 0.3.2.

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


---

_Comment by @dhruvmanila on 2024-03-13 11:31_

Thanks for the report. Ruff does't look at the `outputs` field, it only updates the `source` field which is where the source code is which makes this bug a bit weird. It could be that the deserialization is causing this change. I'll need to look into it. 


---

_Label `bug` added by @dhruvmanila on 2024-03-13 11:31_

---

_Label `notebook` added by @dhruvmanila on 2024-03-13 11:31_

---

_Comment by @dhruvmanila on 2024-03-13 11:38_

Yeah, it's while deserializing the JSON string when the number is being updated. It can be reproduced using the given notebook with the following patch:

```diff
diff --git a/crates/ruff_notebook/src/notebook.rs b/crates/ruff_notebook/src/notebook.rs
index fef73cd85..1bdc9209d 100644
--- a/crates/ruff_notebook/src/notebook.rs
+++ b/crates/ruff_notebook/src/notebook.rs
@@ -186,7 +186,7 @@ impl Notebook {
         }
 
         Ok(Self {
-            raw: raw_notebook,
+            raw: dbg!(raw_notebook),
             index: OnceCell::new(),
             // The additional newline at the end is to maintain consistency for
             // all cells. These newlines will be removed before updating the
```

And running:
```console
cargo dev round-trip /path/to/notebook.ipynb
```

It's the same two numbers which are getting updated:
1. `0.022936753928661346` to `0.022936753928661343`
2. `0.16423597931861877` to `0.16423597931861875`

---

_Comment by @apaleyes on 2024-03-18 11:41_

Hi @dhruvmanila , and thanks for looking into this. Is this something that can/will be fixed on the Ruff side? Maybe there is some workaround to prevent this behaviour from happening while the fix hasn't arrived?

---

_Comment by @dhruvmanila on 2024-03-21 02:38_

I'm not sure. I need to look at it in a bit more detail. I plan to do so this week.

---

_Referenced in [astral-sh/ruff#18160](../../astral-sh/ruff/issues/18160.md) on 2025-05-18 12:40_

---

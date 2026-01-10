---
number: 9029
title: Formatter --preview add trailing comma
type: issue
state: open
author: hoxbro
labels:
  - formatter
  - preview
  - style
assignees: []
created_at: 2023-12-06T20:53:05Z
updated_at: 2024-02-14T16:30:07Z
url: https://github.com/astral-sh/ruff/issues/9029
synced_at: 2026-01-10T01:22:48Z
---

# Formatter --preview add trailing comma

---

_Issue opened by @hoxbro on 2023-12-06 20:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have the following code:
``` python
self._edits.append(
    {"operation": "update", "id": index, "fields": list(fields), "region_fields": []}
)
```

Which I run with `ruff format --preview` and gets 

``` python
self._edits.append({
    "operation": "update",
    "id": index,
    "fields": list(fields),
    "region_fields": [],
})
```

I would expect it to behave like this based on the new rule `hug_parens_with_braces_and_square_brackets`

``` python
self._edits.append({
    "operation": "update", "id": index, "fields": list(fields), "region_fields": []
})
```

Playground example: https://play.ruff.rs/6d3bf559-edf0-47f9-b1fd-1fc3418d7a46

---

_Comment by @charliermarsh on 2023-12-06 20:53_

Black seems to leave this code unmodified in preview.

---

_Label `bug` added by @charliermarsh on 2023-12-06 20:53_

---

_Label `formatter` added by @charliermarsh on 2023-12-06 20:53_

---

_Comment by @MichaReiser on 2023-12-07 02:03_

> Black seems to leave this code unmodified in preview.

Not entirely. Black hugs the parentheses, but formats all entries on a single line

```python
self._edits.append({
    "operation": "update", "id": index, "fields": list(fields), "region_fields": []
})
```

The fix here should be simple, similar to what we do in `FormatArgs`: Wrap the content (the entries) in a group. This gives us a two-staged formatting: Try to fit the entire dictionary on a line, try to fit the entries on a single line but break the `{`, `}`, and last, split each entry on a single line

```dif
--- a/crates/ruff_python_formatter/src/expression/expr_dict.rs	(revision Staged)
+++ b/crates/ruff_python_formatter/src/expression/expr_dict.rs	(date 1701914528224)
@@ -60,7 +60,7 @@
             joiner.finish()
         });
 
-        parenthesized("{", &format_pairs, "}")
+        parenthesized("{", &group(&format_pairs), "}")
             .with_dangling_comments(open_parenthesis_comments)
             .fmt(f)
     }
```


---

_Comment by @charliermarsh on 2023-12-07 02:05_

I don't see that behavior in the playground (https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADbAJNdAD2IimZxl1OAqkg9-7Mdp6WkAR2uY3jqi33CoVxaYgt_dqJQeRWYh2AUUVPYgfmUe5ec4ZcHkdVoq5sOpB1ReRTlQOrJGhEZF9IQqwqtTN2ew7VbGZJ7Tok5yApL_OQ8pjc2rk02ensMLtudWiG0va-XmA-ONGFqx6jYSP7KVMP-bLXSdrnjVEvSF7jcGNtsCCv5gAAASCWI0OPsLj4AAa8B3AEAAEAyP3uxxGf7AgAAAAAEWVo=):

<img width="1792" alt="Screen Shot 2023-12-06 at 9 06 03 PM" src="https://github.com/astral-sh/ruff/assets/1309177/aff1ce50-9cbb-4561-ab8f-a3c22218c1c8">

Maybe I'm doing something wrong.


---

_Comment by @MichaReiser on 2023-12-07 02:07_

It seems they changed this between Stable and Main. Stable gives you the formatting I shared. Main leaves it as is. I don't like the change. Makes it rather magic and doesn't save on vertical space.

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-12-22 06:16_

---

_Comment by @MichaReiser on 2024-02-12 11:22_

Note: The pattern where the "huggable" fully fits on its own line seems somewhat common in Airflow's codebase https://github.com/apache/airflow/pull/37355

---

_Referenced in [apache/airflow#37355](../../apache/airflow/pulls/37355.md) on 2024-02-12 12:13_

---

_Comment by @MichaReiser on 2024-02-13 12:43_

Related black issue https://github.com/psf/black/issues/4099

There's the option to format this as:

```python
self._edits.append(
    {"operation": "update", "id": index, "fields": list(fields), "region_fields": []}
)
```

or 

```python
self._edits.append({
    "operation": "update", "id": index, "fields": list(fields), "region_fields": []
})
```

The former probably requires the use of `best_fitting![expression.format(), block_indent(&expression.format()), expression.format()]` where it should be sufficient to `group` the `pairs` for the second one. 

---

_Comment by @MichaReiser on 2024-02-14 16:29_

I remove this from the stable formatter milestone because we decided not to ship the hugging preview style

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-14 16:29_

---

_Label `preview` added by @MichaReiser on 2024-02-14 16:29_

---

_Label `bug` removed by @MichaReiser on 2024-02-14 16:30_

---

_Label `style` added by @MichaReiser on 2024-02-14 16:30_

---

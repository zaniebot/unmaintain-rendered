---
number: 14445
title: Fixes on Jupyter notebooks cause panics when fix replaces/deletes multiple cells
type: issue
state: open
author: beskep
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-11-19T00:45:11Z
updated_at: 2024-12-03T04:52:21Z
url: https://github.com/astral-sh/ruff/issues/14445
synced_at: 2026-01-10T01:22:55Z
---

# Fixes on Jupyter notebooks cause panics when fix replaces/deletes multiple cells

---

_Issue opened by @beskep on 2024-11-19 00:45_

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

- command: `ruff check .\src\test.ipynb --fix --diff`
(No panic when `--isolated` included)

- ruff version: 0.7.4

- pyproject.toml
```toml
[project]
requires-python = ">=3.13"
dependencies = ["ruff>=0.7.4"]

[tool.ruff]
preview = true

[tool.ruff.lint]
select = ["W"]
```


- ipynb file: [test.ipynb.zip](https://github.com/user-attachments/files/17807695/test.ipynb.zip)


---

_Comment by @dylwil3 on 2024-11-19 02:05_

Thanks so much for this!

This can be reproduced with a notebook that just consists of three empty cells. We are somehow counting empty cells as newlines and then erroring when we try to "delete" them.

---

_Label `bug` added by @dylwil3 on 2024-11-19 02:05_

---

_Label `notebook` added by @dylwil3 on 2024-11-19 02:05_

---

_Comment by @dylwil3 on 2024-11-19 02:20_

Actually this bug has a wider blast radius.

If you make a notebook with three cells like this:

```python
a = [1]
a.append(2)
# new cell
a.append(3)
# new cell
a.append(4)
```
and apply the unsafe fix for `FURB118` you also get a panic.

---

_Renamed from "[Linter panic] when fixing W391 in Jupyter notebook" to "Fixes on Jupyter notebooks cause panics when fix replaces/deletes multiple cells" by @dylwil3 on 2024-11-19 02:21_

---

_Comment by @dhruvmanila on 2024-11-19 03:12_

I'm guessing that cell deletion is creating a problem when `update_cell_offsets` and `update_cell_contents` are being called.

---

_Comment by @dhruvmanila on 2024-11-19 09:37_

The root cause is because the edit spans across the cell boundary which is an invariant we like to follow. In the case of `FURB113`, the diagnostics is between 3 cells and thus the edit is as well.

Refer to how the cell offsets are being updated (incorrectly):
```rs
// Original cell offsets
[crates/ruff_notebook/src/notebook.rs:229:9] &self.cell_offsets = CellOffsets(
    [
        0,
        20,
        32,
        44,
    ],
)

// The edit
[crates/ruff_linter/src/fix/mod.rs:97:13] edit = Edit {
    range: 8..43,
    content: Some(
        "x.extend((2, 3, 4))",
    ),
}

// Source map created for the edit
[crates/ruff_notebook/src/notebook.rs:230:9] source_map = SourceMap(
    [
        SourceMarker {
            source: 8,
            dest: 8,
        },
        SourceMarker {
            source: 43,
            dest: 27,
        },
    ]
)

// After the above source map is applied to update the cell offsets
[crates/ruff_notebook/src/notebook.rs:263:9] &self.cell_offsets = CellOffsets(
    [
        0,
        20,
        32,
        28,
    ],
)
```

I don't this rule existed when I implemented this.

---

_Comment by @dhruvmanila on 2024-11-19 09:48_

So, I think we need to consider replacement and deletion across cell boundaries. Insertion is fine because it's at an exact position which isn't going to affect this logic. There's some details on how the source markers are being used to update the cell offsets in the PR description: https://github.com/astral-sh/ruff/pull/4665.

---

_Comment by @dylwil3 on 2024-11-19 15:13_

Oh that's neat- and nice drawing! For deletions we could just delete the cell and adjust the cell offsets (so there will be fewer cell offsets than there used to be). What do you think?

For replacements it's a little less clear what to do, since folks separate things across cells in order to control what executes when. 

---

_Comment by @dhruvmanila on 2024-11-20 05:15_

> For deletions we could just delete the cell and adjust the cell offsets (so there will be fewer cell offsets than there used to be). What do you think?

The thing is we don't really know the kind of edit when trying to update the cell offsets. The main reason for adding the `SourceMap` abstraction is to avoid passing this information and it would also help in adding multi-language support in Ruff. So, in `update_cell_offsets` you only have access to the list of `SourceMarker` and we don't really know which two source markers makes up an edit.

I think we'll need a generic solution which just looks at source markers to determine that certain cell offsets needs to be removed. I'm not exactly sure what this looks like.

---

_Comment by @dylwil3 on 2024-12-03 04:52_

> The thing is we don't really know the kind of edit when trying to update the cell offsets.

Can you say a bit more about this? It doesn't seem like the information of edit-kind is lost. If I understand correctly, the construction of the `SourceMap` maintains the invariant that there are an even number of markers. Each edit kind is recoverable by examining the pairs of markers: adjacent markers pointing to the same destination is a delation, adjacent markers pointing to different destinations is a replacement or insertion (the latter when the markers have the same source). Could we iterate through the cell offsets and determine whether they are between adjacent source markers, and, if so,  just not propagate them to the target?

Is there a use for `SourceMap` instances that don't maintain this invariant of evenness? 

Finally - it's unclear to me how this relates to supporting other languages. Could you say more about that?

Apologies for my confusion!

---

_Referenced in [astral-sh/ruff#20036](../../astral-sh/ruff/pulls/20036.md) on 2025-08-22 05:02_

---

_Referenced in [astral-sh/ruff#20789](../../astral-sh/ruff/issues/20789.md) on 2025-10-10 19:38_

---

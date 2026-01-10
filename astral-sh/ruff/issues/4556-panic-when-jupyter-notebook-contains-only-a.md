```yaml
number: 4556
title: Panic when Jupyter notebook contains only a single empty code cell
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2023-05-21T02:24:11Z
updated_at: 2023-06-12T14:14:18Z
url: https://github.com/astral-sh/ruff/issues/4556
synced_at: 2026-01-10T11:09:47Z
```

# Panic when Jupyter notebook contains only a single empty code cell

---

_Issue opened by @dhruvmanila on 2023-05-21 02:24_

Jupyter notebook content (1 empty code cell):
```python

```

Command:

```console
$ cargo run --all-features --bin ruff -- check --select=ALL /path/to/notebook.ipynb
```

Output:

```
    Finished dev [unoptimized + debuginfo] target(s) in 0.37s
     Running `target/debug/ruff check --select=ALL /Users/dhruv/playground/python/ruff_test.ipynb`
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: Detected debug build without --no-cache.

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 1', crates/ruff/src/message/text.rs:73:32
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
/Users/dhruv/playground/python/ruff_test.ipynb:%                                                                                                                                                
```

---
* Ruff version: https://github.com/charliermarsh/ruff/commit/fc63c6f2e260760963c5fdea2d9d4e0cf38ec58a

---

_Label `bug` added by @MichaReiser on 2023-05-21 06:32_

---

_Comment by @sladyn98 on 2023-05-22 09:05_

@MichaReiser 
```
let after = checker
    .locator
    .slice(TextRange::new(docstring.end(), stmt.end()));
 ```
 
after would be an empty slice when the notebook contains only a single empty cell, thus `after.universal_newlines().skip(1) ` is trying to skip to an element that does not exist, hence the "index out of bounds" panic.

---

_Comment by @MichaReiser on 2023-05-22 09:17_

> @MichaReiser
> 
> ```
> let after = checker
>     .locator
>     .slice(TextRange::new(docstring.end(), stmt.end()));
> ```
> 
> after would be an empty slice when the notebook contains only a single empty cell, thus `after.universal_newlines().skip(1) ` is trying to skip to an element that does not exist, hence the "index out of bounds" panic.

Where did you find the `after.universal_newlines().skip(1)` call. Skip should not traverse passed the end. It skips a line if there's one, but does nothing if the iterator has already reached the end of the line.

My assumption is that the `jupyter_index.row_to_cell[start_location.row.get()]` is out of bounds because `jupyter_index.row_to_cell` is empty?

https://github.com/charliermarsh/ruff/blob/c6e5fed6587c95839a814ee8dfa845b465351229/crates/ruff/src/message/text.rs#L72-L78

---

_Comment by @dhruvmanila on 2023-05-22 15:52_

`jupyter_index.row_to_cell` is empty indeed but contains the default `[0]` which is just a placeholder as the index starts at 1. Now, there's a message to be displayed on the first line but the source is empty (not even a newline). This means there's no entry for the first line in the index which is creating this panic.

---

_Comment by @MichaReiser on 2023-05-22 16:09_

> `jupyter_index.row_to_cell` is empty indeed but contains the default `[0]` which is just a placeholder as the index starts at 1. Now, there's a message to be displayed on the first line but the source is empty (not even a newline). This means there's no entry for the first line in the index which is creating this panic.

Hmm, the `LineIndex` should never be empty. It always adds at least one entry for the start position `0`. 

https://github.com/charliermarsh/ruff/blob/9131a8e7f158329c362363fadcb97629f893a8a4/crates/ruff_python_ast/src/source_code/line_index.rs#L30

What's the easiest way for me to reproduce this bug?

---

_Comment by @dhruvmanila on 2023-05-23 00:25_

Oh by index I meant the Jupyter Index is empty, as in it only contains the placeholder (`jupyter_index.row_to_cell = vec![0]`). Now, `start_location.row.get()` returns 1 as it's one-indexed which results in a panic.

But, here's a notebook which will help in reproducing the bug (https://gist.github.com/dhruvmanila/2291dbeea59bc506186060617ef0ec4f) using the following command:

```console
cargo run --all-features --bin ruff -- check --no-cache --isolated --select=D100 /path/to/notebook.ipynb
```

---

_Comment by @MichaReiser on 2023-05-23 06:07_

Ah I see. Thanks. So this is an issue with the `JupyterIndex` for empty files.

---

_Closed by @dhruvmanila on 2023-06-12 14:14_

---

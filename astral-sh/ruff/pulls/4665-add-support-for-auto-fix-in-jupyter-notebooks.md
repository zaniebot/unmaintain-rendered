```yaml
number: 4665
title: Add support for auto-fix in Jupyter notebooks
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: feat/jupyter-notebook-autofix
created_at: 2023-05-26T08:33:00Z
updated_at: 2023-06-12T15:51:53Z
url: https://github.com/astral-sh/ruff/pull/4665
synced_at: 2026-01-12T03:43:29Z
```

# Add support for auto-fix in Jupyter notebooks

---

_Pull request opened by @dhruvmanila on 2023-05-26 08:33_

## Summary

Add initial support for applying auto-fixes in Jupyter Notebook.

### Solution

Cell offsets are the boundaries for each cell in the concatenated source code. They are represented using `TextSize`. It includes the start and end offset as well, thus creating a range for each cell. These offsets are updated using the `SourceMap` markers.

### SourceMap

`SourceMap` contains markers constructed from each edits which tracks the original source code position to the transformed positions. The following drawing might make it clear:

![image](https://github.com/astral-sh/ruff/assets/67177269/3c94e591-70a7-4b57-bd32-0baa91cc7858)

The center column where the dotted lines are present are the markers included in the `SourceMap`. The `Notebook` looks at these markers and updates the cell offsets after each linter loop. If you notice closely, the destination takes into account all of the markers before it.

The index is constructed only when required as it's only used to render the diagnostics. So, a `OnceCell` is used for this purpose. The cell offsets, cell content and the index will be updated after each iteration of linting in the mentioned order. The order is important here as the content is updated as per the new offsets and index is updated as per the new content.

## Limitations

### 1 (resolved https://github.com/charliermarsh/ruff/pull/4665/commits/d72192301c88f77c2709f0759a77b554c30f5eb3)

Auto-fixes which spans across multiple cell will *panic*. This is because it's difficult to determine which part of the edit content belongs to which cell. This mainly includes the import sorter in the following scenario where there are 2 continuous cells with import statements:
	
```python
import random
import os
# ---
import math
```

(The commented line break is just to visualize cell separation and is not part of the code.)

Here, the concatenated content would be:

```python
import random
import os
import math
```

And the formatted output would be:

```python
import math
import random
import os
```

Now, it's not possible to determine which part of the content belongs to which cell and even if we try to update it as per the line count the context will change as an import might move out from the actual cell.

#### Possible solutions:
* We could ignore the `Edit` which spans across multiple cells but this creates a problem in the lint loop. Take the above code as an example where there are 2 cells containing import statements which needs to be sorted. Now, this edit will span both cells, we'll ignore this edit, in the next iteration the same edit will be created and ignored and this will go on till we reach `MAX_ITERATION`.
* Scope rules to be either global or local to the cell -- should we apply this rule to the concatenated source code or individually to each cell?
* As this problem mainly occurs in the import sorter due to the fact that the `Edit` is scoped to an entire import block, we could refactor the sorter to create individual edits such as move this import statement from line 4 to line 2, remove this import statement, remove this symbol from this import statement, add a symbol to this import statement, etc. We would have to think about how to represent an edit like separating each symbol on it's own line (black style).

### 2

Styling rules such as the ones in `pycodestyle` will not be applicable everywhere in Jupyter notebook, especially at the cell boundaries. Let's take an example where a rule suggests to have 2 blank lines before a function and the cells contains the following code:

```python
import something
# ---
def first():
	pass

def second():
	pass
```

(Again, the comment is only to visualize cell boundaries.)

In the concatenated source code, the 2 blank lines will be added but it shouldn't actually be added when we look in terms of Jupyter notebook. It's as if the function `first` is at the start of a file.

`nbqa` solves this by recording newlines before and after running `autopep8`, then running the tool and restoring the newlines at the end (refer https://github.com/nbQA-dev/nbQA/pull/807).

## Test Plan

Three commands were run in order with common flags (`--select=ALL --no-cache --isolated`) to isolate which stage the problem is occurring:
1. Only diagnostics
2. Fix with diff (`--fix --diff`)
3. Fix (`--fix`)

### https://github.com/facebookresearch/segment-anything

```
-------------------------------------------------------------------------------
 Jupyter Notebooks       3            0            0            0            0
 |- Markdown             3           98            0           94            4
 |- Python               3          513          468            4           41
 (Total)                            611          468           98           45
-------------------------------------------------------------------------------
```

```console
$ cargo run --all-features --bin ruff -- check --no-cache --isolated --select=ALL /path/to/segment-anything/**/*.ipynb --fix
...
Found 180 errors (89 fixed, 91 remaining).
```

### https://github.com/openai/openai-cookbook

```
-------------------------------------------------------------------------------
 Jupyter Notebooks      65            0            0            0            0
 |- Markdown            64         3475           12         2507          956
 |- Python              65         9700         7362         1101         1237
 (Total)                          13175         7374         3608         2193
===============================================================================
```

```console
$ cargo run --all-features --bin ruff -- check --no-cache --isolated --select=ALL /path/to/openai-cookbook/**/*.ipynb --fix
error: Failed to parse /path/to/openai-cookbook/examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb:cell 4:29:18: unexpected token '-'
...
Found 4227 errors (2165 fixed, 2062 remaining).
```

### https://github.com/tensorflow/docs

```
-------------------------------------------------------------------------------
 Jupyter Notebooks     150            0            0            0            0
 |- Markdown             1           55            0           46            9
 |- Python               1          402          289           60           53
 (Total)                            457          289          106           62
-------------------------------------------------------------------------------
```

~These repository includes some notebooks where it panics because an edit spans across multiple cells. This is due to the import statements are in 2 or more continuous cells.~ (resolved https://github.com/charliermarsh/ruff/pull/4665/commits/d72192301c88f77c2709f0759a77b554c30f5eb3)

```console
$ cargo run --all-features --bin ruff -- check --no-cache --isolated --select=ALL /path/to/tensorflow-docs/**/*.ipynb --fix
error: Failed to parse /path/to/tensorflow-docs/site/en/guide/extension_type.ipynb:cell 80:1:1: unexpected token Indent
error: Failed to parse /path/to/tensorflow-docs/site/en/r1/tutorials/eager/custom_layers.ipynb:cell 20:1:1: unexpected token Indent
error: Failed to parse /path/to/tensorflow-docs/site/en/guide/data.ipynb:cell 175:5:14: unindent does not match any outer indentation level
error: Failed to parse /path/to/tensorflow-docs/site/en/r1/tutorials/representation/unicode.ipynb:cell 30:1:1: unexpected token Indent
...
Found 12726 errors (5140 fixed, 7586 remaining).
```

### https://github.com/tensorflow/models

```
-------------------------------------------------------------------------------
 Jupyter Notebooks      46            0            0            0            0
 |- Markdown             1           11            0            6            5
 |- Python               1          328          249           19           60
 (Total)                            339          249           25           65
-------------------------------------------------------------------------------
```

```console
$ cargo run --all-features --bin ruff -- check --no-cache --isolated --select=ALL /path/to/tensorflow-models/**/*.ipynb --fix
...
Found 4856 errors (2690 fixed, 2166 remaining).
```

## To-Do:

- [ ] ~Revert back the auto-fixes if the linter loop panics (as is done for Python files)~
- [x] Verify that the updated notebook is parseable by `nbformat` (only for testing purposes)
- [ ] ~Add roundtrip support (this will most likely be done with the second todo)~ (different PR)
- [x] Ignore cells containing magic commands (`%`, `!`, `?`) on any cells (this will ignore cells which contains code as well as magic commands). ~Just for backup, ignore any cells containing syntax error~
- [ ] ~Add `--diff` preview for Jupyter notebook~
- [x] Newline at the end of each cell is removed but then Ruff will complain about missing newline at the end of file. The `--fix` flag will try to add the newline, but we'll remove it before updating the cell and this loop will go on.

resolves: #1218
fixes: #4556 


---

_Converted to draft by @dhruvmanila on 2023-05-26 08:33_

---

_Comment by @github-actions[bot] on 2023-05-26 09:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      8.5±0.18ms     4.8 MB/sec    1.00      8.2±0.16ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.02  1758.8±53.46µs     9.5 MB/sec    1.00  1722.1±44.25µs     9.7 MB/sec
formatter/numpy/globals.py                 1.02    169.5±5.41µs    17.4 MB/sec    1.00    166.4±9.00µs    17.7 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.08ms     7.3 MB/sec    1.00      3.4±0.13ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.30ms     2.2 MB/sec    1.01     18.9±0.28ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.10ms     3.8 MB/sec    1.01      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   550.5±10.93µs     5.4 MB/sec    1.02   563.6±20.20µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.18ms     3.3 MB/sec    1.02      7.9±0.23ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.12ms     4.8 MB/sec    1.04      8.9±0.14ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1867.9±53.82µs     8.9 MB/sec    1.02  1908.7±37.67µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   218.4±12.47µs    13.5 MB/sec    1.01    219.8±6.26µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.09ms     6.4 MB/sec    1.02      4.1±0.14ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.0±0.05ms     5.1 MB/sec    1.00      7.9±0.06ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1627.4±14.17µs    10.2 MB/sec    1.00  1593.1±13.14µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    157.0±1.79µs    18.8 MB/sec    1.00    156.7±3.35µs    18.8 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.03ms     7.8 MB/sec    1.00      3.3±0.04ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.9±0.15ms     2.4 MB/sec    1.00     16.9±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.04ms     3.8 MB/sec    1.00      4.3±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    440.7±7.74µs     6.7 MB/sec    1.00    436.0±6.83µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.06ms     3.5 MB/sec    1.00      7.4±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.10ms     4.8 MB/sec    1.00      8.4±0.07ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1768.0±13.79µs     9.4 MB/sec    1.00  1747.5±12.47µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±2.89µs    15.5 MB/sec    1.00    189.7±5.45µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/jupyter/index.rs`:44 on 2023-05-26 15:31_

Could we do without the separate builder here?

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:78 on 2023-05-26 15:38_

would it make sense to move this to the index?

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:234 on 2023-05-26 15:43_

can't we just always add `new_text_size - current_text_size`?

---

_@konstin reviewed on 2023-05-26 15:48_

looks good so far, i'll give it a more nit-picky review once it's not a draft anymore

---

_@dhruvmanila reviewed on 2023-05-29 02:20_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:234 on 2023-05-29 02:20_

No, we can't otherwise it'll overflow as it's a `TextSize` which is internally a `u32`.

This is a replacement edit which means that the new text could be less in length as compared to the existing text.

---

_@dhruvmanila reviewed on 2023-05-29 02:23_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:78 on 2023-05-29 02:23_

The reason to keep the index separate is to have the ability to refresh _just_ the index for each iteration of auto-fix. The cell offsets cannot be updated just with the new cell content, we analyze each edit and update the offsets accordingly. This is done separately and is unrelated to the index.

---

_@dhruvmanila reviewed on 2023-05-29 02:26_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:44 on 2023-05-29 02:26_

The reason to have a builder is to have both the index and cell offsets built in a single loop with each individual cell.

Cell offsets are built from the source code only for the first time. They cannot be refreshed using the transformed source code but require to process each individual edit to update the offsets.

When a single auto-fix iteration ends, the index is refreshed from the transformed source code, source code is updated in each cell and cell offsets are updated from the edits (order is important).

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:205 on 2023-05-31 16:41_

This additional newline at the end is for consistency for all cells. These newlines will be removed before writing back to individual cells. Refer to `update_cell_contents`

---

_@dhruvmanila reviewed on 2023-05-31 16:55_

---

_@dhruvmanila reviewed on 2023-06-01 06:44_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:259 on 2023-06-01 06:44_

The reason to choose `String` over `StringArray` is that the latter should contain the newline as part of each element which means that `str.lines` would not work and we would either need a custom iterator with strings _including_ the newline character or add newlines to each element individually.


---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:59 on 2023-06-01 10:23_

This was a hard bug to find. Trailing newline need to be considered in the count as well. Take the following cell source value:

```python
"import os\n"
```

This will be 2 lines instead of 1.

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:79 on 2023-06-01 10:23_

Same as above, here we'll have to check the last vector element:

```python
[
	"import os\n",
	"import math\n"
]
```

This will be 3 lines instead of 2.

---

_@dhruvmanila reviewed on 2023-06-01 10:24_

---

_Marked ready for review by @dhruvmanila on 2023-06-01 10:44_

---

_Review requested from @konstin by @dhruvmanila on 2023-06-01 10:44_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 15:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:19 on 2023-06-02 06:45_

Nit: Rust's convention is to not use the `get` prefix for getters and only use the `set` prefix for setters
```suggestion
    pub fn cell(&self, row: usize) -> u32 {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:20 on 2023-06-02 06:46_

Should this return an `Option<u32>` instead to avoid out-of-bound errors?

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:13 on 2023-06-02 06:46_

Maybe: Create newtype wrappers for `Cell` and `Row` (or use `OneIndexed`?) Newtype wrappers can encode the information that `cell` returns a `Cell` and prevents users from mixing cell with rows.

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:45 on 2023-06-02 06:59_

Nit: The collect here is fairly expensive (more than I would expect from a `is_valid_code_cell`) method because it copies the whole contents of the cell. 

You can either move the `lines.iter().any` check into the match arms OR write your own iterator:

```
enum CellLinesIter<'a> {
	String(Lines<'a>),
	Array(std::slice::Iter<'a, &'a str>),
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:36 on 2023-06-02 06:59_

Nit: How about making this a method on `Cell` instead? 

```
cell.is_valid_code_cell()` 
```

feels more idiomatic to me

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:75 on 2023-06-02 07:00_

Nit: Would a `u32` suffice?

How is the cell number defined? 

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:63 on 2023-06-02 07:02_

What's the difference between a `JupyterNotebook` and a `Notebook`?

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:182 on 2023-06-02 07:03_

This is somewhat expensive because it requires copying over all code blocks. Would a "virtual" notebook work where we only store the indices of the code cells and than skip all non-code cells while iterating?

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:23 on 2023-06-02 07:06_

Would it instead be possible to return a `SourceMap` that maps source positions to target positions in the `FixResult`. I would prefer that because it has a more narrow API and allows better re-use if we have other mapped types (e.g. python code in markdown files)

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:8 on 2023-06-02 07:08_

Do we always need to build the index? It seems to be a very costly operation (iterates over the whole content). 

Do we need the index during fixing or is it only used when emitting diagnostics?

---

_@MichaReiser reviewed on 2023-06-02 07:08_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/index.rs`:14 on 2023-06-02 07:46_

What's the purpose of this being an `Arc`?

---

_Review comment by @konstin on `crates/ruff/src/jupyter/index.rs`:79 on 2023-06-02 07:49_

i'd make this a source code comment

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:48 on 2023-06-02 07:56_

it's fine for this PR, but i believe we do need to keep the python code or we'll see some undefined symbol false positives because they were defined in cell that starts with a `%` line

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:205 on 2023-06-02 07:59_

would also make this a code comment

---

_@konstin reviewed on 2023-06-02 08:04_

needs tests, otherwise it's looking good

---

_Comment by @charliermarsh on 2023-06-02 15:18_

My main suggestion for resolving (1), at least for import sorting (which, I think, is the only edit that spans multiple statements in this way), is to find a way to make the `BlockBuilder` aware of cell boundaries. The `BlockBuilder` already _kind of_ supports this behavior, because it has to support isort's split directives:

```py
import sys
import os
# isort: split
import abc
import collections
```

In this case, the first two imports will be treated as an import block, and the second two imports will be treated as an independent import block. Conceptually, this is identical (?) to the splits we want to enforce between cells. I'd suggest finding a way to feed those splits into the `BlockBuilder`!

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/mod.rs`:23 on 2023-06-02 15:19_

I would support this -- could you describe in a bit more detail how it would work?

---

_@charliermarsh reviewed on 2023-06-02 15:19_

---

_@dhruvmanila reviewed on 2023-06-03 16:19_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:20 on 2023-06-03 16:19_

So, these are only used when emitting the diagnostics. How do you think we should handle the `None` case?

If we use the default value, that would render the diagnostic at the top of file.

This is not similar but I think `source_location` panics as well if it's out of bounds.

---

_@dhruvmanila reviewed on 2023-06-03 16:20_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:13 on 2023-06-03 16:20_

Newtype wrappers makes sense but let me see if `OneIndexed` works better.

---

_@dhruvmanila reviewed on 2023-06-03 16:31_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:45 on 2023-06-03 16:31_

Correct me if I'm wrong, but that's a vector of string slice (`&str`), so is it not just collecting into a vector of view to the actual `String` and _not_ copying it? The only reason I'm collecting is to avoid code repetition, although one could argue against it :)

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:13 on 2023-06-03 16:36_

Actually, I'm not sure if Newtype wrappers would be correct as the cell and row numbers are generated internally and it's only returned via the public interface. It's used directly when emitting the diagnostic message and it won't have any context about these newtypes.

---

_@dhruvmanila reviewed on 2023-06-03 16:36_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:63 on 2023-06-03 16:41_

`JupyterNotebook` is the deserialized version of the JSON string while `Notebook` is the custom datatype holding the raw notebook (`JupyterNotebook`) and other related information.

---

_@dhruvmanila reviewed on 2023-06-03 16:41_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:75 on 2023-06-03 16:41_

Yes, it should 👍 

> How is the cell number defined?

Oh, actually it's an index value in a `Vec<Cell>` which are available on `JupyterNotebook`

---

_@dhruvmanila reviewed on 2023-06-03 16:41_

---

_@dhruvmanila reviewed on 2023-06-03 16:44_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:182 on 2023-06-03 16:44_

I believe that's what is happening here unless I'm mistaken with what you're referring to as "expensive"?

Here, we're collecting all the indices which are valid code cells, storing it on `Notebook` to be used wherever required.

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:8 on 2023-06-03 16:49_

Good point. The index is only required when emitting the diagnostics, I believe we could use `OnceCell` to only compute it when we're emitting them. Then, I think it would even eliminate the need to refresh the index.

---

_@dhruvmanila reviewed on 2023-06-03 16:49_

---

_@dhruvmanila reviewed on 2023-06-03 17:11_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:14 on 2023-06-03 17:11_

So, `Diagnostics` takes ownership of `JupyterIndex` once the linting is completed to emit the correct information for each diagnostic w.r.t a Jupyter notebook. Now, that the index belongs to `Notebook` struct and cloning would be expensive, `Arc` made sense to me. On the other hand, we could take a reference and use lifetimes on `Diagnostics`.

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:48 on 2023-06-03 17:14_

Yes, we'll have to roll out our own magic transformer :)

---

_@dhruvmanila reviewed on 2023-06-03 17:14_

---

_Comment by @dhruvmanila on 2023-06-03 17:17_

> i'd make this a source code comment

(There's no reply section for this comment, so putting it out here)

I'm not sure what do you mean by "source code comment". Do you want me to comment the reason for the `trailing_newline`?

> would also make this a code comment

Same as above.

> needs tests, otherwise it's looking good

Yes, I wanted to wait for the initial feedback incase there were some changes although I've already started writing some basic test cases.

---

_Comment by @zanieb on 2023-06-03 17:21_

> (There's no reply section for this comment, so putting it out here)

fyi GitHub does weird things with threaded conversations — the threads are up higher at https://github.com/charliermarsh/ruff/pull/4665#discussion_r1214042114 / https://github.com/charliermarsh/ruff/pull/4665#discussion_r1214052152

---

_@dhruvmanila reviewed on 2023-06-04 08:29_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/index.rs`:14 on 2023-06-04 08:29_

We can avoid using `Arc` altogether by storing the `SourceKind` on `Diagnostics` instead of `JupyterIndex`. But, this would mean that `SourceKind` (thus, `Notebook` and all of the schema types) need to implement the `PartialEq` trait which is only used for a test case.

---

_@MichaReiser reviewed on 2023-06-05 07:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:20 on 2023-06-05 07:17_

> This is not similar but I think source_location panics as well if it's out of bounds.

That's true and we should probably avoid that too... It's unfortunate if ruff dies just because it fails to render a single diagnostic (This isn't meant that YOU should change it, it should probably be me)

> So, these are only used when emitting the diagnostics. How do you think we should handle the None case?

I think that's' a reasonable fallback and provides better usability than taking down ruff. I would expect users to report a bug that we can then fix.

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/index.rs`:13 on 2023-06-05 07:18_

Not sure if I understand fully, but I trust your judgment. My main intention of introducing them would be to make it harder to mix up the two. 

---

_@MichaReiser reviewed on 2023-06-05 07:18_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:45 on 2023-06-05 07:19_

Right, I missed that. I think it could still make sense to move the `any` into the match arms to avoid the unnecessary allocation (and writing a vec entry for every cell).

---

_@MichaReiser reviewed on 2023-06-05 07:19_

---

_@MichaReiser reviewed on 2023-06-05 07:20_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:75 on 2023-06-05 07:20_

Would it be possible to use our `IndexVec` and `newtype_index` macro to make this relationship explicit? 

https://github.com/charliermarsh/ruff/blob/d1c7a07d2ae56f4ff56b81c7f2a398e2262c42b9/crates/ruff_python_semantic/src/scope.rs#L140-L163

`IndexVec` is a zero-cost wrapper around `Vec` that can be indexed by the struct defined with `newtype_index` (rather than any `usize`). 

---

_@MichaReiser reviewed on 2023-06-05 07:23_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:63 on 2023-06-05 07:23_

Could we add some documentation explaining this. Or maybe rename the notebooks: `SerializedNotebook` or `RawNotebook` for the JSON like representation? 

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:182 on 2023-06-05 07:23_

You're right, lol. I'm sorry

---

_@MichaReiser reviewed on 2023-06-05 07:23_

---

_@MichaReiser reviewed on 2023-06-05 07:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:23 on 2023-06-05 07:34_

I think it would roughly do the same as `update_cell_offsets` does today: 

The source map is defined as a `Vec<SourceMapMarker>`  where each marker stores the `source` to `destination` position:

https://github.com/charliermarsh/ruff/blob/d1c7a07d2ae56f4ff56b81c7f2a398e2262c42b9/crates/ruff_formatter/src/lib.rs#L285-L290

* Insertion: 
	* One marker at the start: `insertion_offset` -> `output.text_len()`
	* One marker at the end: `insertion_offset` -> `output.text_len()`
* Deletion:
	* One marker at the start: `deletion_start` -> `output.text_len()`
	* One marker at the end: `deletion_end` -> `output.text_len()`
* Replacement:
	* One marker at the start: `replacement_start` -> `output.text_len()`
	* One marker at the end: `replacement_end` -> `output.text_len()`


The entries are guaranteed to be sorted (at least per "apply fixes" iteration). We can now search the start and end of each cell (or use an iterator and make use of the sorted property to avoid `cell` count binary searches):

* cell start: Find the last source map entry that is smaller or equal to the cell start. The output position of the start is: `marker.dest + cell.start - marker.source` I think we need to use the first entry if multiple source marker start RIGHT at the start of the cell. 
* cell end: Find the last source map entry that ends before or at `cel.end`. You can map the end as: `marker.dest + marker.source - cell.end`

There are probably some corner cases that I didn't consider. 

One downside of this is that returning the `BTreeMap` is free because we build it anyway whereas building the source map has the cost of writing the source mark entries for each edit.

---

_@dhruvmanila reviewed on 2023-06-06 08:14_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:75 on 2023-06-06 08:14_

I'm not sure if that'll work, correct me if I'm wrong. So, `IndexVec` will own it's elements. This means we'll have to copy all of the valid code `Cell` objects into this vector. At the end, we'll have to copy it back to the `RawNotebook` to serialize it into JSON. Move won't work here as only valid cells will be part of the vector thus leaving a partial vector.

---

_Renamed from "Initial support for auto-fix in Jupyter notebooks" to "Add support for auto-fix in Jupyter notebooks" by @dhruvmanila on 2023-06-09 13:05_

---

_Comment by @dhruvmanila on 2023-06-09 13:16_

## Updates since last review

* `SourceMap` have been added and the cell offsets are updated using that
* Empty code cells are handled while creating the index
* Basic unit tests have been added to tests the above improvements
* Import sorting panic has been fixed by considering each cell as an individual block using the cell offsets

---

_Review comment by @charliermarsh on `crates/ruff/src/linter.rs`:296 on 2023-06-11 02:25_

Let's use TODO over FIXME for consistency within the codebase.

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/source_map.rs`:8 on 2023-06-11 02:25_

I think many of the `pub`s in here can be `pub(crate)`.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/block.rs`:153 on 2023-06-11 02:28_

Nice.

---

_Review comment by @charliermarsh on `crates/ruff/src/source_kind.rs`:7 on 2023-06-11 02:29_

Nice.

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:344 on 2023-06-11 02:40_

I'm loving all the documentation you're adding as you go. This is a very, very good habit. Props.

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/notebook.rs`:195 on 2023-06-11 02:41_

Nit: maybe move this up a line? To match the logic: "Push the contents to the current cell, and push the starting offset of the _next_ cell."

---

_@charliermarsh approved on 2023-06-11 02:44_

This is really excellent work.

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:46 on 2023-06-12 08:32_

my concern before setting this to on by default would be that this causes a bunch of undefined symbols when the symbols do exist. but i'd say let's merge this and check with notebooks from github whether this is even actually a problem.

`find . -name "*.ipynb" | wc -l` in the ecosystem checkouts says 7639, so we have more than enough notebooks to check

---

_@konstin approved on 2023-06-12 09:42_

Is there any integration test that shows a jupyter notebook before and after being fixed?

---

_@dhruvmanila reviewed on 2023-06-12 13:42_

---

_Review comment by @dhruvmanila on `crates/ruff/src/autofix/source_map.rs`:8 on 2023-06-12 13:42_

This isn't possible currently as `Notebook` is being initialized in `ruff_cli` which uses `SourceMap` in one of it's public function which utilizes `SourceMarker`.

---

_Review comment by @dhruvmanila on `crates/ruff/src/autofix/source_map.rs`:8 on 2023-06-12 13:48_

Hmm, so it is possible it's just that the some parts will be public to allow `ruff_cli` to access.

---

_@dhruvmanila reviewed on 2023-06-12 13:48_

---

_@dhruvmanila reviewed on 2023-06-12 13:57_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:46 on 2023-06-12 13:57_

Yes, I do think this will be a problem, I've looked at how black handles it. I'll open an issue detailing out what needs to happen and we can then implement it.

---

_Comment by @dhruvmanila on 2023-06-12 14:05_

> Is there any integration test that shows a jupyter notebook before and after being fixed?

Currently, I've tested it out on the repositories mentioned in the PR description. I'm trying to think how to have E2E testing here as just the snapshots might not be enough. I'll run it on the ecosystem notebooks.

---

_Merged by @dhruvmanila on 2023-06-12 14:14_

---

_Closed by @dhruvmanila on 2023-06-12 14:14_

---

_Branch deleted on 2023-06-12 14:15_

---

_Comment by @konstin on 2023-06-12 14:27_

i was thinking about a notebook where we have before fixing and after fixing both in git and check that the fixed version looks like the one we have stored

---

_Comment by @dhruvmanila on 2023-06-12 15:51_

Right, so do you mean to compare the 2 JSON objects? They both will be available in source control. I think we should rather compare the source code content as the JSON will not be identical:

> The reason to choose `String` over `StringArray` is that the latter should contain the newline as part of each element which means that `str.lines` would not work and we would either need a custom iterator with strings _including_ the newline character or add newlines to each element individually.
>
> https://github.com/astral-sh/ruff/pull/4665#discussion_r1212663495

So, 2 notebooks where one is the original content and the other what we expect Ruff to transform into.
1. Read the notebook
2. Pass the source code to Ruff
3. Update the internal state
4. Read the expected notebook
5. Compare


---

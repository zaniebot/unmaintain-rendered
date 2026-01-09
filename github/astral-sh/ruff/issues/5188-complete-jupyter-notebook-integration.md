---
number: 5188
title: Complete Jupyter notebook integration
type: issue
state: closed
author: dhruvmanila
labels:
  - core
assignees: []
created_at: 2023-06-19T18:30:44Z
updated_at: 2024-02-06T12:13:07Z
url: https://github.com/astral-sh/ruff/issues/5188
synced_at: 2026-01-07T13:12:15-06:00
---

# Complete Jupyter notebook integration

---

_Issue opened by @dhruvmanila on 2023-06-19 18:30_

Meta issue to keep track of the remaining tasks before shipping Jupyter notebook native integration:

- [x] #4727
- [ ] #5189
- [ ] Add support for viewing Jupyter Notebooks in the playground
- [x] #5190

## Formatter Support

- [ ] ~Add support for `IpyEscapeCommand` token in [`SimpleTokenizer`](https://github.com/astral-sh/ruff/blob/036035bc502cd13dbb555385a3e6398f89b977a5/crates/ruff_python_trivia/src/tokenizer.rs) (if required)~
- [x] Add support for formatting `StmtIpyEscapeCommand`
- [x] Add support for formatting `ExprIpyEscapeCommand`
- [x] https://github.com/astral-sh/ruff/pull/7749

## Editor Integrations

- [x] https://github.com/astral-sh/ruff-vscode/issues/256 - I think this will probably involve changes in `ruff-lsp` so that other editors can benefit as well.

## IPython Syntax Handling (`%`, `!`, etc.)

- [x] #5030

### Ruff

- [x] #6090
- [x] #6087

### Proposed new rules

- [ ] Empty magic command (`%`) (Autofix: remove the statement)
- [ ] Magic command contains whitespace between the prefix and command value (`% matplotlib inline`) (Autofix: remove all the whitespaces) (Only valid for `%` magic prefix)

### Existing rule changes

- [x] #6116
- [x] #6672 

_I'll be looking at other related rules and update this issue accordingly_

### Parser

- [x] #6359
- [x] #6357
- [x] https://github.com/astral-sh/RustPython-Parser/pull/23
- [x] https://github.com/astral-sh/RustPython-Parser/pull/30
- [x] https://github.com/astral-sh/RustPython-Parser/pull/31

## Autofix for Jupyter Notebook

- [x] https://github.com/astral-sh/ruff/issues/1218

### Implementation

- [x] https://github.com/astral-sh/ruff/pull/5114
- [x] https://github.com/astral-sh/ruff/pull/5076
- [x] https://github.com/astral-sh/ruff/pull/5028
- [x] https://github.com/astral-sh/ruff/pull/4665

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-19 18:35_

---

_Referenced in [astral-sh/ruff#1218](../../astral-sh/ruff/issues/1218.md) on 2023-06-20 05:43_

---

_Comment by @charliermarsh on 2023-06-20 20:54_

My main question would be: at what point do you think we can remove the feature flag, and open this up to users? (Maybe we're already past that point!)

---

_Label `core` added by @charliermarsh on 2023-06-20 20:54_

---

_Comment by @dhruvmanila on 2023-06-21 03:00_

I do think we're ready except that without the (1) task above Ruff might give false positives. Without that we would ignore any cell which contains magic commands. Now, we would also ignore if it contains valid Python code. It could be that a variable is defined in that cell and is being referenced in another cell. Here, we might trigger undefined variable. Similar example can be given for imports.

I see this a lot for `%matplotlib inline` command: https://sourcegraph.com/search?q=context:global+lang:%22Jupyter+Notebook%22+%25matplotlib+inline&patternType=standard&sm=1&groupBy=repo

---

_Comment by @dhruvmanila on 2023-06-21 03:06_

What do you think about releasing this under experimental? I'm not sure what would that look like but maybe we don't add the `*.ipynb` to `include` and ask users to explicitly opt in to that.

---

_Comment by @charliermarsh on 2023-06-21 03:13_

Yeah, I think it would be reasonable to release it now (even without magic handling) and omit `*.ipynb` in `include`, so users need to opt-in explicitly. We can market it as experimental to get feedback. We can include it in the next release IMO.

We should also add the magic command support, but doesn't have to block if it's opt-in :)

---

_Comment by @charliermarsh on 2023-06-21 03:25_

(To be explicit: we could just remove the flag now.)

---

_Referenced in [astral-sh/ruff#5363](../../astral-sh/ruff/pulls/5363.md) on 2023-06-26 15:39_

---

_Referenced in [astral-sh/ruff-vscode#256](../../astral-sh/ruff-vscode/issues/256.md) on 2023-08-28 13:59_

---

_Referenced in [astral-sh/ruff#8094](../../astral-sh/ruff/issues/8094.md) on 2023-10-20 15:31_

---

_Comment by @jpmckinney on 2023-10-30 19:43_

I'm not sure if this deserves its own issue, but: Would it be possible to use `extend-include = ["*.ipynb"]` and `ruff format` to **only** re-format the Python code in cells, and **not** the ipynb file itself?

My users typically edit notebooks in Google Colab. Google Colab uses two-space indentation in the ipynb file, instead of the typical one-space indentation. `ruff format` currently re-indents with one space. As such, edit history becomes difficult/impossible to track, because effectively every line gets rewritten between the user saving from Google Colab (changing to 2 spaces) and CI running `ruff format` (changing to 1 space).

Google Colab also puts keys like metadata, nbformat, nbformat_minor at the top of the file, but `ruff format` moves these to the bottom. `ruff format` also changes the order of `execution_count`, `outputs`, etc.

I would love to be able to opt out of ipynb formatting, so that only the Python code inside cells is reformatted.

---

_Referenced in [astral-sh/ruff#8370](../../astral-sh/ruff/issues/8370.md) on 2023-10-31 02:22_

---

_Comment by @dhruvmanila on 2023-10-31 02:23_

(I think it deserves its own issue, we can discuss it there :))

---

_Referenced in [PyLops/pylops#485](../../PyLops/pylops/issues/485.md) on 2023-11-26 06:14_

---

_Comment by @dhruvmanila on 2024-02-06 12:13_

Closing this as completed. The only thing remaining is to support `--add-noqa`.

---

_Closed by @dhruvmanila on 2024-02-06 12:13_

---

_Referenced in [rapidsai/deployment#348](../../rapidsai/deployment/pulls/348.md) on 2024-03-19 16:18_

---

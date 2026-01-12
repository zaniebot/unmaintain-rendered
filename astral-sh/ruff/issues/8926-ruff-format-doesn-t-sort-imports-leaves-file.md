```yaml
number: 8926
title: "`ruff format` doesn't sort imports, leaves file unchanged"
type: issue
state: closed
author: alexei
labels:
  - question
assignees: []
created_at: 2023-11-30T15:36:05Z
updated_at: 2025-07-13T19:36:47Z
url: https://github.com/astral-sh/ruff/issues/8926
synced_at: 2026-01-12T15:54:48Z
```

# `ruff format` doesn't sort imports, leaves file unchanged

---

_@alexei_

I'm looking into using ruff (0.1.6 on Python 3.12) for formatting and... it doesn't appear to be doing anything, at least in terms of replicating `isort` behaviour.

### Input

```python
from _common import foo
import dataclasses as dc
import psycopg
from typing import Any
import hashlib
from foo.bar import baz
from functools import partial
from time import monotonic
from enum import Enum, auto
from uuid import UUID
```

### Command

```
$ python -m ruff format -v ./test_ruff.py
[2023-11-30][15:29:28][ruff_cli::resolve][DEBUG] Using Ruff default settings
[2023-11-30][15:29:28][ruff_cli::commands::format][DEBUG] format_path; path=.../test_ruff.py
[2023-11-30][15:29:28][tracing::span][DEBUG] Printer::print;
[2023-11-30][15:29:28][ruff_cli::commands::format][DEBUG] Formatted 1 files in 191.92µs
1 file left unchanged
```

### Expected result

```python
import dataclasses as dc
from enum import Enum, auto
from functools import partial
import hashlib
from time import monotonic
from typing import Any
from uuid import UUID

import psycopg

from _common import foo
from foo.bar import baz
```

Or something like that.

### Actual result

The file is unchanged.

---

What am I missing?

---

_Comment by @charliermarsh on 2023-11-30 15:54_

In Ruff, import sorting and re-categorization is part of the linter, not the formatter. The formatter will re-_format_ imports, but it won't rearrange or regroup them, because the formatter maintains the invariant that it doesn't modify the program's AST (i.e., its semantics and behavior).

To get isort-like behavior, you'd want to run `ruff check --fix` with `--select I` or adding `extend-select = ["I"]` to your `pyproject.toml` or `ruff.toml`.


---

_Comment by @zanieb on 2023-11-30 15:55_

See also https://github.com/astral-sh/ruff/issues/8087, https://github.com/astral-sh/ruff/issues/8612, and https://github.com/astral-sh/ruff/issues/8367

---

_Closed by @zanieb on 2023-11-30 15:55_

---

_Label `question` added by @zanieb on 2023-11-30 15:56_

---

_Comment by @alexei on 2023-11-30 16:06_

Got it, thanks!

---

_Comment by @Hubro on 2024-02-27 14:13_

While the rationale makes complete sense, this does make it pretty awkward and unintuitive to sort import statements as part of auto-formatting in editor configs and such. Would it be a possibility to add a shortcut for this as a flag to `ruff format`, just as a UX improvement?

```
--sort-imports
    Sort imports after formatting, equivalent to running `ruff check --fix --select I`
```

---

_Comment by @alexei on 2024-02-27 16:10_

I agree it sounds technically correct, but a little awkward from a practical standpoint. When I need to format the code, I use a script that runs:

```shell
ruff check . --select I --fix
ruff format .
```

---

_Comment by @ruslaniv on 2024-04-08 09:08_

> To get isort-like behavior, you'd want to run `ruff check --fix` with `--select I` or adding `extend-select = ["I"]` to your `pyproject.toml` or `ruff.toml`.

I have added
```python
[tool.ruff.lint]
extend-select = ["I"]
```
to `pyproject toml`, so now when running 
```shell
ruff check --fix config.py
```
I get
```shell
config.py:37:1: E402 Module level import not at top of file
Found 1 error.
```
but nothing gets resorted in the file. 
How do I tell ruff to actually physically resort the imports in the file just like iSort does

---

_Comment by @alexei on 2024-04-08 09:14_

@ruslaniv have you tried select instead of extend-select?

---

_Comment by @ruslaniv on 2024-04-08 09:19_

Yes, just tried it
```python
[tool.ruff.lint]
select = ["I"]
```
and in this case it's not even picking up the error: 
```shell
ruff check --fix config.py           
All checks passed!
```

---

_Comment by @MichaReiser on 2024-04-08 09:27_

@ruslaniv please consider creating a new issue, ideally with minimal repro.

Can you try running `ruff check config.py --show-settings` to see what rules are selected?

---

_Comment by @gboeer on 2024-04-11 14:24_

**EDIT:** In my case, I just forgot to commit the `pyproject.toml` after having added the ruff settings (e.g. to do import sorting).  After committing the `pyproject.toml` the subsequent linting/sorting errors get caught and reformatted as expected in my pre-commit.

I can add a similar observation to this. Having a simple file like:

```py
from pathlib import Path
import os

print(Path(os.getcwd()))
```

and a pyproject.toml entry:
```
[tool.ruff.lint]
select = ["I"]
```

The cli command behaves as expected:
```bash
ruff check temp.py
temp.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

And will correctly fix the import statement when I add `--fix`

However, in my pre-commit hook the file will not be fixed but accepted as is (with the wrongly ordered inputs). 

My pre-commit is configured as:

```yml
- repo: https://github.com/astral-sh/ruff-pre-commit
  # Ruff version.
  rev: v0.3.5
  hooks:
    # Run the linter.
    - id: ruff
      args: [ --fix ]
    # Run the formatter.
    - id: ruff-format
```

---

_Comment by @charliermarsh on 2024-04-11 14:46_

@gboeer - This sounds similar to https://github.com/astral-sh/ruff-pre-commit/issues/64 -- perhaps chime in there?

---

_Comment by @zhou13 on 2024-06-19 19:06_

This issue causes some inconvenience using the sorting functionality:
* It is intuitive to think `ruff format` will help you reorder (i.e., format) the import, rather than using `ruff check --fix`;
* While it might be not true, but I would imagine that `ruff check --fix` might be somehow dangerous and thus hesitant to run it too often. However, sorting the import should be saf enough to run on every file save in IDE.
* This makes isort function from ruff lsp-server awkward to use in many IDEs:
  * Normally, IDE has an option like `format_on_save` using LSP, so it is very easy to run `ruff format` in your IDE;
  * To sort the import, one needs to run a code action. This takes more keystrokes, and it is much harder to configure to run it with `format_on_save` in general.

I do understand that currently import sorting and re-categorization is implemented as the linter, not the formatter, which might make sense in terms of the function group. But as a user, I think the software UI ergonomics is more important here.

---

_Comment by @ll-nick on 2025-07-13 19:36_

For neovim, I solved this issue by calling both `vim.lsp.buf.format` and the `source.organizeImports` in the `BufPreWrite` autocommand.

```
function(client, bufnr)
    local augroup = vim.api.nvim_create_augroup("LspFormatting", {})
    if client.supports_method("textDocument/formatting") then
      vim.api.nvim_clear_autocmds({ group = augroup, buffer = bufnr })
      vim.api.nvim_create_autocmd("BufWritePre", {
        group = augroup,
        buffer = bufnr,
        callback = function()
          vim.lsp.buf.format({
            async = false,
          })

          -- Ruff proved import organization via the linter rather than the formatter.
          -- Call the corresponding code action here to get auto sort on save behavior analogous to e.g. clang-format.
          -- See https://github.com/astral-sh/ruff/issues/8926 for reference
          if client.name == "ruff" then
            vim.lsp.buf.code_action({
              context = { only = { "source.organizeImports" } },
              apply = true,
              buffer = bufnr,
            })
          end
        end,
      })
    end
end
```

---

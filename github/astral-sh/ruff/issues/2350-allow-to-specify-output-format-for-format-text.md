---
number: 2350
title: allow to specify output format for --format=text
type: issue
state: open
author: spaceone
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-01-30T12:09:25Z
updated_at: 2024-04-07T10:18:26Z
url: https://github.com/astral-sh/ruff/issues/2350
synced_at: 2026-01-07T13:12:14-06:00
---

# allow to specify output format for --format=text

---

_Issue opened by @spaceone on 2023-01-30 12:09_

flake8 supports specifying the output format via an command line argument: `flake8 --format '%(path)s'`

allowed specifiers are:
    * code
    * text
    * path
    * row
    * col

ruff could support `--error-format '{path}:{row}:{col} {code}: {text}` when `--format=text`.

This would allow me to easily open all files with specific violations in my editor (`vim -p $(ruff --select E --error-format '{path}')` )

---

_Label `configuration` added by @charliermarsh on 2023-01-30 12:17_

---

_Comment by @ngnpope on 2023-01-30 12:17_

You're likely to end up with duplicates like that if there are multiple violations in a file. So you'd want another flag to avoid duplicate rows, or have `ruff` deduplicate automatically, or use `ruff --select E --error-format '{path}' | sort --unique`.

Also note that `tabpagemax` is set to `10` by default, so you may want to up that when using `-p`...

---

_Comment by @charliermarsh on 2023-01-30 12:20_

I kind of intentionally didn't do this because it felt like it'd add a good deal of complexity, and that in most cases these problems would be solvable by chaining different tools. E.g., in this case, couldn't you pipe the output, split at `:`, and take everything before it to see the list of paths?

---

_Comment by @ngnpope on 2023-01-30 12:20_

This would also be nice for formatting the output when using `--statistics`. 

---

_Comment by @ngnpope on 2023-01-30 12:24_

> ...and that in most cases these problems would be solvable by chaining different tools.

True, this is possible, and you don't even need to fiddle splitting text strings:

```console
ruff check --format=json . | jq --raw-output .[].filename | sort --unique
```

---

_Comment by @spaceone on 2023-01-30 13:00_

> You're likely to end up with duplicates like that if there are multiple violations in a file. So you'd want another flag to avoid duplicate rows, or have `ruff` deduplicate automatically, or use `ruff --select E --error-format '{path}' | sort --unique`.

jeah, that's not something I expect `ruff` to do.

> Also note that `tabpagemax` is set to `10` by default, so you may want to up that when using `-p`...

yes, I have [set tabpagemax=30 in vimrc](https://github.com/spaceone/dotfiles/blob/master/vimrc#L8).

Currently I am using `vim -p $(ruff --select SIM115 --fix $files | sed -rne 's/:.*//gp')`.
So this feature would just allow me to not always write that much/complex.

---

_Comment by @goodspark on 2023-05-08 18:18_

FWIW, this will make Ruff even easier to drop in with VSCode's built-in flake8 integration, which fails bc VSCode sends `--format "%(row)d,%(col)d,%(code).1s,%(code)s:%(text)s"` to Ruff. Alternatively, one could just use the Ruff VSCode extension.

---

_Label `needs-decision` added by @MichaReiser on 2024-03-30 09:16_

---

_Comment by @ankush on 2024-04-07 10:18_

pylint format works with vim's quickfix (better than just opening file as this will also use location and error message)

```
nvim -q <(ruff check --output-format=pylint)
```



---

_Referenced in [astral-sh/ruff#20438](../../astral-sh/ruff/issues/20438.md) on 2025-09-17 07:22_

---

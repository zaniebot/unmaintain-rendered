```yaml
number: 15031
title: "Support `grouped` output format for `--statistics`"
type: issue
state: open
author: un-pogaz
labels:
  - cli
assignees: []
created_at: 2024-12-17T08:52:54Z
updated_at: 2025-12-24T07:42:40Z
url: https://github.com/astral-sh/ruff/issues/15031
synced_at: 2026-01-12T15:54:54Z
```

# Support `grouped` output format for `--statistics`

---

_@un-pogaz_

Actualy, if your use the option --statistics to `ruff check`, Ruff will output the list of all rules violations but never specifie wich file has the errors. And if your don't use the option --statistics, Ruff generate a output that will content each singular rule violation, which can quickly become particularly unreadable.

In a generale way, there is no option to get a clear list of files with rules violations, and only them.

So, I suggest to add a option --statistics-per-files derivated from the original --statistics, or a additional option --per-files to append to the original one.

This option will output a statistics table, but grouped for each individual files that have a rule violations.
Like this:
```
src/utils/catalogs.py:
5    W291    [*] trailing-whitespace
1    I001    [*] unsorted-imports
1    W292    [*] missing-newline-at-end-of-file
[*] fixable with `ruff check --fix`

src/formatter_functions.py:
3   INT003  [ ] printf-in-get-text-func-call
2   E721    [ ] type-comparison
1   F821    [ ] undefined-name

src/main.py:
1   I001    [*] unsorted-imports
1   F821    [ ] undefined-name
1   W191    [ ] tab-indentation
[*] fixable with `ruff check --fix`

[...]
```

This allow to quick know wich files need to be edit, open it in your IDE, IDE that will show you the exact position of each error in this file.

---

_Comment by @dhruvmanila on 2024-12-17 09:02_

Thanks for the suggestion! What do you think about using `--output-format=grouped`? That doesn't show the statistics but groups the diagnostics on a per-file basis.

---

_Label `cli` added by @dhruvmanila on 2024-12-17 09:02_

---

_Comment by @un-pogaz on 2024-12-17 11:34_

Oh, thanks. This result is good for my usage.

So, their is no need to add a "new" option for `--statistics`, because `--output-format=grouped` can take this role, their just need to make them compatible together, which would be really handy and shorter (my output take ~5000 lines, need to clean up but usefull)

---

_Renamed from "Add a option --statistics-per-files" to "Support `grouped` output format for `--statistics`" by @dhruvmanila on 2024-12-18 05:49_

---

_Comment by @y2kbugger on 2025-12-23 19:37_

I'm hoping for an output format of json or toml for writing scripts against.


---

_Comment by @MichaReiser on 2025-12-24 07:42_

> I'm hoping for an output format of json or toml for writing scripts against.

Have you tried `--output-format=json`:

```
uvx ruff@latest check --statistics --output-format=json
warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
[
  {
    "code": null,
    "name": "invalid-syntax",
    "count": 12,
    "fixable": false,
    "fixable_count": 0
  },
  {
    "code": "D103",
    "name": "undocumented-public-function",
    "count": 1,
    "fixable": false,
    "fixable_count": 0
  },
  {
    "code": "T201",
    "name": "print",
    "count": 1,
    "fixable": false,
    "fixable_count": 0
  }
]
```

---

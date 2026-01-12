```yaml
number: 15882
title: "Docs (`linter.md`): clarify that Python files are always searched for in subdirectories"
type: pull_request
state: merged
author: anordin95
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-02T19:25:01Z
updated_at: 2025-02-04T15:36:17Z
url: https://github.com/astral-sh/ruff/pull/15882
synced_at: 2026-01-12T15:55:53Z
```

# Docs (`linter.md`): clarify that Python files are always searched for in subdirectories

---

_@anordin95_

## Nits related to clarity of which files are linted

Hi!

I noticed no mention of recursing through sub-dirs and only the fourth example: `ruff check path/to/code/ ` included the phrase: "and any subdirectories" leading me to believe only providing a path would recurse and the others would not. I don't think that's the actual behavior. It does feel a bit repetitive to say so in each example and in the intro paragraph, but I do somewhat like the clarity.

Also, I opted to remove the "all" from "Lint all files in the current directory." (and others) since I presume it doesn't lint all files, only "all discovered Python files". Without the "all", I think it's clear many files will be linted, and it gives
a hint that further logic exists for filtering.

---

_Label `documentation` added by @AlexWaygood on 2025-02-03 11:38_

---

_Renamed from "nits: linter.md" to "Docs nits in `linter.md`" by @AlexWaygood on 2025-02-03 11:38_

---

_@AlexWaygood reviewed on 2025-02-03 11:53_

---

_Review comment by @AlexWaygood on `docs/linter.md`:20 on 2025-02-03 11:53_

I can see how this is unclear on `main`, but the suggested rewording here _does_ feel a little repetitive now. What about something like this?

````
`ruff check` is the primary entrypoint to the Ruff linter. It accepts a list of files or
directories, and lints all discovered Python files, optionally fixing any fixable errors.
When linting a directory, Ruff searches for Python files recursively in that directory
and all its subdirectories:

```console
$ ruff check                  # Lint all files in the current directory.
$ ruff check --fix            # Lint all files in the current directory, and fix any fixable errors.
$ ruff check --watch          # Lint all files in the current directory, and re-lint on change.
$ ruff check path/to/code/    # Lint all files in `path/to/code`
```
````


---

_Renamed from "Docs nits in `linter.md`" to "Docs (`linter.md`): clarify that Python files are always searched for in subdirectories" by @AlexWaygood on 2025-02-03 11:54_

---

_@anordin95 reviewed on 2025-02-03 18:35_

---

_Review comment by @anordin95 on `docs/linter.md`:20 on 2025-02-03 18:35_

Mmm, yes that's better! Well done :)

---

_@anordin95 reviewed on 2025-02-03 18:46_

---

_Review comment by @anordin95 on `docs/linter.md`:20 on 2025-02-03 18:46_

PS do you have opinions on:
`Lint all files in the current directory.`
vs.
`Lint files starting from the current directory.`

I prefer the latter since it hints there is some filtering and recursion without losing clarity, e.g. only Python files, respect .gitignore, etc. 

---

_@AlexWaygood approved on 2025-02-04 15:33_

Thanks!

---

_Merged by @AlexWaygood on 2025-02-04 15:36_

---

_Closed by @AlexWaygood on 2025-02-04 15:36_

---

```yaml
number: 2536
title: Add helper info for when a import would work except for missing extension
type: issue
state: open
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2026-01-16T20:29:51Z
updated_at: 2026-01-16T20:29:51Z
url: https://github.com/astral-sh/ty/issues/2536
synced_at: 2026-01-16T21:03:54Z
```

# Add helper info for when a import would work except for missing extension

---

_@MeGaGiGaGon_

This is a situation that I run into around once a year when copy/pasting code between projects. Sometimes a newly made file will be missing the `.py` extension, so despite looking correct and even having syntax highlighting thanks to VSCode recognizing it as a python file, it doesn't work for seemingly no reason. It would be nice if ty gave an additional note in this case since tracking it down can be extremely frustrating.
https://play.ty.dev/51dd064f-ddb2-42c6-a2aa-3db4c2f3f508
```powershell
PS ~\ty_issues>echo "import module" > main.py
PS ~\ty_issues>echo "" > module  # Note the missing .py extension
PS ~\ty_issues>uvx ty check
error[unresolved-import]: Cannot resolve imported module `module`
 --> main.py:1:8
  |
1 | import module
  |        ^^^^^^
  |
info: Searched in the following paths during module resolution:
# Paths omitted for brevity
Found 1 diagnostic
PS ~\ty_issues>echo "" > module.py  # On fixing the extension everything works fine again
PS ~\ty_issues>uvx ty check
All checks passed!
```
The help could look something like this:
```powershell
error[unresolved-import]: Cannot resolve imported module `module`
 --> main.py:1:8
  |
1 | import module
  |        ^^^^^^
  |
help: Found matching file named `module`, but it is not importable due to missing `.py` extension
info: Searched in the following paths during module resolution:
```
It would also be extra-extra nice if that help also showed up in the on-hover-error-display in VSC.

---

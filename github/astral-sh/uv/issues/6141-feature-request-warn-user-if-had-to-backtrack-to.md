---
number: 6141
title: "Feature Request: Warn user if had to backtrack to a very old version of a package for a newer version of Python"
type: issue
state: open
author: notatallshaw
labels: []
assignees: []
created_at: 2024-08-16T03:21:41Z
updated_at: 2024-08-16T14:32:51Z
url: https://github.com/astral-sh/uv/issues/6141
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request: Warn user if had to backtrack to a very old version of a package for a newer version of Python

---

_Issue opened by @notatallshaw on 2024-08-16 03:21_

My example here involves universal resolution, but I think this issue is a more general user experience issue any time backtracking is involved because newer packages don't satisfy newer Python versions.

If resolve for the docker package with minimum version of Python 3.12 I get docker 7.1.0:
```
echo "docker" | uv pip compile --universal  --python-version 3.12 --annotation-style line - 2>/dev/null | grep "d
ocker=="
docker==7.1.0
```

But if I resolve for docker package with minimum version of Python 3.13 I get docker 2.7.0:
```
echo "docker" | uv pip compile --universal  --python-version 3.13 --annotation-style line - 2>/dev/null | grep "d
ocker=="
docker==2.7.0
```

Now, I understand why this is technically correct, but docker 2.7.0 was released on 2017-12-19, where as Python 3.13 is expected to be released on 2024-10-01. It is not likely a "real" solution that to satisfy a new version of Python you need a *very* old package.

If uv has to backtrack to previous versions of a package to meet minimum Python versions, it would be useful for it to warn the user when that previous version of the package was released long before that minimum version of Python. There should be some leeway, packages can start getting ready for a new version of Python upto 1 year beforehand.

I don't think uv can "fix" this, but I do think they can warn the user that this happened (though extracting the correct heuristics from the resolution might be extremely difficult?). 


---

_Referenced in [astral-sh/uv#6186](../../astral-sh/uv/issues/6186.md) on 2024-08-20 00:41_

---

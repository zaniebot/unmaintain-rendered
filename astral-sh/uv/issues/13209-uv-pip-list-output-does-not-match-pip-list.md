```yaml
number: 13209
title: "`uv pip list` output does not match `pip list`: distribution package names instead of import package names"
type: issue
state: closed
author: gjoseph92
labels:
  - question
  - compatibility
assignees: []
created_at: 2025-04-30T02:09:28Z
updated_at: 2025-05-02T14:28:40Z
url: https://github.com/astral-sh/uv/issues/13209
synced_at: 2026-01-12T16:01:22Z
```

# `uv pip list` output does not match `pip list`: distribution package names instead of import package names

---

_@gjoseph92_

### Summary

`uv pip list` and `pip list`'s outputs seem to refer to package names differently. I believe uv is using [distribution package names](https://packaging.python.org/en/latest/discussions/distribution-package-vs-import-package/#what-s-a-distribution-package), whereas pip is using [import package names](https://packaging.python.org/en/latest/discussions/distribution-package-vs-import-package/#what-s-an-import-package).

```shell
uv init uv-pip-list
cd uv-pip-list
uv add docstring-parser jaraco-classes pymupdfb pypdf2 pip
diff --side-by-side  <(uv run pip list) <(uv pip list)
```
```
Package          Version					Package          Version
---------------- -------					---------------- -------
docstring_parser 0.16					      |	docstring-parser 0.16
jaraco.classes   3.4.0					      |	jaraco-classes   3.4.0
more-itertools   10.7.0						more-itertools   10.7.0
pip              25.1						pip              25.1
PyMuPDFb         1.24.10				      |	pymupdfb         1.24.10
PyPDF2           3.0.1					      |	pypdf2           3.0.1
```

Notice that the `_` in `docstring_parser` (pip) becomes `-` in `docstring-parser` (uv), case in `PyMuPDFb` is lost, etc.

Not sure this is a big deal. When I switched a system over to uv, it broke some old code that parses the output of `pip list`. Honestly, I might prefer uv's choice, since it matches with what you'd `uv pip uninstall/install`. But given how widely `pip list` is probably parsed, being consistent might be preferable.

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.4.30

### Python version

Python 3.12.5

---

_Label `bug` added by @gjoseph92 on 2025-04-30 02:09_

---

_Comment by @zanieb on 2025-04-30 02:12_

We always display normalized package names. pip isn't showing the import package name, I think it's just showing the non-canonicalized published package name.

---

_Label `bug` removed by @zanieb on 2025-04-30 02:12_

---

_Label `question` added by @zanieb on 2025-04-30 02:12_

---

_Label `compatibility` added by @zanieb on 2025-04-30 02:12_

---

_Comment by @zanieb on 2025-04-30 02:13_

If you have a use-case, we're happy to hear it — but generally we've been firm on showing normalized names throughout.

---

_Comment by @gjoseph92 on 2025-04-30 02:33_

Makes sense—the normalization would tend to produce something that looks like an import package name. By normalization, you mean specifically https://packaging.python.org/en/latest/specifications/name-normalization/#name-normalization?

> If you have a use-case, we're happy to hear it — but generally we've been firm on showing normalized names throughout.

I think that's a good move for uv, and I appreciate the consistency.

I guess it's a tradeoff in the `uv pip` interface, though. Matching pip and all its oddities can be unpleasant, and isn't something you claim to do anyway. On the other hand, matching pip and all its oddities makes drop-in replacement easier, which helps with adoption.

For me, this is fine either way; I'm going to update the integration test this took down to be more robust and not parse the output of `pip list`. I more opened this for others to find. I imagine plenty of CI code somewhere is diffing `pip list`; I was honestly a bit surprised nobody had opened this yet—maybe that's a good sign it doesn't come up that much.

---

_Comment by @konstin on 2025-04-30 08:49_

Have you seen `uv pip list --format json`? The text output is subject to change for stylistic reasons, so we recommend parsing the JSON output. I can also recommend https://packaging.pypa.io/en/stable/utils.html#packaging.utils.NormalizedName for this, which allows operating on normalized package names in Python.

---

_Closed by @charliermarsh on 2025-05-02 14:28_

---

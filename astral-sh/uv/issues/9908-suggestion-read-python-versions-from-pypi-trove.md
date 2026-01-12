```yaml
number: 9908
title: "Suggestion: read Python versions from PyPI Trove Classifiers"
type: issue
state: open
author: RomainBrault
labels:
  - wish
  - needs-decision
assignees: []
created_at: 2024-12-15T09:57:33Z
updated_at: 2024-12-16T20:31:29Z
url: https://github.com/astral-sh/uv/issues/9908
synced_at: 2026-01-12T16:00:02Z
```

# Suggestion: read Python versions from PyPI Trove Classifiers

---

_@RomainBrault_

Would it be possible to have an option to read Python versions from trove classifiers ?

I.e. read

```string
Programming Language :: Python :: 3.13
```

in `pyproject.toml` instead of `.python-version(s)`.

Rational:

 - Many people using pypi will set this metadata. I guess it is wildly adopted.
 - It is used in some Github actions as metadata (e.g. https://github.com/hynek/build-and-inspect-python-package)
 - It would help maintenance by avoiding duplication of information

Bonus:
 - In this case accessing the python versions would require some toml parsing and string splitting. Thus it would be helpful to have in uv something that give a clean list of the project Python versions or even the intersection of the project Python versions and the set of UV available Python.

---

_Comment by @zanieb on 2024-12-15 16:48_

The classifiers are for all supported versions, right? This should already be available via `requires-python`, with the exception of whether or not the latest version is supported in some cases?

---

_Comment by @RomainBrault on 2024-12-16 07:26_

requires-python are just some bounds and not a list of Python versions. You don´t need to provide an upper one or set it very loose like python<4.0. You can also set exception on minor version in the requires-python segment.

On the other hand the trove classifiers are usually a list of labels of major python versions supported, which looks similar in purpose (while still a bit different) to the .python-versions file.

Maybe I miss the intended purpose of `.python-version file`. From what I understood from the doc, it provide a convenient way to do `uv python install` for a bunch of Python versions/interpreter listed in `.python-versions`. In the case where this list is just a bunch of major python version this information is already in the trove classifiers.

Trove classifier are related but independent from requires-python, as requires-python can have loose bound for forward compatibility (library). For a major version of Python represented by a trove classifier, the user (or uv) should pull the latest Python version for this major, respecting the bound given by requires-python.

---

_Label `wish` added by @zanieb on 2024-12-16 20:31_

---

_Label `needs-decision` added by @zanieb on 2024-12-16 20:31_

---

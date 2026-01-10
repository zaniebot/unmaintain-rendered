```yaml
number: 12916
title: "ICN001 and N817 don't play with each other"
type: issue
state: closed
author: ember91
labels:
  - bug
  - incompatibility
assignees: []
created_at: 2024-08-16T05:51:14Z
updated_at: 2024-08-19T19:37:10Z
url: https://github.com/astral-sh/ruff/issues/12916
synced_at: 2026-01-10T11:09:54Z
```

# ICN001 and N817 don't play with each other

---

_Issue opened by @ember91 on 2024-08-16 05:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## List of keywords searched
 - ICN001
 - N817

## Minimal code snippets

`test1.py`

```python
import xml.etree.ElementTree as Et
```

`test2.py`

```python
import xml.etree.ElementTree as ET
```

## Invoked commands

```bash
ruff check test1.py test2.py
```

## Current ruff settings

```toml
select = ["ICN001", "N817"]
```

## Current ruff version

`ruff 0.6.0`

## Description

It seems like `N817` wants me to import it as `Et` while `ICN001` wants me to import it as `ET`. Perhaps it is ok that rules conflict in ruff. If so I guess this is not a bug.

---

_Label `bug` added by @MichaReiser on 2024-08-16 06:10_

---

_Comment by @MichaReiser on 2024-08-16 06:12_

Thanks. No, that's a bug. Rules should be compatible out of the box. 

I see two options here:

1. `N817` to ignore any names configured in `checker.settings.flake8_import_conventions.aliases` when `ICN001` is enabled
1. `ICN001` to ignore all camel case names configured in `checker.settings.flake8_import_conventions.aliases` if `N817` is enabled.

I prefer 1. because it involves less conditional logic (it ignores all the names rather than *some*). 

@charliermarsh what do you think?

---

_Comment by @cclauss on 2024-08-16 07:52_

I also prefer 1.

% `echo "from xml.etree import ElementTree" | ruff check --select=ALL --ignore=D100,F401 -`
```
-:1:23: ICN001 `xml.etree.ElementTree` should be imported as `ET`
  |
1 | from xml.etree import ElementTree
  |                       ^^^^^^^^^^^ ICN001
```
% `echo "import xml.etree.ElementTree as ET" | ruff check --select=ALL --ignore=D100,F401 -`
```
-:1:8: N817 CamelCase `ElementTree` imported as acronym `ET`
  |
1 | import xml.etree.ElementTree as ET
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^ N817
```

---

_Comment by @NMertsch on 2024-08-16 09:18_

I also prefer 1. Not because of the implementation logic, but because it favors the specific rule `ICN001` over the generic rule `N817`:

- [N817](https://docs.astral.sh/ruff/rules/camelcase-imported-as-acronym/): Generally, don't use acronyms for camel-case imports, like `MCN` for `MyClassName`.
- [ICN001](https://docs.astral.sh/ruff/rules/unconventional-import-alias/): For `ElementTree` specifically, use the alias `ET` because that's the convention for this module.

---

_Comment by @AlexWaygood on 2024-08-16 09:23_

(1) sounds good to me too

---

_Assigned to @MichaReiser by @MichaReiser on 2024-08-16 09:29_

---

_Label `incompatibility` added by @MichaReiser on 2024-08-16 09:50_

---

_Comment by @ember91 on 2024-08-16 15:17_

Wow! You are so responsive in this repo. Hats off!

---

_Closed by @MichaReiser on 2024-08-16 15:28_

---

_Comment by @hofbi on 2024-08-19 17:05_

@MichaReiser I still see `N817 CamelCase ElementTree imported as acronym ET` on `v0.6.1`. If I understand https://github.com/astral-sh/ruff/pull/12922 correctly, it should now be `import ElementTree as XET`? However, the autofix of `ICN001` still autofixes to `import ElementTree as ET`. Do I miss anything?

---

_Comment by @cclauss on 2024-08-19 17:40_

I have never seen `XET` in the wild and the docs/tutorial recommends `ET`.

https://docs.python.org/3/library/xml.etree.elementtree.html#tutorial

---

_Comment by @AlexWaygood on 2024-08-19 17:45_

@hofbi / @cclauss: We are definitely not recommending `XET` as an alias for `xml.etree.ElementTree` in this rule! 😄

The tests added in #12922 are specifically testing that the "conventional aliases" setting can be used to configure this rule. One fixture is being run with the "conventional aliases" setting being set to `"xml.etree.ElementTree": "XET"`, and the snapshot you see as a result shows you what would happen if you ran Ruff with that (strange) configuration

---

_Comment by @hofbi on 2024-08-19 18:45_

Ok makes sense for XET. Then do I have to configure anything so that both check play nicely together. Right now I still see the N817 error after the autofix adding ET.

---

_Comment by @MichaReiser on 2024-08-19 18:48_

> Ok makes sense for XET. Then do I have to configure anything so that both check play nicely together. Right now I still see the N817 error after the autofix adding ET.

There's a separate fix for `from ... import` that has not been released yet https://github.com/astral-sh/ruff/pull/12946 

If the incompatibility that you're seeing isn't related to `from ... import`, please open a new issue and share a code example with us. Happy to take a look.

---

_Comment by @hofbi on 2024-08-19 18:51_

Oh I missed that one. My issue is definitely https://github.com/astral-sh/ruff/issues/12940. So just waiting for the next release to get this fixed, thanks.

---

_Comment by @MichaReiser on 2024-08-19 19:37_

No worries. I missed that one too when implanting the fix 🥲

---

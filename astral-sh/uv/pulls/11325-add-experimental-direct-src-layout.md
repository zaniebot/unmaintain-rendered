```yaml
number: 11325
title: Add experimental direct src layout
type: pull_request
state: open
author: konstin
labels:
  - preview
assignees: []
draft: true
base: main
head: konsti/direct-src-layout
created_at: 2025-02-07T18:14:55Z
updated_at: 2025-10-06T20:26:42Z
url: https://github.com/astral-sh/uv/pull/11325
synced_at: 2026-01-10T06:36:15Z
```

# Add experimental direct src layout

---

_Pull request opened by @konstin on 2025-02-07 18:14_

Currently, there are two main options for a project layout, as described in https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/.

`<package_name>/__init__.py`: The project is importable even when not installed. The community seems to be migrating to the `src` layout https://hynek.me/articles/testing-packaging/.

`src/<package_name>/__init__.py`: We add an extra layer of directory only to repeat the package name. This makes the library layout feel higher overhead than needed, making adoption especially in small projects harder, where the alternative is just having some Python files in a directory without any packaging.

As an alternative, we introduce `src/__init__.py`, a layout familiar from other programming languages. It is low overhead (no extra indirection, docs are "put your python files in `src`, `uv sync` and `import <package_name>`") and it enforces isolation through (editable) installation (you can't `import <package_name>` directly).

Python does not support this natively it always want the module name in the directory or filename. We hack around this by using a custom meta finder that we insert at the top import priority. This code runs at every python startup. Imports are lazy where possible to reduce overhead. Since we're hacking the Python import system to achieve this, it's experimental. I like `src/__init__.py` a lot, but everything that runs code through `.pth` in implicit, pre-main life is to be regarded with some suspicion.

This PR only adds editable support (the harder part), proper `build_{sdist,wheel}` will be added in an upstack PR.

---

_Label `preview` added by @konstin on 2025-02-07 18:14_

---

_@konstin reviewed on 2025-02-07 18:15_

---

_Review comment by @konstin on `crates/uv-build-backend/src/wheel.rs`:283 on 2025-02-07 18:15_

This feature should be stabilized separately from the general build backend.


---

_Comment by @carljm on 2025-02-07 22:37_

In this proposed layout, what determines the name of your top-level importable package name? It is determined by the name of the outer directory containing the `src/` directory? Or by the `name` key in the project metadata? Historically neither of those things have had any required correspondence with the top-level module name, which means there are a lot of existing packages that simply could not adopt this layout. And the fact that I have to even ask this question (and I don't think there's any answer that's obviously more intuitive than the other possible answers) is a warning sign.

Regardless of the answer, I find this pretty confusing. Why should the top-level package have an intervening `src/` directory between the package name and the contents, when no other Python package has this, and Python's own import system doesn't understand or support this?

Also, this layout doesn't really address the core problem with the "flat" layout that the "src" layout was originally meant to address, that if you run `python` in your project root, your code is importable when not installed. With this proposal, now it's just importable under the "wrong" top-level module name `src` instead of under the actual package name, which I am quite sure people will discover and abuse / be confused by.

What concrete advantage does this have over the existing flat layout?

Static analysis tools will be unable to understand libraries using this layout, unless we convince them to add special-cased support for it.

---

_Comment by @cthoyt on 2025-02-08 14:55_

Another good post on why src/package_name layout is good: https://blog.ionelmc.ro/2014/05/25/python-packaging/

---

_@T-256 reviewed on 2025-02-13 13:03_

---

_Review comment by @T-256 on `crates/uv-build-backend/src/metadata.rs`:856 on 2025-02-13 13:03_

It somehow confuses me as a user.
- `module_root` auto appends `<package_name>` to the given name.
- `direct_src` prevents previous step, then on editable installs, uses `_package_name.pth` and `_package_name_direct_src_support.py` for custom import loader.

Another approach could be always using direct path for `module_root` (i.e. `src` points to `src/__init__.py`) and always use custom import loader for editable installs.


---

_Converted to draft by @konstin on 2025-07-24 09:59_

---

_Comment by @ZachHandley on 2025-10-06 20:26_

https://github.com/astral-sh/uv/issues/16120

I actually just made a post about this -- I'd love this, personally

---

```yaml
number: 20458
title: "include `.pyw` files by default when linting and formatting"
type: pull_request
state: merged
author: amyreese
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: amy/pyw-extension
created_at: 2025-09-18T00:22:13Z
updated_at: 2025-09-24T15:39:32Z
url: https://github.com/astral-sh/ruff/pull/20458
synced_at: 2026-01-10T17:40:28Z
```

# include `.pyw` files by default when linting and formatting

---

_Pull request opened by @amyreese on 2025-09-18 00:22_

- Adds test cases exercising file selection by extension with `--preview` enabled and disabled.
- Adds `INCLUDE_PREVIEW` with file patterns including `*.pyw`.
- In global preview mode, default configuration selects patterns from `INCLUDE_PREVIEW`.
- Manually tested ruff server with local vscode for both formatting and linting of a `.pyw` file.

Closes https://github.com/astral-sh/ruff/issues/13246

---

_Label `configuration` added by @amyreese on 2025-09-18 00:28_

---

_Label `preview` added by @amyreese on 2025-09-18 00:28_

---

_Comment by @github-actions[bot] on 2025-09-18 00:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[ruff] include `.pyw` files by default when linting and formatting" to "include `.pyw` files by default when linting and formatting" by @MichaReiser on 2025-09-18 07:26_

---

_Comment by @MichaReiser on 2025-09-18 07:36_

The basics here look good, but there are a few more places we have to account for `pyw` files.

One such place is `PySourceType`. It's heavily used internally to query whether a file is a Python file, a stub file, or a notebook. We need to decide if we need to add a new variant or if it's okay to add `pyw` to `py` (might not be okay if `PySourceType` is used in any module resolution logic as `pyw` files aren't importable as far as I know). This will require reviewing where `PySourceType` is used (and how).

It might also be good to do a quick search for ".py" (or "pyi" because there are fewer instances) to see if there are other places where we check for the extension that need updating.

This is related to https://github.com/astral-sh/ruff/issues/13691



---

_Comment by @amyreese on 2025-09-19 03:55_

> One such place is `PySourceType`. It's heavily used internally to query whether a file is a Python file, a stub file, or a notebook. We need to decide if we need to add a new variant or if it's okay to add `pyw` to `py` (might not be okay if `PySourceType` is used in any module resolution logic as `pyw` files aren't importable as far as I know). This will require reviewing where `PySourceType` is used (and how).

I tried looking through various places where this is used, but there really is an overwhelming number of places where it's used that I don't have context on, but what I've looked at so far usually seems reasonable. 

I went ahead and added the mapping to the existing `Python` enum, and tests are passing, but I don't have a good idea what sort of tests I could add to help give signal on whether that is sufficient, or how to compare that with the alternative of adding a new enum type.

That said, I'm also not sure what you mean by "module resolution logic", so if you can suggest specific crates/files to look at more closely, I'd be happy to dig further.

> It might also be good to do a quick search for ".py" (or "pyi" because there are fewer instances) to see if there are other places where we check for the extension that need updating.

I did do that originally, though I searched for `ipynb` instead, but apparently I missed the exact one place that `PySourceType` showed up in the gigantic list of results. 😅🫠


---

_Comment by @MichaReiser on 2025-09-19 06:49_

> I tried looking through various places where this is used, but there really is an overwhelming number of places where it's used that I don't have context on,

Yeah, same here. We don't need to study each line for a very long time. It's okay if we miss something. 

I only did a quick search. Most usages indeed do look good. The one I think we need to take a closer look is 

https://github.com/astral-sh/ruff/blob/b0279265e82903b57bb32f8bf04a2962740c5a8f/crates/ruff_linter/src/rules/flake8_no_pep420/rules/implicit_namespace_package.rs#L72-L73

because it tries to resemble python's module resolution at runtime. 

---

_@MichaReiser approved on 2025-09-24 07:36_

---

_Merged by @amyreese on 2025-09-24 15:39_

---

_Closed by @amyreese on 2025-09-24 15:39_

---

_Branch deleted on 2025-09-24 15:39_

---

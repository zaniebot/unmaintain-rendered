```yaml
number: 19648
title: "Fix link: unused_import.rs"
type: pull_request
state: merged
author: hunterhogan
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-30T20:21:56Z
updated_at: 2025-08-01T10:10:44Z
url: https://github.com/astral-sh/ruff/pull/19648
synced_at: 2026-01-10T17:52:17Z
```

# Fix link: unused_import.rs

---

_Pull request opened by @hunterhogan on 2025-07-30 20:21_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

https://typing.python.org/en/latest/source/libraries.html returns a 301, which points to https://typing.python.org/en/latest/guides/libraries.html. But the anchor (and the information) was moved to https://typing.python.org/en/latest/spec/distributing.html.

I fixed the link. 

Since I had the file open, I skimmed the document for minor edits.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->




---

_Label `documentation` added by @MichaReiser on 2025-07-31 06:17_

---

_Comment by @MichaReiser on 2025-07-31 06:17_

Thank you

---

_Comment by @ntBre on 2025-07-31 20:17_

I think we just need to accept the new snaps here. Happy to push a commit if you want!

<details><summary>Patch</summary>
<p>

```diff
diff --git a/crates/ruff/tests/snapshots/integration_test__rule_f401.snap b/crates/ruff/tests/snapshots/integration_test__rule_f401.snap
index a861fe395c..20e77e7894 100644
--- a/crates/ruff/tests/snapshots/integration_test__rule_f401.snap
+++ b/crates/ruff/tests/snapshots/integration_test__rule_f401.snap
@@ -95,6 +95,6 @@ is stricter, which could affect the suggested fix. See [this FAQ section](https:
 ## References
 - [Python documentation: `import`](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)
 - [Python documentation: `importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec)
-- [Typing documentation: interface conventions](https://typing.python.org/en/latest/source/libraries.html#library-interface-public-and-private-symbols)
+- [Typing documentation: interface conventions](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols)
 
 ----- stderr -----
diff --git a/crates/ruff/tests/snapshots/lint__output_format_sarif.snap b/crates/ruff/tests/snapshots/lint__output_format_sarif.snap
index 664f8f0d8a..36f426c7ad 100644
--- a/crates/ruff/tests/snapshots/lint__output_format_sarif.snap
+++ b/crates/ruff/tests/snapshots/lint__output_format_sarif.snap
@@ -95,7 +95,7 @@ exit_code: 1
           "rules": [
             {
               "fullDescription": {
-                "text": "## What it does\nChecks for unused imports.\n\n## Why is this bad?\nUnused imports add a performance overhead at runtime, and risk creating\nimport cycles. They also increase the cognitive load of reading the code.\n\nIf an import statement is used to check for the availability or existence\nof a module, consider using `importlib.util.find_spec` instead.\n\nIf an import statement is used to re-export a symbol as part of a module's\npublic interface, consider using a \"redundant\" import alias, which\ninstructs Ruff (and other tools) to respect the re-export, and avoid\nmarking it as unused, as in:\n\n```python\nfrom module import member as member\n```\n\nAlternatively, you can use `__all__` to declare a symbol as part of the module's\ninterface, as in:\n\n```python\n# __init__.py\nimport some_module\n\n__all__ = [\"some_module\"]\n```\n\n## Fix safety\n\nFixes to remove unused imports are safe, except in `__init__.py` files.\n\nApplying fixes to `__init__.py` files is currently in preview. The fix offered depends on the\ntype of the unused import. Ruff will suggest a safe fix to export first-party imports with\neither a redundant alias or, if already present in the file, an `__all__` entry. If multiple\n`__all__` declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix\nto remove third-party and standard library imports -- the fix is unsafe because the module's\ninterface changes.\n\n## Example\n\n```python\nimport numpy as np  # unused import\n\n\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nUse instead:\n\n```python\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nTo check the availability of a module, use `importlib.util.find_spec`:\n\n```python\nfrom importlib.util import find_spec\n\nif find_spec(\"numpy\") is not None:\n    print(\"numpy is installed\")\nelse:\n    print(\"numpy is not installed\")\n```\n\n## Preview\nWhen [preview](https://docs.astral.sh/ruff/preview/) is enabled,\nthe criterion for determining whether an import is first-party\nis stricter, which could affect the suggested fix. See [this FAQ section](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) for more details.\n\n## Options\n- `lint.ignore-init-module-imports`\n- `lint.pyflakes.allowed-unused-imports`\n\n## References\n- [Python documentation: `import`](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)\n- [Python documentation: `importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec)\n- [Typing documentation: interface conventions](https://typing.python.org/en/latest/source/libraries.html#library-interface-public-and-private-symbols)\n"
+                "text": "## What it does\nChecks for unused imports.\n\n## Why is this bad?\nUnused imports add a performance overhead at runtime, and risk creating\nimport cycles. They also increase the cognitive load of reading the code.\n\nIf an import statement is used to check for the availability or existence\nof a module, consider using `importlib.util.find_spec` instead.\n\nIf an import statement is used to re-export a symbol as part of a module's\npublic interface, consider using a \"redundant\" import alias, which\ninstructs Ruff (and other tools) to respect the re-export, and avoid\nmarking it as unused, as in:\n\n```python\nfrom module import member as member\n```\n\nAlternatively, you can use `__all__` to declare a symbol as part of the module's\ninterface, as in:\n\n```python\n# __init__.py\nimport some_module\n\n__all__ = [\"some_module\"]\n```\n\n## Fix safety\n\nFixes to remove unused imports are safe, except in `__init__.py` files.\n\nApplying fixes to `__init__.py` files is currently in preview. The fix offered depends on the\ntype of the unused import. Ruff will suggest a safe fix to export first-party imports with\neither a redundant alias or, if already present in the file, an `__all__` entry. If multiple\n`__all__` declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix\nto remove third-party and standard library imports -- the fix is unsafe because the module's\ninterface changes.\n\n## Example\n\n```python\nimport numpy as np  # unused import\n\n\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nUse instead:\n\n```python\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nTo check the availability of a module, use `importlib.util.find_spec`:\n\n```python\nfrom importlib.util import find_spec\n\nif find_spec(\"numpy\") is not None:\n    print(\"numpy is installed\")\nelse:\n    print(\"numpy is not installed\")\n```\n\n## Preview\nWhen [preview](https://docs.astral.sh/ruff/preview/) is enabled,\nthe criterion for determining whether an import is first-party\nis stricter, which could affect the suggested fix. See [this FAQ section](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) for more details.\n\n## Options\n- `lint.ignore-init-module-imports`\n- `lint.pyflakes.allowed-unused-imports`\n\n## References\n- [Python documentation: `import`](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)\n- [Python documentation: `importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec)\n- [Typing documentation: interface conventions](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols)\n"
               },
               "help": {
                 "text": "`{name}` imported but unused; consider using `importlib.util.find_spec` to test for availability"
diff --git a/crates/ruff_linter/src/message/snapshots/ruff_linter__message__sarif__tests__results.snap b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__sarif__tests__results.snap
index 224cdf75ae..a4ddc029fb 100644
--- a/crates/ruff_linter/src/message/snapshots/ruff_linter__message__sarif__tests__results.snap
+++ b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__sarif__tests__results.snap
@@ -81,7 +81,7 @@ expression: value
           "rules": [
             {
               "fullDescription": {
-                "text": "## What it does\nChecks for unused imports.\n\n## Why is this bad?\nUnused imports add a performance overhead at runtime, and risk creating\nimport cycles. They also increase the cognitive load of reading the code.\n\nIf an import statement is used to check for the availability or existence\nof a module, consider using `importlib.util.find_spec` instead.\n\nIf an import statement is used to re-export a symbol as part of a module's\npublic interface, consider using a \"redundant\" import alias, which\ninstructs Ruff (and other tools) to respect the re-export, and avoid\nmarking it as unused, as in:\n\n```python\nfrom module import member as member\n```\n\nAlternatively, you can use `__all__` to declare a symbol as part of the module's\ninterface, as in:\n\n```python\n# __init__.py\nimport some_module\n\n__all__ = [\"some_module\"]\n```\n\n## Fix safety\n\nFixes to remove unused imports are safe, except in `__init__.py` files.\n\nApplying fixes to `__init__.py` files is currently in preview. The fix offered depends on the\ntype of the unused import. Ruff will suggest a safe fix to export first-party imports with\neither a redundant alias or, if already present in the file, an `__all__` entry. If multiple\n`__all__` declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix\nto remove third-party and standard library imports -- the fix is unsafe because the module's\ninterface changes.\n\n## Example\n\n```python\nimport numpy as np  # unused import\n\n\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nUse instead:\n\n```python\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nTo check the availability of a module, use `importlib.util.find_spec`:\n\n```python\nfrom importlib.util import find_spec\n\nif find_spec(\"numpy\") is not None:\n    print(\"numpy is installed\")\nelse:\n    print(\"numpy is not installed\")\n```\n\n## Preview\nWhen [preview](https://docs.astral.sh/ruff/preview/) is enabled,\nthe criterion for determining whether an import is first-party\nis stricter, which could affect the suggested fix. See [this FAQ section](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) for more details.\n\n## Options\n- `lint.ignore-init-module-imports`\n- `lint.pyflakes.allowed-unused-imports`\n\n## References\n- [Python documentation: `import`](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)\n- [Python documentation: `importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec)\n- [Typing documentation: interface conventions](https://typing.python.org/en/latest/source/libraries.html#library-interface-public-and-private-symbols)\n"
+                "text": "## What it does\nChecks for unused imports.\n\n## Why is this bad?\nUnused imports add a performance overhead at runtime, and risk creating\nimport cycles. They also increase the cognitive load of reading the code.\n\nIf an import statement is used to check for the availability or existence\nof a module, consider using `importlib.util.find_spec` instead.\n\nIf an import statement is used to re-export a symbol as part of a module's\npublic interface, consider using a \"redundant\" import alias, which\ninstructs Ruff (and other tools) to respect the re-export, and avoid\nmarking it as unused, as in:\n\n```python\nfrom module import member as member\n```\n\nAlternatively, you can use `__all__` to declare a symbol as part of the module's\ninterface, as in:\n\n```python\n# __init__.py\nimport some_module\n\n__all__ = [\"some_module\"]\n```\n\n## Fix safety\n\nFixes to remove unused imports are safe, except in `__init__.py` files.\n\nApplying fixes to `__init__.py` files is currently in preview. The fix offered depends on the\ntype of the unused import. Ruff will suggest a safe fix to export first-party imports with\neither a redundant alias or, if already present in the file, an `__all__` entry. If multiple\n`__all__` declarations are present, Ruff will not offer a fix. Ruff will suggest an unsafe fix\nto remove third-party and standard library imports -- the fix is unsafe because the module's\ninterface changes.\n\n## Example\n\n```python\nimport numpy as np  # unused import\n\n\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nUse instead:\n\n```python\ndef area(radius):\n    return 3.14 * radius**2\n```\n\nTo check the availability of a module, use `importlib.util.find_spec`:\n\n```python\nfrom importlib.util import find_spec\n\nif find_spec(\"numpy\") is not None:\n    print(\"numpy is installed\")\nelse:\n    print(\"numpy is not installed\")\n```\n\n## Preview\nWhen [preview](https://docs.astral.sh/ruff/preview/) is enabled,\nthe criterion for determining whether an import is first-party\nis stricter, which could affect the suggested fix. See [this FAQ section](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) for more details.\n\n## Options\n- `lint.ignore-init-module-imports`\n- `lint.pyflakes.allowed-unused-imports`\n\n## References\n- [Python documentation: `import`](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)\n- [Python documentation: `importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec)\n- [Typing documentation: interface conventions](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols)\n"
               },
               "help": {
                 "text": "`{name}` imported but unused; consider using `importlib.util.find_spec` to test for availability"
```

</p>
</details> 

---

_Merged by @MichaReiser on 2025-08-01 08:47_

---

_Closed by @MichaReiser on 2025-08-01 08:47_

---

_Comment by @github-actions[bot] on 2025-08-01 08:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-08-01 10:10_

---

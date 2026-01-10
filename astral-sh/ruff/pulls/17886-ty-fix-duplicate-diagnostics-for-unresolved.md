```yaml
number: 17886
title: "[ty] Fix duplicate diagnostics for unresolved module when an `import from` statement imports multiple members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/duplicate-import-diags
created_at: 2025-05-06T11:11:58Z
updated_at: 2025-05-06T11:37:11Z
url: https://github.com/astral-sh/ruff/pull/17886
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Fix duplicate diagnostics for unresolved module when an `import from` statement imports multiple members

---

_Pull request opened by @AlexWaygood on 2025-05-06 11:11_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/17876. Rather than emitting a separate diagnostic for the unresolved module on each `ast::Alias` in the `ImportFrom` statement, we only emit a single diagnostic for the statement as a whole.

## Test Plan

new mdtests and snapshots


---

_Label `ty` added by @AlexWaygood on 2025-05-06 11:11_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-06 11:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-06 11:11_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-06 11:11_

---

_Comment by @github-actions[bot] on 2025-05-06 11:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:unresolved-import] src/packaging/_manylinux.py:187:16: Cannot resolve import `_manylinux`
+ error[lint:unresolved-import] src/packaging/_manylinux.py:187:16: Cannot resolve imported module `_manylinux`

com2ann (https://github.com/ilevkivskyi/com2ann)
- error[lint:unresolved-import] src/test_cli.py:8:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] src/test_cli.py:8:8: Cannot resolve imported module `pytest`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[lint:unresolved-import] pytest_robotframework/__init__.py:19:6: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:19:6: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:19:6: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:19:6: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:20:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:20:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:21:6: Cannot resolve import `robot`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:21:6: Cannot resolve imported module `robot`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:21:6: Cannot resolve import `robot`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:22:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:22:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:22:6: Cannot resolve import `robot.api`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:23:6: Cannot resolve import `robot.errors`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:23:6: Cannot resolve imported module `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:23:6: Cannot resolve import `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:24:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:24:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:25:6: Cannot resolve import `robot.model.visitor`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:25:6: Cannot resolve imported module `robot.model.visitor`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:26:6: Cannot resolve import `robot.running`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:26:6: Cannot resolve imported module `robot.running`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:27:6: Cannot resolve import `robot.running.context`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:27:6: Cannot resolve imported module `robot.running.context`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:28:6: Cannot resolve import `robot.running.librarykeywordrunner`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:28:6: Cannot resolve imported module `robot.running.librarykeywordrunner`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:29:6: Cannot resolve import `robot.running.statusreporter`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:29:6: Cannot resolve imported module `robot.running.statusreporter`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:29:6: Cannot resolve import `robot.running.statusreporter`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:29:6: Cannot resolve import `robot.running.statusreporter`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:30:6: Cannot resolve import `robot.utils`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:30:6: Cannot resolve imported module `robot.utils`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:30:6: Cannot resolve import `robot.utils`
- error[lint:unresolved-import] pytest_robotframework/__init__.py:31:6: Cannot resolve import `robot.utils.error`
+ error[lint:unresolved-import] pytest_robotframework/__init__.py:31:6: Cannot resolve imported module `robot.utils.error`
- error[lint:unresolved-import] pytest_robotframework/_internal/cringe_globals.py:13:10: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/cringe_globals.py:13:10: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/cringe_globals.py:13:10: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/exception_getter.py:8:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/exception_getter.py:8:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:11:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:11:8: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:12:6: Cannot resolve import `_pytest.assertion`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:12:6: Cannot resolve imported module `_pytest.assertion`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:13:6: Cannot resolve import `_pytest.assertion.rewrite`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:13:6: Cannot resolve imported module `_pytest.assertion.rewrite`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:13:6: Cannot resolve import `_pytest.assertion.rewrite`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:13:6: Cannot resolve import `_pytest.assertion.rewrite`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:18:6: Cannot resolve import `_pytest.main`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:18:6: Cannot resolve imported module `_pytest.main`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:29:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:29:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:30:6: Cannot resolve import `robot.conf.settings`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:30:6: Cannot resolve imported module `robot.conf.settings`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:30:6: Cannot resolve import `robot.conf.settings`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:34:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:34:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:35:6: Cannot resolve import `robot.output`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:35:6: Cannot resolve imported module `robot.output`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:36:6: Cannot resolve import `robot.rebot`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:36:6: Cannot resolve imported module `robot.rebot`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:37:6: Cannot resolve import `robot.result.resultbuilder`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:37:6: Cannot resolve imported module `robot.result.resultbuilder`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:38:6: Cannot resolve import `robot.run`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:38:6: Cannot resolve imported module `robot.run`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:38:6: Cannot resolve import `robot.run`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:39:6: Cannot resolve import `robot.utils.error`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:39:6: Cannot resolve imported module `robot.utils.error`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:89:10: Cannot resolve import `_pytest.terminal`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:89:10: Cannot resolve imported module `_pytest.terminal`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:90:10: Cannot resolve import `pluggy`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:90:10: Cannot resolve imported module `pluggy`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:91:10: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:91:10: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:91:10: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:91:10: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/plugin.py:91:10: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:9:6: Cannot resolve import `_pytest._code.code`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:9:6: Cannot resolve imported module `_pytest._code.code`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:9:6: Cannot resolve import `_pytest._code.code`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:11:6: Cannot resolve import `robot`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:11:6: Cannot resolve imported module `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:12:6: Cannot resolve import `robot.errors`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:12:6: Cannot resolve imported module `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:12:6: Cannot resolve import `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:12:6: Cannot resolve import `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:13:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:13:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:14:6: Cannot resolve import `robot.running.bodyrunner`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:14:6: Cannot resolve imported module `robot.running.bodyrunner`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:15:6: Cannot resolve import `robot.running.model`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:15:6: Cannot resolve imported module `robot.running.model`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:16:6: Cannot resolve import `robot.running.statusreporter`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:16:6: Cannot resolve imported module `robot.running.statusreporter`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:35:10: Cannot resolve import `_pytest._code.code`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:35:10: Cannot resolve imported module `_pytest._code.code`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:36:10: Cannot resolve import `_pytest._io`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:36:10: Cannot resolve imported module `_pytest._io`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:37:10: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/robot_file_support.py:37:10: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/xdist_utils.py:6:10: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/xdist_utils.py:6:10: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/pytest/xdist_utils.py:12:16: Cannot resolve import `xdist`
+ error[lint:unresolved-import] pytest_robotframework/_internal/pytest/xdist_utils.py:12:16: Cannot resolve imported module `xdist`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:10:6: Cannot resolve import `_pytest._code.code`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:10:6: Cannot resolve imported module `_pytest._code.code`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:11:6: Cannot resolve import `_pytest.runner`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:11:6: Cannot resolve imported module `_pytest.runner`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:11:6: Cannot resolve import `_pytest.runner`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:15:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:15:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:16:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/library.py:16:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:16:6: Cannot resolve import `_pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:16:6: Cannot resolve imported module `_pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:17:6: Cannot resolve import `_pytest.python`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:17:6: Cannot resolve imported module `_pytest.python`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:18:6: Cannot resolve import `ansi2html`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:18:6: Cannot resolve imported module `ansi2html`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:19:6: Cannot resolve import `pluggy`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:19:6: Cannot resolve imported module `pluggy`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:19:6: Cannot resolve import `pluggy`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:20:6: Cannot resolve import `pluggy._hooks`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:20:6: Cannot resolve imported module `pluggy._hooks`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:21:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:22:6: Cannot resolve import `robot`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:22:6: Cannot resolve imported module `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:22:6: Cannot resolve import `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:22:6: Cannot resolve import `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:23:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:23:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:23:6: Cannot resolve import `robot.api.interfaces`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:24:6: Cannot resolve import `robot.errors`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:24:6: Cannot resolve imported module `robot.errors`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:25:6: Cannot resolve import `robot.model`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:25:6: Cannot resolve imported module `robot.model`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:25:6: Cannot resolve import `robot.model`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:26:6: Cannot resolve import `robot.running.librarykeywordrunner`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:26:6: Cannot resolve imported module `robot.running.librarykeywordrunner`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:27:6: Cannot resolve import `robot.running.model`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:27:6: Cannot resolve imported module `robot.running.model`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:28:6: Cannot resolve import `robot.utils.error`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:28:6: Cannot resolve imported module `robot.utils.error`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:65:10: Cannot resolve import `_pytest.nodes`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:65:10: Cannot resolve imported module `_pytest.nodes`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:66:10: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:66:10: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:66:10: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:66:10: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:67:10: Cannot resolve import `robot.running.builder.settings`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:67:10: Cannot resolve imported module `robot.running.builder.settings`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:68:10: Cannot resolve import `robot.running.context`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:68:10: Cannot resolve imported module `robot.running.context`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:648:10: Cannot resolve import `robot.running.librarykeyword`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:648:10: Cannot resolve imported module `robot.running.librarykeyword`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:649:10: Cannot resolve import `robot.running.testlibraries`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:649:10: Cannot resolve imported module `robot.running.testlibraries`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:18:6: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:18:6: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:19:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:19:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:19:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:20:6: Cannot resolve import `robot`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:20:6: Cannot resolve imported module `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:20:6: Cannot resolve import `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:21:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:21:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:21:6: Cannot resolve import `robot.api.interfaces`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:21:6: Cannot resolve import `robot.api.interfaces`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:22:6: Cannot resolve import `robot.conf.settings`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:22:6: Cannot resolve imported module `robot.conf.settings`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:22:6: Cannot resolve import `robot.conf.settings`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:23:6: Cannot resolve import `robot.running.context`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:23:6: Cannot resolve imported module `robot.running.context`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:24:6: Cannot resolve import `robot.version`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:24:6: Cannot resolve imported module `robot.version`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:171:10: Cannot resolve import `robot.running`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:171:10: Cannot resolve imported module `robot.running`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:289:12: Cannot resolve import `_pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:289:12: Cannot resolve imported module `_pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:290:12: Cannot resolve import `pluggy`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:290:12: Cannot resolve imported module `pluggy`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:291:12: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:291:12: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:292:12: Cannot resolve import `robot`
+ error[lint:unresolved-import] pytest_robotframework/_internal/robot/utils.py:292:12: Cannot resolve imported module `robot`
- error[lint:unresolved-import] pytest_robotframework/_internal/utils.py:6:6: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] pytest_robotframework/_internal/utils.py:6:6: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/_internal/utils.py:6:6: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] pytest_robotframework/hooks.py:13:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/hooks.py:13:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] pytest_robotframework/hooks.py:13:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] pytest_robotframework/hooks.py:16:10: Cannot resolve import `pytest`
+ error[lint:unresolved-import] pytest_robotframework/hooks.py:16:10: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/conftest.py:11:6: Cannot resolve import `lxml.etree`
+ error[lint:unresolved-import] tests/conftest.py:11:6: Cannot resolve imported module `lxml.etree`
- error[lint:unresolved-import] tests/conftest.py:11:6: Cannot resolve import `lxml.etree`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/conftest.py:15:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/conftest.py:20:10: Cannot resolve import `lxml.etree`
+ error[lint:unresolved-import] tests/conftest.py:20:10: Cannot resolve imported module `lxml.etree`
- error[lint:unresolved-import] tests/conftest.py:20:10: Cannot resolve import `lxml.etree`
- error[lint:unresolved-import] tests/conftest.py:20:10: Cannot resolve import `lxml.etree`
- error[lint:unresolved-import] tests/fixtures/test_python/test_as_keyword_context_manager_try_except.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_as_keyword_context_manager_try_except.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_assertion_passes_custom_messages.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_assertion_passes_custom_messages.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator/conftest.py:5:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator/conftest.py:5:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator/conftest.py:11:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator/conftest.py:11:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator/conftest.py:11:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator_with_non_instance_method/conftest.py:5:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator_with_non_instance_method/conftest.py:5:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator_with_non_instance_method/conftest.py:11:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator_with_non_instance_method/conftest.py:11:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_catch_errors_decorator_with_non_instance_method/conftest.py:11:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_console_output.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_console_output.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_and_second_test.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_and_second_test.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_exitonerror.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_exitonerror.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_exitonerror_multiple_tests.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_exitonerror_multiple_tests.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_setup/conftest.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_setup/conftest.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_setup/test_foo.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_setup/test_foo.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_teardown/conftest.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_teardown/conftest.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_teardown/test_foo.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_error_moment_teardown/test_foo.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_fixture.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_fixture.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_fixture_class_scope.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_fixture_class_scope.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_fixture_class_scope.py:3:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_fixture_module_scope/conftest.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_fixture_module_scope/conftest.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_and_pytest_raises.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_and_pytest_raises.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_doesnt_suppress.py:6:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_doesnt_suppress.py:6:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_body_and_exit.py:6:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_body_and_exit.py:6:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_exit.py:6:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_exit.py:6:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_returns_context_manager_that_isnt_used.py:6:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_returns_context_manager_that_isnt_used.py:6:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_try_except.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_decorator_try_except.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_raises.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keyword_raises.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_context_manager.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_context_manager.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_function.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_function.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_keyword_inside_context_manager.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_keyword_inside_context_manager.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_keyword_inside_context_manager.py:4:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_keywordify_keyword_inside_context_manager.py:4:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_listener_calls_log_file/Listener.py:7:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_listener_calls_log_file/Listener.py:7:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_listener_not_run_during_collection/conftest.py:5:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_listener_not_run_during_collection/conftest.py:5:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_listener_not_run_during_collection/conftest.py:9:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_listener_not_run_during_collection/conftest.py:9:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_listener_not_run_during_collection/conftest.py:9:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_one_test_skipped.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_one_test_skipped.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_parameterized_tags.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_parameterized_tags.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_parametrize.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_parametrize.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_hook_in_different_suite/a/test_c.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_hook_in_different_suite/a/test_c.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_item_hook/foo/conftest.py:5:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_item_hook/foo/conftest.py:5:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_item_hook/foo/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_item_hook/foo/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_item_hook/foo/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_session_hook/conftest.py:5:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_session_hook/conftest.py:5:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_session_hook/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_session_hook/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_pytest_runtest_protocol_session_hook/conftest.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_keyword_in_python_test/test_foo.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_keyword_in_python_test/test_foo.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_args_hook/test_foo.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_args_hook/test_foo.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_args_hook_collect_only/test_foo.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_args_hook_collect_only/test_foo.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook/test_foo.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook/test_foo.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook_listener_instance/conftest.py:5:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook_listener_instance/conftest.py:5:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook_listener_instance/conftest.py:9:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook_listener_instance/conftest.py:9:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_modify_options_hook_listener_instance/conftest.py:9:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_merge_listeners/Listener.py:7:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_merge_listeners/Listener.py:7:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_merge_listeners/Listener.py:11:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_merge_listeners/Listener.py:11:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_merge_listeners/Listener.py:11:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_variable_merge_listeners/Listener.py:7:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_variable_merge_listeners/Listener.py:7:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_variable_merge_listeners/Listener.py:11:10: Cannot resolve import `robot`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_variable_merge_listeners/Listener.py:11:10: Cannot resolve imported module `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_robot_options_variable_merge_listeners/Listener.py:11:10: Cannot resolve import `robot`
- error[lint:unresolved-import] tests/fixtures/test_python/test_set_log_level.py:3:6: Cannot resolve import `robot.api.logger`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_set_log_level.py:3:6: Cannot resolve imported module `robot.api.logger`
- error[lint:unresolved-import] tests/fixtures/test_python/test_set_log_level.py:4:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_set_log_level.py:4:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_setup_passes/conftest.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_setup_passes/conftest.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_setup_passes/test_foo.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_setup_passes/test_foo.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_setup_skipped/conftest.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_setup_skipped/conftest.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_suite_variables.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_suite_variables.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_suite_variables_with_slash.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_suite_variables_with_slash.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_tags.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_tags.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_passes/conftest.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_passes/conftest.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_passes/test_foo.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_passes/test_foo.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_skipped/conftest.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_teardown_skipped/conftest.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_variables_list.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_variables_list.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_variables_not_in_scope_in_other_suites/test_one.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_variables_not_in_scope_in_other_suites/test_one.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_variables_not_in_scope_in_other_suites/test_two.py:3:6: Cannot resolve import `robot.libraries.BuiltIn`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_variables_not_in_scope_in_other_suites/test_two.py:3:6: Cannot resolve imported module `robot.libraries.BuiltIn`
- error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_fails/test_foo.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_fails/test_foo.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_fails_no_reason/test_foo.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_fails_no_reason/test_foo.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_passes/test_foo.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_passes/test_foo.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_passes_no_reason/test_foo.py:3:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/fixtures/test_python/test_xfail_passes_no_reason/test_foo.py:3:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator/foo.py:3:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator/foo.py:3:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:6:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:6:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:11:10: Cannot resolve import `basedtyping`
+ error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:11:10: Cannot resolve imported module `basedtyping`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_and_other_decorator/foo.py:11:10: Cannot resolve import `basedtyping`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_class_library/ClassLibrary.py:5:6: Cannot resolve import `robot.api`
+ error[lint:unresolved-import] tests/fixtures/test_robot/test_keyword_decorator_class_library/ClassLibrary.py:5:6: Cannot resolve imported module `robot.api`
- error[lint:unresolved-import] tests/fixtures/test_robot/test_listener_calls_log_file/Listener.py:7:6: Cannot resolve import `robot.api.interfaces`
+ error[lint:unresolved-import] tests/fixtures/test_robot/test_listener_calls_log_file/Listener.py:7:6: Cannot resolve imported module `robot.api.interfaces`
- error[lint:unresolved-import] tests/test_common.py:5:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/test_common.py:5:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/test_common.py:5:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/test_common.py:6:6: Cannot resolve import `robot.conf.settings`
+ error[lint:unresolved-import] tests/test_common.py:6:6: Cannot resolve imported module `robot.conf.settings`
- error[lint:unresolved-import] tests/test_python.py:9:6: Cannot resolve import `_pytest.assertion.util`
+ error[lint:unresolved-import] tests/test_python.py:9:6: Cannot resolve imported module `_pytest.assertion.util`
- error[lint:unresolved-import] tests/test_python.py:10:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/test_python.py:10:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/test_python.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/test_python.py:10:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/test_robot.py:6:6: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/test_robot.py:6:6: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] tests/test_robot.py:6:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/test_robot.py:6:6: Cannot resolve import `pytest`
- error[lint:unresolved-import] tests/test_robot.py:12:10: Cannot resolve import `pytest`
+ error[lint:unresolved-import] tests/test_robot.py:12:10: Cannot resolve imported module `pytest`
- Found 349 diagnostics
+ Found 265 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[lint:unresolved-import] more_itertools/__init__.py:3:7: Cannot resolve import `.more`
+ error[lint:unresolved-import] more_itertools/__init__.py:3:7: Cannot resolve imported module `.more`
- error[lint:unresolved-import] more_itertools/__init__.py:4:7: Cannot resolve import `.recipes`
+ error[lint:unresolved-import] more_itertools/__init__.py:4:7: Cannot resolve imported module `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
+ error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve imported module `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- error[lint:unresolved-import] more_itertools/more.py:32:7: Cannot resolve import `.recipes`
- Found 63 diagnostics
+ Found 52 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:unresolved-import] pyinstrument/magic/magic.py:10:8: Cannot resolve import `IPython`
+ error[lint:unresolved-import] pyinstrument/magic/magic.py:10:8: Cannot resolve imported module `IPython`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:12:6: Cannot resolve import `IPython.core.magic`
+ error[lint:unresolved-import] pyinstrument/magic/magic.py:12:6: Cannot resolve imported module `IPython.core.magic`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:12:6: Cannot resolve import `IPython.core.magic`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:12:6: Cannot resolve import `IPython.core.magic`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:12:6: Cannot resolve import `IPython.core.magic`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:13:6: Cannot resolve import `IPython.core.magic_arguments`
+ error[lint:unresolved-import] pyinstrument/magic/magic.py:13:6: Cannot resolve imported module `IPython.core.magic_arguments`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:13:6: Cannot resolve import `IPython.core.magic_arguments`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:13:6: Cannot resolve import `IPython.core.magic_arguments`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:14:6: Cannot resolve import `IPython.display`
+ error[lint:unresolved-import] pyinstrument/magic/magic.py:14:6: Cannot resolve imported module `IPython.display`
- error[lint:unresolved-import] pyinstrument/magic/magic.py:14:6: Cannot resolve import `IPython.display`
- error[lint:unresolved-import] pyinstrument/middleware.py:6:6: Cannot resolve import `django.conf`
+ error[lint:unresolved-import] pyinstrument/middleware.py:6:6: Cannot resolve imported module `django.conf`
- error[lint:unresolved-import] pyinstrument/middleware.py:7:6: Cannot resolve import `django.http`
+ error[lint:unresolved-import] pyinstrument/middleware.py:7:6: Cannot resolve imported module `django.http`
- error[lint:unresolved-import] pyinstrument/middleware.py:8:6: Cannot resolve import `django.utils.module_loading`
+ error[lint:unresolved-import] pyinstrument/middleware.py:8:6: Cannot resolve imported module `django.utils.module_loading`
- error[lint:unresolved-import] pyinstrument/middleware.py:15:10: Cannot resolve import `django.utils.deprecation`
+ error[lint:unresolved-import] pyinstrument/middleware.py:15:10: Cannot resolve imported module `django.utils.deprecation`
- error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:489:14: Cannot resolve import `_winreg`
+ error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:489:14: Cannot resolve imported module `_winreg`
- error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:533:10: Cannot resolve import `com.sun`
+ error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:533:10: Cannot resolve imported module `com.sun`
- error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:534:10: Cannot resolve import `com.sun.jna.platform`
+ error[lint:unresolved-import] pyinstrument/vendor/appdirs.py:534:10: Cannot resolve imported module `com.sun.jna.platform`
- Found 104 diagnostics
+ Found 98 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:unresolved-import] beartype/_check/convert/_reduce/_nonpep/api/redapinumpy.py:100:10: Cannot resolve import `numpy`
+ error[lint:unresolved-import] beartype/_check/convert/_reduce/_nonpep/api/redapinumpy.py:100:10: Cannot resolve imported module `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilclick.py:89:10: Cannot resolve import `click.core`
+ error[lint:unresolved-import] beartype/_util/api/external/utilclick.py:89:10: Cannot resolve imported module `click.core`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
+ error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve imported module `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:55:10: Cannot resolve import `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:101:10: Cannot resolve import `numpy`
+ error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:101:10: Cannot resolve imported module `numpy`
- error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:237:10: Cannot resolve import `numpy`
+ error[lint:unresolved-import] beartype/_util/api/external/utilnumpy.py:237:10: Cannot resolve imported module `numpy`
- error[lint:unresolved-import] beartype/door/_func/infer/kind/inferthirdparty.py:124:10: Cannot resolve import `numpy`
+ error[lint:unresolved-import] beartype/door/_func/infer/kind/inferthirdparty.py:124:10: Cannot resolve imported module `numpy`
- Found 575 diagnostics
+ Found 566 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[lint:unresolved-import] bench/test_benchmarks.py:7:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] bench/test_benchmarks.py:7:8: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] conftest.py:5:8: Cannot resolve import `pytest`
+ error[lint:unresolved-import] conftest.py:5:8: Cannot resolve imported module `pytest`
- error[lint:unresolved-import] conftest.py:7:6: Cannot resolve import `hypothesis`
+ error[lint:unresolved-import] conftest.py:7:6: Cannot resolve imported module `hypothesis`
- error[lint:unresolved-import] conftest.py:7:6: Cannot resolve import `hypothesis`
- error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve import `.`
+ error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve imported module `.`
- error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve import `.`
- error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve import `.`
- error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve import `.`
- error[lint:unresolved-import] src/attr/__init__.py:10:1: Cannot resolve import `.`
- error[lint:unresolved-import] src/attr/__init__.py:11:7: Cannot resolve import `._cmp`
+ error[lint:unresolved-import] src/attr/__init__.py:11:7: Cannot resolve imported module `._cmp`
- error[lint:unresolved-import] src/attr/__init__.py:12:7: Cannot resolve import `._config`
+ error[lint:unresolved-import] src/attr/__init__.py:12:7: Cannot resolve imported module `._config`
- error[lint:unresolved-import] src/attr/__init__.py:12:7: Cannot resolve import `._config`
- error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve import `._funcs`
+ error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve imported module `._funcs`
- error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve import `._funcs`
- error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve import `._funcs`
- error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve import `._funcs`
- error[lint:unresolved-import] src/attr/__init__.py:13:7: Cannot resolve import `._funcs`
- error[lint:unresolved-import] src/attr/__init__.py:14:7: Cannot resolve import `._make`
+ error[lint:unresolved-import] ...*[Comment body truncated]*

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-06 11:28_

---

_@sharkdp approved on 2025-05-06 11:33_

Thank you!

---

_Merged by @AlexWaygood on 2025-05-06 11:37_

---

_Closed by @AlexWaygood on 2025-05-06 11:37_

---

_Branch deleted on 2025-05-06 11:37_

---

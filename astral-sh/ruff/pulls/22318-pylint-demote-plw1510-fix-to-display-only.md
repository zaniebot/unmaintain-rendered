```yaml
number: 22318
title: "[`pylint`] Demote `PLW1510` fix to display-only"
type: pull_request
state: merged
author: ntBre
labels:
  - fixes
assignees: []
merged: true
base: main
head: brent/plw1510-fix
created_at: 2025-12-31T20:43:21Z
updated_at: 2026-01-06T00:36:18Z
url: https://github.com/astral-sh/ruff/pull/22318
synced_at: 2026-01-10T16:30:32Z
```

# [`pylint`] Demote `PLW1510` fix to display-only

---

_Pull request opened by @ntBre on 2025-12-31 20:43_

Summary
--

Closes #17091. `PLW1510` checks for `subprocess.run` calls without a `check`
keyword argument and previously had a safe fix to add `check=False`. That's the
default value, so technically it preserved the code's behavior, but as discussed
in #17091 and #17087, Ruff can't actually know what the author intended.

I don't think it hurts to keep this as a display-only fix instead of removing it
entirely, but it definitely shouldn't be safe at the very least.

Test Plan
--

Existing tests


---

_@ntBre reviewed on 2025-12-31 20:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/subprocess_run_without_check.rs`:58 on 2025-12-31 20:44_

This felt worth a comment here, but I think this is pretty reasonable in general. These fixes aren't usually shown to users. We could of course update the test code instead:

https://github.com/astral-sh/ruff/blob/8cd04df49d15869e9c1ce82ecc882ece0c7eb0d7/crates/ruff_linter/src/test.rs#L354-L359

---

_Label `fixes` added by @ntBre on 2025-12-31 20:44_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 20:51_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -46 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L31'>RELEASING/verify_release.py:31:14:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L31'>RELEASING/verify_release.py:31:14:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L62'>RELEASING/verify_release.py:62:14:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L62'>RELEASING/verify_release.py:62:14:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L108'>superset-extensions-cli/src/superset_extensions_cli/cli.py:108:15:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L108'>superset-extensions-cli/src/superset_extensions_cli/cli.py:108:15:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L174'>superset-extensions-cli/src/superset_extensions_cli/cli.py:174:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L174'>superset-extensions-cli/src/superset_extensions_cli/cli.py:174:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L58'>superset-extensions-cli/src/superset_extensions_cli/cli.py:58:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L58'>superset-extensions-cli/src/superset_extensions_cli/cli.py:58:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/unit_tests/fixtures/bash_mock.py#L25'>tests/unit_tests/fixtures/bash_mock.py:25:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/unit_tests/fixtures/bash_mock.py#L25'>tests/unit_tests/fixtures/bash_mock.py:25:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -30 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:5:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_isort.py#L58'>tests/codebase/test_isort.py:58:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_isort.py#L58'>tests/codebase/test_isort.py:58:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_js_license_set.py#L50'>tests/codebase/test_js_license_set.py:50:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_js_license_set.py#L50'>tests/codebase/test_js_license_set.py:50:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_license.py#L40'>tests/codebase/test_license.py:40:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_license.py#L40'>tests/codebase/test_license.py:40:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L48'>tests/codebase/test_no_client_server_common.py:48:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L48'>tests/codebase/test_no_client_server_common.py:48:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L57'>tests/codebase/test_no_client_server_common.py:57:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L57'>tests/codebase/test_no_client_server_common.py:57:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_tornado_common.py#L55'>tests/codebase/test_no_tornado_common.py:55:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_tornado_common.py#L55'>tests/codebase/test_no_tornado_common.py:55:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_vermin.py#L39'>tests/codebase/test_vermin.py:39:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_vermin.py#L39'>tests/codebase/test_vermin.py:39:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/project.py#L46'>tests/support/util/project.py:46:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/project.py#L46'>tests/support/util/project.py:46:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L337'>src/latch_cli/centromere/ctx.py:337:43:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L337'>src/latch_cli/centromere/ctx.py:337:43:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L342'>src/latch_cli/centromere/ctx.py:342:43:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L342'>src/latch_cli/centromere/ctx.py:342:43:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1510 | 46 | 0 | 0 | 0 | 46 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -46 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L31'>RELEASING/verify_release.py:31:14:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L31'>RELEASING/verify_release.py:31:14:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L62'>RELEASING/verify_release.py:62:14:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/RELEASING/verify_release.py#L62'>RELEASING/verify_release.py:62:14:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L108'>superset-extensions-cli/src/superset_extensions_cli/cli.py:108:15:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L108'>superset-extensions-cli/src/superset_extensions_cli/cli.py:108:15:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L174'>superset-extensions-cli/src/superset_extensions_cli/cli.py:174:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L174'>superset-extensions-cli/src/superset_extensions_cli/cli.py:174:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L58'>superset-extensions-cli/src/superset_extensions_cli/cli.py:58:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/superset-extensions-cli/src/superset_extensions_cli/cli.py#L58'>superset-extensions-cli/src/superset_extensions_cli/cli.py:58:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/unit_tests/fixtures/bash_mock.py#L25'>tests/unit_tests/fixtures/bash_mock.py:25:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/superset/blob/c31224c891d3269893d3724c62777ab6557d598d/tests/unit_tests/fixtures/bash_mock.py#L25'>tests/unit_tests/fixtures/bash_mock.py:25:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -30 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L43'>release/system.py:43:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:5:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_isort.py#L58'>tests/codebase/test_isort.py:58:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_isort.py#L58'>tests/codebase/test_isort.py:58:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_js_license_set.py#L50'>tests/codebase/test_js_license_set.py:50:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_js_license_set.py#L50'>tests/codebase/test_js_license_set.py:50:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_license.py#L40'>tests/codebase/test_license.py:40:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_license.py#L40'>tests/codebase/test_license.py:40:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L48'>tests/codebase/test_no_client_server_common.py:48:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L48'>tests/codebase/test_no_client_server_common.py:48:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L57'>tests/codebase/test_no_client_server_common.py:57:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_client_server_common.py#L57'>tests/codebase/test_no_client_server_common.py:57:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_tornado_common.py#L55'>tests/codebase/test_no_tornado_common.py:55:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_tornado_common.py#L55'>tests/codebase/test_no_tornado_common.py:55:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_vermin.py#L39'>tests/codebase/test_vermin.py:39:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_vermin.py#L39'>tests/codebase/test_vermin.py:39:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/project.py#L46'>tests/support/util/project.py:46:12:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/project.py#L46'>tests/support/util/project.py:46:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L337'>src/latch_cli/centromere/ctx.py:337:43:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L337'>src/latch_cli/centromere/ctx.py:337:43:</a> PLW1510 `subprocess.run` without explicit `check` argument
- <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L342'>src/latch_cli/centromere/ctx.py:342:43:</a> PLW1510 [*] `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/centromere/ctx.py#L342'>src/latch_cli/centromere/ctx.py:342:43:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1510 | 46 | 0 | 0 | 0 | 46 |

</p>
</details>





---

_Marked ready for review by @ntBre on 2025-12-31 21:13_

---

_@MichaReiser approved on 2026-01-05 10:29_

---

_Merged by @ntBre on 2026-01-06 00:36_

---

_Closed by @ntBre on 2026-01-06 00:36_

---

_Branch deleted on 2026-01-06 00:36_

---

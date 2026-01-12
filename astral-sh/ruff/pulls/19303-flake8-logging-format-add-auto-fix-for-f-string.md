```yaml
number: 19303
title: "[`flake8-logging-format`] Add auto-fix for f-string logging calls (`G004`)"
type: pull_request
state: merged
author: hamirmahal
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/add-auto-fix-for-f-string-logging-statements-g004
created_at: 2025-07-12T22:46:34Z
updated_at: 2025-08-27T22:58:00Z
url: https://github.com/astral-sh/ruff/pull/19303
synced_at: 2026-01-12T15:56:36Z
```

# [`flake8-logging-format`] Add auto-fix for f-string logging calls (`G004`)

---

_@hamirmahal_

Closes #19302

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This adds an auto-fix for `Logging statement uses f-string` Ruff G004, so users don't have to resolve it manually.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I ran the auto-fixes on a Python file locally and and it worked as expected.
<!-- How was it tested? -->

---

_Comment by @github-actions[bot] on 2025-07-12 22:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +138 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/8cd100bd01853e7dc95dcf410cd85dda3baa95a4/tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py#L37'>tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py:37:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/8cd100bd01853e7dc95dcf410cd85dda3baa95a4/tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py#L37'>tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py:37:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/8cd100bd01853e7dc95dcf410cd85dda3baa95a4/tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py#L39'>tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py:39:17:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/8cd100bd01853e7dc95dcf410cd85dda3baa95a4/tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py#L39'>tests_integration/tests_using_container_services/spcs/docker/test_counter/simple_counter.py:39:17:</a> G004 [*] Logging statement uses f-string
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +120 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/app.py#L103'>superset/app.py:103:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/app.py#L103'>superset/app.py:103:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/cli/examples.py#L38'>superset/cli/examples.py:38:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/cli/examples.py#L38'>superset/cli/examples.py:38:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/database/uploaders/csv_reader.py#L122'>superset/commands/database/uploaders/csv_reader.py:122:17:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/database/uploaders/csv_reader.py#L122'>superset/commands/database/uploaders/csv_reader.py:122:17:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/theme/seed.py#L104'>superset/commands/theme/seed.py:104:26:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/theme/seed.py#L104'>superset/commands/theme/seed.py:104:26:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/theme/seed.py#L96'>superset/commands/theme/seed.py:96:26:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/commands/theme/seed.py#L96'>superset/commands/theme/seed.py:96:26:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/bart_lines.py#L62'>superset/examples/bart_lines.py:62:18:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/bart_lines.py#L62'>superset/examples/bart_lines.py:62:18:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/birth_names.py#L111'>superset/examples/birth_names.py:111:22:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/birth_names.py#L111'>superset/examples/birth_names.py:111:22:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/multiformat_time_series.py#L87'>superset/examples/multiformat_time_series.py:87:18:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/multiformat_time_series.py#L87'>superset/examples/multiformat_time_series.py:87:18:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/paris.py#L58'>superset/examples/paris.py:58:18:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/paris.py#L58'>superset/examples/paris.py:58:18:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/random_time_series.py#L71'>superset/examples/random_time_series.py:71:18:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/random_time_series.py#L71'>superset/examples/random_time_series.py:71:18:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/sf_population_polygons.py#L62'>superset/examples/sf_population_polygons.py:62:18:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/examples/sf_population_polygons.py#L62'>superset/examples/sf_population_polygons.py:62:18:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L43'>superset/extensions/discovery.py:43:24:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L43'>superset/extensions/discovery.py:43:24:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L53'>superset/extensions/discovery.py:53:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L53'>superset/extensions/discovery.py:53:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L62'>superset/extensions/discovery.py:62:33:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L62'>superset/extensions/discovery.py:62:33:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L65'>superset/extensions/discovery.py:65:30:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L65'>superset/extensions/discovery.py:65:30:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L69'>superset/extensions/discovery.py:69:22:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/discovery.py#L69'>superset/extensions/discovery.py:69:22:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L103'>superset/extensions/local_extensions_watcher.py:103:22:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L103'>superset/extensions/local_extensions_watcher.py:103:22:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L47'>superset/extensions/local_extensions_watcher.py:47:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L47'>superset/extensions/local_extensions_watcher.py:47:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L73'>superset/extensions/local_extensions_watcher.py:73:28:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L73'>superset/extensions/local_extensions_watcher.py:73:28:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L78'>superset/extensions/local_extensions_watcher.py:78:21:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L78'>superset/extensions/local_extensions_watcher.py:78:21:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L92'>superset/extensions/local_extensions_watcher.py:92:32:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/extensions/local_extensions_watcher.py#L92'>superset/extensions/local_extensions_watcher.py:92:32:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/apache/superset/blob/a8be5a5a0caab08c3b3a18606808ec947dd9afe0/superset/migrations/shared/catalogs.py#L305'>superset/migrations/shared/catalogs.py:305:17:</a> G004 Logging statement uses f-string
... 77 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L135'>src/bokeh/application/handlers/directory.py:135:25:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L135'>src/bokeh/application/handlers/directory.py:135:25:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/document_lifecycle.py#L66'>src/bokeh/application/handlers/document_lifecycle.py:66:25:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/document_lifecycle.py#L66'>src/bokeh/application/handlers/document_lifecycle.py:66:25:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L581'>src/bokeh/io/notebook.py:581:19:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L581'>src/bokeh/io/notebook.py:581:19:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L582'>src/bokeh/io/notebook.py:582:19:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L582'>src/bokeh/io/notebook.py:582:19:</a> G004 [*] Logging statement uses f-string
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/state.py#L176'>src/bokeh/io/state.py:176:22:</a> G004 Logging statement uses f-string
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/state.py#L176'>src/bokeh/io/state.py:176:22:</a> G004 [*] Logging statement uses f-string
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| G004 | 138 | 0 | 0 | 138 | 0 |

</p>
</details>




---

_Comment by @MeGaGiGaGon on 2025-07-13 01:12_

How does this handle arbitrary format specs? Reading the code it looks to me like they all get converted to `%s`, which is definitely not ideal, and should at the very least make the fix unsafe/not offer a fix at all. As an example, what does the fix do on this code?
```py
import logging
logging.info(f"{x:arbitrary data in format spec} {x:{'nested format spec'}}")
```

---

_Label `fixes` added by @MichaReiser on 2025-07-14 07:43_

---

_Comment by @ntBre on 2025-07-17 16:27_

Thanks for working on this! I think @MeGaGiGaGon raises some good points. I think it would make sense to bail out and not offer a fix if we see any complicated format specs, especially for a first iteration. We could keep the `%d` and `%f` handling that it looks like you already worked on, but I'd even be happy with only handling `{var}` -> `%s` in a first version (omitting format specs entirely).

I can try to give this a review next week if you resolve the conflicts :)

---

_Review requested from @ntBre by @ntBre on 2025-07-17 16:27_

---

_Comment by @hamirmahal on 2025-07-21 06:42_

You're welcome! That works for me. I pulled the latest changes and fixed the merge conflicts.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/violations.rs`:331 on 2025-07-21 21:44_

tiny nit: We usually put this below `message`.

I also think it might be nice to wrap % in backticks:


```suggestion
    fn fix_title(&self) -> Option<String> {
        Some("Convert to lazy `%` formatting".to_string())
    }
```

This message feels very slightly misleading to me (it makes me think the fix is something like `"%s" % var` with the user adding a `%`), but I'm also not coming up with anything better, and they do need to add some `%`s themselves, in the quotes.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:61 on 2025-07-21 21:45_

I think it would be nice to factor everything in the `is_rule_enabled` check into a function. We're getting pretty indented here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:111 on 2025-07-21 21:52_

I believe that it's a breaking change to add a safe fix on stable: [docs](https://docs.astral.sh/ruff/versioning/#version-changes). We should gate this behind a preview check like these:

https://github.com/astral-sh/ruff/blob/cabdd969ec3b7807a777ac87de94771a67525777/crates/ruff_linter/src/preview.rs#L204-L207

You can do something like this:

```rust
let mut diagnostic = checker.report_diagnostic(LoggingFString, msg.range());

if !is_f_string_logging_fix_enabled(checker.settings()) {
    return
}

// fix code here
```

`report_diagnostic` ensures the diagnostic gets reported when the guard it returns is dropped, so if you return early without setting a fix, everything should work out.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:80 on 2025-07-22 00:44_

I think this would also be helped by my suggestion to create the diagnostic early and return if we don't want a fix. Instead of setting a flag and breaking here, we can just return.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:110 on 2025-07-22 00:48_

nit: We usually try to use named arguments to `format!`. Something like this maybe:


```suggestion
                    let replacement = format!(
                        "\"{format_string}\"{sep}{args}",
                        sep = if args.is_empty() {
                            ""
                        } else {
                            ", "
                        },
                        args = args.join(", ")
                    );
```

I think you can also use string literals instead of converting to `String`s.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:111 on 2025-07-22 00:57_

How does this fix handle an f-string with `%` formatting directives in it? I'm not sure if we have any utility functions for detecting that, but I think that would cause problems here. For example:

```python
import logging

x = 1
logging.error(f"{x} -> %s", x)
```

I think the current fix will overwrite the whole `logging.error` range. I think the logging functions also accept `**kwargs` as described on [`Logger.debug`](https://docs.python.org/3/library/logging.html#logging.Logger.debug), so we may need to be more careful about existing arguments in general.

---

_@ntBre reviewed on 2025-07-22 00:59_

Thanks! I had some organizational suggestions and also some edge cases I think we'll need to handle.

---

_Comment by @hamirmahal on 2025-07-23 16:21_

You're welcome! Weekends are usually when I have more availability; I'll see if I can commit your suggestions now, though.

---

_Review comment by @hamirmahal on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:111 on 2025-07-27 18:27_

Hmm... I think the code in its current state does this.

```python
import logging

x = 1
logging.error(f"{x} -> %s", x) # original
logging.error("%s -> %s", x, x) # auto-fixed
```

I'm seeing the same output for both the original and fixed line, so I think this is valid? I could be missing something.

```
ERROR:root:1 -> 1
ERROR:root:1 -> 1
```

---

_@hamirmahal reviewed on 2025-07-27 18:27_

---

_@hamirmahal reviewed on 2025-07-27 18:28_

---

_Review comment by @hamirmahal on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:111 on 2025-07-27 18:28_

When I tried to [gate the f-string formatting fix behind a preview check](https://github.com/astral-sh/ruff/commit/07990a73916030c077b279b61ad473dc491630b4), that caused CI to fail.

---

_Review requested from @ntBre by @hamirmahal on 2025-07-27 18:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:15 on 2025-08-07 18:50_

I think we could report the diagnostic here at the top and then immediately return if preview is not enabled:

```rust
let mut diagnostic = checker.report_diagnostic(LoggingFString, msg.range());

if !is_fix_f_string_logging_enabled(checker.settings()) {
    return;
}
```

The diagnostic gets emitted when the guard returned by `report_diagnostic` is dropped, so we can do neat stuff like this. Later in the function we can just attach a fix to `diagnostic`, if needed.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:32 on 2025-08-07 18:51_

Yeah, then we can also drop these additional `report_diagnostic` calls and just return here, with the change above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:82 on 2025-08-07 18:51_

This can also be an early return:

```rust
if args.is_empty() {
    return;
}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:55 on 2025-08-07 18:52_

We already checked `args.is_empty()`, so this is always a comma.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:110 on 2025-08-07 18:53_

nit: I think we can just call the function `logging_f_string` to match the rule name.

We could also keep using `report_diagnostic_if_enabled` inside of the function, if we wanted. If it returns `None`, we'd just early return at the top of the function. This is fine too, though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/snapshots/ruff_linter__rules__flake8_logging_format__tests__G004.py.snap`:278 on 2025-08-07 18:57_

We might want an example with a different variable name, just to make sure which `x` is which. That's my bad for the initial suggestion. Maybe something like this:

```py
y = 1
logging.error(f"{y} -> %s", x)
```

I just want to make sure it comes out as `logging.error("%s -> %s", y, x)` and not `x, y`. Hopefully that makes sense. We might also want to test the opposite order to be extra sure. I wouldn't really mind bailing out if the format string contains an existing `%` either, just to avoid an erroneous fix.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:54 on 2025-08-07 19:22_

I think we should try to reuse the f-string's quotes (at least the first element's if it's concatenated) here instead of hard-coding double quotes. You may need to add a `string_flags` method or something on `FStringPart`. I forget how we've done this before, but this should be a good start:

https://github.com/astral-sh/ruff/blob/1070e8cbf9211028a1add17d618ee7d7e78062e9/crates/ruff_python_ast/src/nodes.rs#L571-L576

Unfortunately `quote_style` isn't quite enough because it doesn't capture whether it's a single- or triple-quoted string. We probably need something like this if it doesn't already exist and I'm just overlooking it:

```rust
    pub fn quote_str(&self) -> &str {
        match self {
            Self::Literal(string_literal) => string_literal.flags.quote_str(),
            Self::FString(f_string) => f_string.flags.quote_str(),
        }
    }
```

---

_@ntBre reviewed on 2025-08-07 19:26_

Thanks, this is still looking good! Just a few more minor structural suggestions and another test idea. We still need to preview-gate this too. Let me know if you need help with that.

Apologies for the long delay in re-reviewing!

---

_@hamirmahal reviewed on 2025-08-17 20:02_

---

_Review comment by @hamirmahal on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:15 on 2025-08-17 20:02_

Oh, that makes sense. Thanks for the suggestions.

---

_Review requested from @ntBre by @hamirmahal on 2025-08-19 17:36_

---

_Label `preview` added by @ntBre on 2025-08-20 20:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:16 on 2025-08-20 20:06_

```suggestion

fn logging_f_string(
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:94 on 2025-08-20 20:09_

Do we need to clone here? I think `args` is still a mutable `Vec` from above, so we could just extend it directly, unless I'm missing another binding that shadows line 31.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:41 on 2025-08-20 20:12_

Do we need this to be a `String` or could we leave it as a `&str`? I could be missing it, but I'm not immediately seeing  a need for an owned string.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:86 on 2025-08-20 20:17_

I _think_ we could drop the `to_string` calls on everything we put into `args` and `existing_names`. It looks to me like it would be fine to have a `Vec<&str>`, but it's not a big deal if it doesn't work out. Just a bit more efficient not to allocate `String`s if we can avoid it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/mod.rs`:53 on 2025-08-20 20:29_

What we usually do for preview tests is create a copy of the `rules` function above this called `preview_rules` and run all of the same `test_case`s through both functions. That's a good way to compare preview and stable behavior across all the snapshots. Or maybe not all the files, since that would be a lot of snapshots :) Here's an example from a different rule with only a subset of the snapshots:

https://github.com/astral-sh/ruff/blob/39f7a876d6aceedc3db4e3d63250d78bbb830fc2/crates/ruff_linter/src/rules/flake8_comprehensions/mod.rs#L54-L58

I was playing with this branch locally and came up with this:

```rust
    #[test_case(Rule::LoggingFString, Path::new("G004.py"))]
    #[test_case(Rule::LoggingFString, Path::new("G004_arg_order.py"))]
    fn preview_rules(rule_code: Rule, path: &Path) -> Result<()> {
        let snapshot = format!(
            "preview__{}_{}",
            rule_code.noqa_code(),
            path.to_string_lossy()
        );
        let diagnostics = test_path(
            Path::new("flake8_logging_format").join(path).as_path(),
            &settings::LinterSettings {
                logger_objects: vec!["logging_setup.logger".to_string()],
                preview: PreviewMode::Enabled,
                ..settings::LinterSettings::for_rule(rule_code)
            },
        )?;
        assert_diagnostics!(snapshot, diagnostics);
        Ok(())
    }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/mod.rs`:67 on 2025-08-20 20:30_

See my comment above, it's also good to see the snapshots for this. I was wondering where the other snapshot file was!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs`:58 on 2025-08-20 20:37_

I think this check is slightly too conservative  because `%%` escapes the `%` and would be okay, but I think that's fine. It would complicate our analysis substantially because we'd have to make sure not to add a `%s` next to an existing `%`. I spent a little while playing with this and think this simple check is probably best for now.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_logging_format/snapshots/ruff_linter__rules__flake8_logging_format__tests__G004_arg_order.py.snap`:20 on 2025-08-20 21:03_

Ahhh, I forgot that we're already bailing out if an existing `%` is present, so we don't have to worry about these fixes. I think we should also bail out if there are any existing positional arguments since that can also be a problem. And then we can avoid the `existing_names` code above.

To reach that point of the code, the user would have to have existing positional arguments but no `%` placeholders in the string, which either indicates a mistake, or even worse, an f-string like this:

```py
placeholder = "%s"
logging.error(f"{placeholder}", 123)
```

which we also shouldn't try to fix.

---

_@ntBre reviewed on 2025-08-20 21:07_

Thank you again, this is very nearly ready. I mostly had very small performance-related nits and a suggestion on test placement. There's also one last edge case I came up with, but I think we can mostly delete code to solve it, which is always nice.

---

_Review requested from @ntBre by @hamirmahal on 2025-08-24 22:32_

---

_@ntBre approved on 2025-08-26 14:50_

Excellent, thanks again!

I went through the ecosystem changes, and they look good too. We don't show the suggested fix, but they all look like simple cases that I'd expect the fix to handle.

---

_Renamed from "feat: add auto-fix for f-string logging statements" to "[`flake8-logging-format`] Add auto-fix for f-string logging calls (`G004`)" by @ntBre on 2025-08-26 14:51_

---

_Merged by @ntBre on 2025-08-26 14:51_

---

_Closed by @ntBre on 2025-08-26 14:51_

---

_Branch deleted on 2025-08-27 03:12_

---

_Comment by @hamirmahal on 2025-08-27 03:12_

You're welcome! Thanks for the suggestions and feedback.

---

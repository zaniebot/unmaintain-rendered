```yaml
number: 12730
title: Setup tracing and document tracing usage
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-tracing
created_at: 2024-08-07T11:55:33Z
updated_at: 2024-08-08T06:42:41Z
url: https://github.com/astral-sh/ruff/pull/12730
synced_at: 2026-01-10T21:47:02Z
```

# Setup tracing and document tracing usage

---

_Pull request opened by @MichaReiser on 2024-08-07 11:55_

This PR extends our tracing configuration with more refined verbosity levels and adds a small guide on how/when to use tracing. 

## Filtering

I haven't figured out how to do field-level filtering. It is supported according to the `EnvFilter` docs but I always get too many events even if I write complete rubbish field filters ;(

## Existing messages

We should revisit our existing messages and improve them. But we can do so while working on the relevant code and as we learn more about logging.

## When to use instrument

My take on this is that `instrument` helps us to get nice flamegraphs and to attribute events to specific spans. I'm not entirely sure yet what the right amount of `instrument` attributes is. I suspect that we currently have too many.  Let's figure this out over time.

## Test plan

**Using `-v`**

```
INFO Target version: py38
```

**Using `-vv`**

```
2024-08-07 15:21:47.531307982 DEBUG Checking workspace
2024-08-07 15:21:47.531494121 DEBUG Checking package /home/micha/astral/test
2024-08-07 15:21:47.535507928 DEBUG Checking file /home/micha/astral/test/c.py
2024-08-07 15:21:47.536372106 INFO Target version: py38
2024-08-07 15:21:47.536477595 DEBUG Resolved module 'child.a' to '/home/micha/astral/test/child/a.py'.
2024-08-07 15:21:47.539610905 DEBUG Resolved module 'typing' to 'stdlib/typing.pyi'.
2024-08-07 15:21:47.552381082 DEBUG Resolved module 'builtins' to 'stdlib/builtins.pyi'.
2024-08-07 15:21:47.585516812 DEBUG Module 'b' not found in the search paths.
2024-08-07 15:21:47.585969620 DEBUG Checking file /home/micha/astral/test/symlinks/foo.py
2024-08-07 15:21:47.586249639 DEBUG Resolved module 'bar.baz' to '/home/micha/astral/test/bar/baz.py'.
2024-08-07 15:21:47.586441368 DEBUG Checking file /home/micha/astral/test/bar/__init__.py
2024-08-07 15:21:47.586548578 DEBUG Checking file /home/micha/astral/test/hard-links/main.py
2024-08-07 15:21:47.586859207 DEBUG Module 'a' not found in the search paths.
2024-08-07 15:21:47.587004216 DEBUG Module 'a_alias' not found in the search paths.
2024-08-07 15:21:47.587320915 DEBUG Checking file /home/micha/astral/test/hard-links/a.py
2024-08-07 15:21:47.587575634 DEBUG Checking file /home/micha/astral/test/test-notebook.ipynb
2024-08-07 15:21:47.588266662 DEBUG Resolved module 'random' to 'stdlib/random.pyi'.
2024-08-07 15:21:47.588515651 DEBUG Resolved module 'x' to '/home/micha/astral/test/x.py'.
2024-08-07 15:21:47.588970629 DEBUG Checking file /home/micha/astral/test/x.py
2024-08-07 15:21:47.589287938 DEBUG Checking file /home/micha/astral/test/bar/baz.py
2024-08-07 15:21:47.589447748 DEBUG Checking file /home/micha/astral/test/hard-links/sub/a.py
2024-08-07 15:21:47.589697997 DEBUG Checking file /home/micha/astral/test/child/a.py
2024-08-07 15:21:47.589832246 DEBUG Checking file /home/micha/astral/test/child3/a.py
2024-08-07 15:21:47.590109115 DEBUG Checking file /home/micha/astral/test/child2/a.py
2024-08-07 15:21:47.590360335 DEBUG Checking file /home/micha/astral/test/test.py
2024-08-07 15:21:47.591255552 DEBUG Resolved module '_typeshed' to 'stdlib/_typeshed/__init__.pyi'.
2024-08-07 15:21:47.596300975 DEBUG Checking file /home/micha/astral/test/hard-links/a_alias.py
2024-08-07 15:21:47.596563415 DEBUG Checking file /home/micha/astral/test/child/__init__.py
2024-08-07 15:21:47.596673024 DEBUG Checking file /home/micha/astral/test/child3/__init__.py
2024-08-07 15:21:47.596774514 DEBUG Checking file /home/micha/astral/test/child2/__init__.py
2024-08-07 15:21:47.596876443 DEBUG Checking file /home/micha/astral/test/y.py
2024-08-07 15:21:47.597101013 DEBUG Checking file /home/micha/astral/test/hard-links/parent_test.py
```

**Using `-vvv` in release mode**

```
24┐red_knot_workspace::workspace::check{}
24├─   0.000975s   0ms DEBUG red_knot_workspace::workspace Checking workspace
24└─┐red_knot_workspace::workspace::check{self=Package { [salsa id]: Id(0) }}
24  ├─   0.000990s   0ms DEBUG red_knot_workspace::workspace Checking package /home/micha/astral/test
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child3/a.py")}
24    ├─   0.003016s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child3/a.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child3/a.py")}
24      ├─   0.003182s   0ms  INFO red_knot_module_resolver::resolver Target version: py38
24      ├─   0.003698s   0ms DEBUG red_knot_module_resolver::resolver Resolved module 'typing' to 'stdlib/typing.pyi'.
24      ├─   0.005381s   2ms DEBUG red_knot_module_resolver::resolver Resolved module 'builtins' to 'stdlib/builtins.pyi'.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/test-notebook.ipynb")}
24    ├─   0.009774s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/test-notebook.ipynb
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/test-notebook.ipynb")}
24      ├─   0.009876s   0ms DEBUG red_knot_module_resolver::resolver Resolved module 'random' to 'stdlib/random.pyi'.
24      ├─   0.009895s   0ms DEBUG red_knot_module_resolver::resolver Resolved module 'child.a' to '/home/micha/astral/test/child/a.py'.
24      ├─   0.009928s   0ms DEBUG red_knot_module_resolver::resolver Resolved module 'x' to '/home/micha/astral/test/x.py'.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/y.py")}
24    ├─   0.009976s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/y.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/y.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/bar/baz.py")}
24    ├─   0.010015s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/bar/baz.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/bar/baz.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/x.py")}
24    ├─   0.010051s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/x.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/x.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child3/__init__.py")}
24    ├─   0.010094s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child3/__init__.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child3/__init__.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/c.py")}
24    ├─   0.010120s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/c.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/c.py")}
24      ├─   0.010172s   0ms DEBUG red_knot_module_resolver::resolver Module 'b' not found in the search paths.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/hard-links/main.py")}
24    ├─   0.010200s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/hard-links/main.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/hard-links/main.py")}
24      ├─   0.010241s   0ms DEBUG red_knot_module_resolver::resolver Module 'a' not found in the search paths.
24      ├─   0.010255s   0ms DEBUG red_knot_module_resolver::resolver Module 'a_alias' not found in the search paths.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/hard-links/a_alias.py")}
24    ├─   0.010279s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/hard-links/a_alias.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/hard-links/a_alias.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child/a.py")}
24    ├─   0.010314s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child/a.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child/a.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child2/a.py")}
24    ├─   0.010331s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child2/a.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child2/a.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/symlinks/foo.py")}
24    ├─   0.010360s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/symlinks/foo.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/symlinks/foo.py")}
24      ├─   0.010389s   0ms DEBUG red_knot_module_resolver::resolver Resolved module 'bar.baz' to '/home/micha/astral/test/bar/baz.py'.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/hard-links/sub/a.py")}
24    ├─   0.010404s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/hard-links/sub/a.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/hard-links/sub/a.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/hard-links/parent_test.py")}
24    ├─   0.010442s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/hard-links/parent_test.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/hard-links/parent_test.py")}
24      ├─   0.010535s   0ms DEBUG red_knot_module_resolver::resolver Resolved module '_typeshed' to 'stdlib/_typeshed/__init__.pyi'.
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child/__init__.py")}
24    ├─   0.011076s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child/__init__.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child/__init__.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/child2/__init__.py")}
24    ├─   0.011104s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/child2/__init__.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/child2/__init__.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/test.py")}
24    ├─   0.011125s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/test.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/test.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/bar/__init__.py")}
24    ├─   0.011189s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/bar/__init__.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/bar/__init__.py")}
24    ┌─┘
24  ┌─┘
24  └─┐red_knot_workspace::workspace::check_file{path=System("/home/micha/astral/test/hard-links/a.py")}
24    ├─   0.011213s   0ms DEBUG red_knot_workspace::workspace Checking file /home/micha/astral/test/hard-links/a.py
24    └─┐red_knot_workspace::lint::lint_semantic{file=System("/home/micha/astral/test/hard-links/a.py")}
24    ┌─┘
24  ┌─┘
24┌─┘
24┘
```

---

_@MichaReiser reviewed on 2024-08-07 11:57_

---

_Review comment by @MichaReiser on `crates/red_knot/src/logging.rs`:251 on 2024-08-07 11:57_

This is copied from uv. The only change is that I use local time over UTC time. 

---

_Label `red-knot` added by @MichaReiser on 2024-08-07 12:55_

---

_Marked ready for review by @MichaReiser on 2024-08-07 12:57_

---

_Review requested from @carljm by @MichaReiser on 2024-08-07 12:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-07 12:57_

---

_Comment by @zanieb on 2024-08-07 13:32_

What the heck it's not even user-facing and it's nicer than the setup we have in uv!

---

_Comment by @zanieb on 2024-08-07 13:34_

Do you turn on nested span indentation at higher verbosity levels? That's kind of nice.

---

_Comment by @MichaReiser on 2024-08-07 13:36_

> Do you turn on nested span indentation at higher verbosity levels? That's kind of nice.

It's roughly the same as uv where `-vvv` uses the hierarchical output format and  `-vv` shows timestamp which `-v` doesn't

https://github.com/astral-sh/uv/blob/f63a1fde4266a6b2137461469d8ce811f38a639e/crates/uv/src/logging.rs#L166-L178

Most of the code is actually just adopted from UV. 

---

_Comment by @github-actions[bot] on 2024-08-07 13:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @carljm on `crates/red_knot/docs/tracing.md`:4 on 2024-08-07 16:03_

Very minor nit: Doesn't "`debug`..`error`" technically exclude `error`, if we assume Rust range syntax, which is what this seems to be emulating? But `error` is kind of the most user-facing, so probably we shouldn't be ambiguous about this. Maybe just say something like "`debug` or greater severity"?

---

_Review comment by @carljm on `crates/red_knot/docs/tracing.md`:1 on 2024-08-07 16:06_

I assume that the `crates/red_knot/docs/...` directory is for developer-facing docs, not user-facing docs?

---

_Review comment by @carljm on `crates/red_knot/src/logging.rs`:48 on 2024-08-07 16:10_

```suggestion
    /// Enables verbose output. Emits Ruff and Red Knot events up to the [`INFO`](tracing::Level::INFO) Corresponds to `-v`
```

---

_@carljm approved on 2024-08-07 16:17_

This looks amazing!! Thanks for the clear documentation.

The one thing not mentioned here which I would find really useful is how to enable this for a test in e.g. `red_knot_python_semantic` crate, or if that's even doable. I often find myself debugging something in the context of a test, and it would be really useful to have tracing output.

---

_Comment by @MichaReiser on 2024-08-07 19:58_

@carljm in what kind of tracing are you interested in tests? Is it the log messages or primarily the trace spans (or both)

A test helper that accepts an EnvFilter does sound useful for ad-hoc test inspections. 

---

_Comment by @carljm on 2024-08-07 20:34_

Potentially both. It's for the same debugging purposes that you'd use `-vv` or `-vvv` on the command line.

---

_Comment by @MichaReiser on 2024-08-08 06:05_

> Potentially both. It's for the same debugging purposes that you'd use `-vv` or `-vvv` on the command line.

I can take a look as a separate (low priority) PR. I don't think I want to expose both `-vv` and `-vvv` styles. I might just go with `-vvv` 

---

_@MichaReiser reviewed on 2024-08-08 06:06_

---

_Review comment by @MichaReiser on `crates/red_knot/docs/tracing.md`:1 on 2024-08-08 06:06_

For now, that's certainly true I don't know about long-term. We could have `docs/users` and `docs/dev` if we end up with needing both. But most user-facing documentation should probably live on the website.

---

_Merged by @MichaReiser on 2024-08-08 06:28_

---

_Closed by @MichaReiser on 2024-08-08 06:28_

---

_Branch deleted on 2024-08-08 06:28_

---

_Comment by @codspeed-hq[bot] on 2024-08-08 06:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-tracing)

### Merging #12730 will **improve performances by 7.46%**

<sub>Comparing <code>red-knot-tracing</code> (6e00f3f) with <code>main</code> (5107a50)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `red-knot-tracing` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 778.8 µs | 724.7 µs | +7.46% |


---

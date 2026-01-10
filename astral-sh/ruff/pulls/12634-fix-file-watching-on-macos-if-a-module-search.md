```yaml
number: 12634
title: Fix file watching on macOS if a module-search path is a symlink
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: canonicalize-watch-paths
created_at: 2024-08-02T15:40:24Z
updated_at: 2024-08-03T07:38:13Z
url: https://github.com/astral-sh/ruff/pull/12634
synced_at: 2026-01-10T21:47:02Z
```

# Fix file watching on macOS if a module-search path is a symlink

---

_Pull request opened by @MichaReiser on 2024-08-02 15:40_

## Summary

This PR fixes the `symlinked_module_search_path` test for macOS

The scenario that it tests is

```
A module search path is a symlink.

Setup:

 - site-packages
   | - bar/baz.py

 - workspace
   |-- .venv/lib/python3.12/site-packages -> /site-packages
   |
   |-- foo.py
```

The key point here is that the `site-packages` module search path links to a path outside the workspace. But the symlink itself is part of the workspace. 

Fixing this required a few changes:

1. We need to canonicalize the watch paths **before** deduplicating them to avoid removing `.venv/lib/python3.12/site-packages` because we believe the workspace watch already covers it. 
2. We need to canonicalize watch paths or the macOS watcher won't observe any changes made in that directory (it would only observe the symlink file, not what we want)
3. We need to canonicalize the module search paths because the module path otherwise is `.venv/lib/python3.12/site-packages/bar/baz.py` but the watcher emits events for `site-packages/bar/baz.py`.

Now this fixed it for macOS, but it broke Windows and Unix. Fun times. 

I'm not too fond of what comes next, but it seems that Windows and Unix rely on the order in which the watch paths are registered (oh my...) and emit an event for the last registered watch path. In the previous implementation, the workspace was always last, so we only received an event for ``.venv/lib/python3.12/site-packages/bar/baz.py`, but not for `site-packages/bar/baz.py` and thus missed the updated :( 

This PR changes the order in which we register search paths. 


Fixes #12645

## Alternative designs

I considered canonicalizing the path in `apply_changes` so that we cover both the symlinked and canonicalized paths. This works fine unless someone deletes the file, in which case canonicalizing no longer works. 

We could keep a bi-directional map from path -> realpath and realpath -> path in `Files`. I see this as an option but I would still prefer if we don't need it, at least for now.

I'm open to alternative suggestions.

## Test plan

I tested the last revision on macOs and Linux. The test on the Windows CI pass. 




---

_Comment by @github-actions[bot] on 2024-08-02 15:53_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Label `red-knot` added by @MichaReiser on 2024-08-02 17:22_

---

_@MichaReiser reviewed on 2024-08-02 17:24_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/watch/workspace_watcher.rs`:74 on 2024-08-02 17:24_

There's a small risk that we miss events between de-registering and registering. Pyright also removes all watchers before setting the new watchers so I think that's fine (this code should almost never run)

---

_Marked ready for review by @MichaReiser on 2024-08-02 17:25_

---

_Review requested from @carljm by @MichaReiser on 2024-08-02 17:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-02 17:25_

---

_Renamed from "Canonicalize watch paths" to "Fix file watching on macOS if a module-search path is a symlink" by @MichaReiser on 2024-08-02 17:25_

---

_Review comment by @carljm on `crates/red_knot_workspace/src/watch/workspace_watcher.rs`:55 on 2024-08-02 18:20_

I'm not sure I understand how this can happen if a) we first canonicalize all paths and then b) we de-deduplicate nested paths.

---

_Review comment by @carljm on `crates/red_knot_workspace/src/watch/workspace_watcher.rs`:55 on 2024-08-02 18:22_

Oh, if there's a symlink from within one root to within another root, on some systems I guess we might get an event for a path within either root (or both) for the symlinked file (or a file inside a symlinked directory)

---

_@carljm approved on 2024-08-02 18:23_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/watch/workspace_watcher.rs`:55 on 2024-08-03 07:19_

I added an example.

---

_@MichaReiser reviewed on 2024-08-03 07:19_

---

_Merged by @MichaReiser on 2024-08-03 07:24_

---

_Closed by @MichaReiser on 2024-08-03 07:24_

---

_Branch deleted on 2024-08-03 07:24_

---

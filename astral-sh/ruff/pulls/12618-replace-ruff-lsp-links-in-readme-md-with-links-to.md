```yaml
number: 12618
title: "Replace `ruff-lsp` links in `README.md` with links to new documentation page"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-08-02T03:47:00Z
updated_at: 2024-08-04T10:01:36Z
url: https://github.com/astral-sh/ruff/pull/12618
synced_at: 2026-01-12T15:55:41Z
```

# Replace `ruff-lsp` links in `README.md` with links to new documentation page

---

_@InSyncWithFoo_

Since `ruff-lsp` has been (semi-)deprecated for sometime, it wouldn't make sense to mention it in the most prominent sections of the `README`. Instead, they should point to the new <i>[Editor Integrations](https://docs.astral.sh/ruff/editors/)</i> documentation page.

---

_Review comment by @dhruvmanila on `README.md`:38 on 2024-08-02 09:46_

Can we just use the direct link to the editors section? I think we should link the setup section specifically: https://docs.astral.sh/ruff/editors/setup

---

_Review comment by @dhruvmanila on `README.md`:183 on 2024-08-02 09:52_

`ruff-lsp` isn't deprecated yet, so maybe we can reword this as:

```
Ruff can also be used as a [VS Code extension](https://github.com/astral-sh/ruff-vscode) or with [various other editors](https://docs.astral.sh/ruff/editors/setup).
```

---

_@dhruvmanila requested changes on 2024-08-02 09:52_

---

_Label `documentation` added by @dhruvmanila on 2024-08-02 09:52_

---

_@InSyncWithFoo reviewed on 2024-08-02 13:39_

---

_Review comment by @InSyncWithFoo on `README.md`:38 on 2024-08-02 13:39_

There seems to be some problems with the checks:

```text
   Compiling ruff_dev v0.0.0 (/home/runner/work/ruff/ruff/crates/ruff_dev)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 41.32s
     Running `target/debug/ruff_dev generate-docs`
Rule F401 references deprecated option lint.ignore-init-module-imports.
Traceback (most recent call last):
  File "/home/runner/work/ruff/ruff/scripts/generate_mkdocs.py", line 220, in <module>
    main()
  File "/home/runner/work/ruff/ruff/scripts/generate_mkdocs.py", line 120, in main
    raise ValueError(msg)
ValueError: Unexpected absolute link to documentation: (https://docs.astral.sh/ruff/editors/setup)
```

I tried `editors/setup.md` and that [passed CI](https://github.com/astral-sh/ruff/pull/12618/checks?sha=fd0e5b558795844dde04512814d4b7b3318c628d), but the link would lead to nowhere when viewing from the repo instead of the rendered site.

---

_Comment by @dhruvmanila on 2024-08-02 15:14_

I think you'll need to add an entry for this here:

https://github.com/astral-sh/ruff/blob/012198a1b0f4870902992218c04aac3f07ee76c8/scripts/generate_mkdocs.py#L58-L73

It should be:
```
"https://docs.astral.sh/ruff/editors/setup": "editors/setup.md"
```

Although you might want to confirm by running `python scripts/generate_mkdocs.py`

---

_Review requested from @dhruvmanila by @InSyncWithFoo on 2024-08-02 16:21_

---

_Comment by @github-actions[bot] on 2024-08-02 16:39_

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

_@dhruvmanila approved on 2024-08-04 10:01_

Thanks!

---

_Merged by @dhruvmanila on 2024-08-04 10:01_

---

_Closed by @dhruvmanila on 2024-08-04 10:01_

---

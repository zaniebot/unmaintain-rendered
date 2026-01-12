```yaml
number: 12622
title: Use uv workspaces instead of pip
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
draft: true
base: main
head: use-uv-workspace
created_at: 2024-08-02T09:09:54Z
updated_at: 2024-08-12T14:14:57Z
url: https://github.com/astral-sh/ruff/pull/12622
synced_at: 2026-01-12T15:55:41Z
```

# Use uv workspaces instead of pip

---

_@MichaReiser_

## Summary

I don't know what I'm doing and this doesn't work.

* The lock file looks okay?
* I can't run any scripts because the dependencies don't seem to get installed
* I'm probably doing something very wrong

## Test Plan

See contributing.md. I didn't get passed `uv sync`


---

_Label `internal` added by @MichaReiser on 2024-08-02 09:10_

---

_Review comment by @T-256 on `docs/pyproject.toml`:21 on 2024-08-02 10:58_

- fixed `dependencies` under `project` table.
- join dev-dependencies
- add `requires-python`
- use upper bound instead of equality for dependencies. (lockfile will do version locks for us)
```suggestion
[project]
name = "ruff-docs"
version = "0.0.0"
dependencies = [
  "PyYAML>=6.0.1",
  "mkdocs>=1.5.0",
  "mdformat>=0.7.17",
  "mdformat-mkdocs>=2.0.4",
  "mdformat-admon>=2.0.2",
  "mkdocs-material>=9.1.18",
  "mkdocs-redirects>=1.2.1",
]
requires-python = ">=3.8"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Review comment by @T-256 on `docs/requirements-insiders.txt`:1 on 2024-08-02 10:58_

Why do we need to keep it?

---

_@T-256 reviewed on 2024-08-02 11:06_

---

_@MichaReiser reviewed on 2024-08-02 11:25_

---

_Review comment by @MichaReiser on `docs/pyproject.toml`:21 on 2024-08-02 11:25_

lol, slightly emberassing :D Thanks

---

_@MichaReiser reviewed on 2024-08-02 11:26_

---

_Review comment by @MichaReiser on `docs/requirements-insiders.txt`:1 on 2024-08-02 11:26_

We use a custom version of mkdocs but external contributors don't have access to that repository. I don't know if there's a better way to support this?

---

_Comment by @zanieb on 2024-08-02 12:43_

Can you clarify what's not working?

---

_Comment by @MichaReiser on 2024-08-02 12:50_

I can't run the mkdocs script because the dependencies are missing (I checked, they're not in my venv)

---

_Comment by @charliermarsh on 2024-08-02 12:53_

I'm not sure what you're running exactly but we need to mark Ruff as unmanaged. We don't want to be building Ruff to run scripts.

---

_Comment by @MichaReiser on 2024-08-02 13:34_

I try to run `uv run --verbose scripts/generate_mkdocs.py`. Overall, I try to get the workflow documented in the contributing.md to work.

---

_Comment by @charliermarsh on 2024-08-02 13:47_

It's because the workspace members aren't declared as dependencies of the project.

---

_Comment by @charliermarsh on 2024-08-02 13:49_

`uv run --package ruff-docs --verbose scripts/generate_mkdocs.py` does the right thing, but fails because `./docs` isn't a valid Python package (it needs `src/ruff_docs/__init__.py`).


---

_Comment by @charliermarsh on 2024-08-02 13:50_

If you add this to `docs/pyproject.toml`, then `uv run --package ruff-docs -- scripts/generate_mkdocs.py ` works:

```toml
[tool.hatch.build.targets.wheel]
packages = ["src/ruff_docs"]
```

I think we should re-add that to `uv init`, it got dropped during code review.

---

_@charliermarsh reviewed on 2024-08-02 13:58_

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:164 on 2024-08-02 13:58_

This was a breaking change in `mdformat-admon`, in the 2.0.3 release: https://github.com/KyleKing/mdformat-mkdocs/commit/6a329979e27da47a215607d4e30f61d22e683b89

---

_@charliermarsh reviewed on 2024-08-02 13:58_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:297 on 2024-08-02 13:58_

This seems off... Isn't that mismatched now?

---

_Comment by @MichaReiser on 2024-08-02 14:02_

Okay, I don't understand why I would have to declare `ruff_docs` as a package just so that the generate script can resolve `mdformat` :confused: 

---

_Comment by @github-actions[bot] on 2024-08-02 14:18_

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

_Comment by @charliermarsh on 2024-08-05 02:12_

Can you expand on your question?

---

_Comment by @MichaReiser on 2024-08-05 05:40_

It's more of an observation. I understand that the `packages` setting makes the listed package available to other packages. But the `generate_mkdocs` script doesn't depend on the `ruff_docs` package. It depends on some of its transitive dependencies. 

That's why I think it odd that I have to add `ruff_docs` to `packages` just to make the script work. I could understand if I have to move the script into the `ruff_docs` directory to make it work (so that uv knows that it should run the script with additional third party dependencies). 



---

_Comment by @charliermarsh on 2024-08-05 19:19_

@MichaReiser - I think part of the problem is that `ruff_docs` isn't really a package... An alternative is to make these all dev dependencies in the workspace root. Do you find that more intuitive?

```diff
diff --git a/pyproject.toml b/pyproject.toml
index eeba1c160..1ab117188 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -8,7 +8,7 @@ version = "0.5.6"
 description = "An extremely fast Python linter and code formatter, written in Rust."
 authors = [{ name = "Astral Software Inc.", email = "hey@astral.sh" }]
 readme = "README.md"
-requires-python = ">=3.7"
+requires-python = ">=3.8"
 license = { file = "LICENSE" }
 keywords = [
   "automation",
@@ -44,7 +44,18 @@ Documentation = "https://docs.astral.sh/ruff/"
 Changelog = "https://github.com/astral-sh/ruff/blob/main/CHANGELOG.md"

 [tool.uv.workspace]
-members = ["docs", "python/ruff-ecosystem"]
+members = ["python/ruff-ecosystem"]
+
+[tool.uv]
+dev-dependencies = [
+  "PyYAML>=6.0.1",
+  "mkdocs>=1.5.0",
+  "mdformat>=0.7.17",
+  "mdformat-mkdocs>=2.0.4",
+  "mdformat-admon>=2.0.3",
+  "mkdocs-material>=9.1.18",
+  "mkdocs-redirects>=1.2.1",
+]

 [tool.maturin]
 bindings = "bin"
```

Then `uv run -- scripts/generate_mkdocs.py` works.

(I initially didn't realize that the script you were running wasn't part of `docs`.)


---

_Comment by @T-256 on 2024-08-05 22:41_

Can I work on this?

---

_Comment by @MichaReiser on 2024-08-06 05:51_

@charliermarsh I see. But let's pretend that `ruff_docs` is a package. 

>  An alternative is to make these all dev dependencies in the workspace root. Do you find that more intuitive?

Kind of. With `lerna` I would have two options:

1. Make it a workspace-level script
1. Move it into `ruff_docs` and make it a `ruff_docs` script 

To me it seems uv only supports 1. or what's the recommended way to run `generate_mkdocs.py` when declaring the dependency on the package level (let's pretend that `ruff_docs` is a real package)

@T-256 sure! Feel free to open a new PR. I then close this one

---

_Comment by @charliermarsh on 2024-08-06 12:27_

@MichaReiser -- Genuinely trying to follow but I'm a little confused. For (2), you can add a `pyproject.toml` to `ruff_docs`, declare the dependencies in that `pyproject.toml`, and then run `uv run --package ruff-docs ./scripts/generate_mkdocs.py`. Isn't that the current state of this PR?

---

_Comment by @MichaReiser on 2024-08-06 12:55_

>  Isn't that the current state of this PR?

Not what's described in the `CONTRIBUTING.md`. So no. I wasn't aware that this is a possibility. 

I think where the behavior of `uv run` differs from what I expected is that I expected `uv run ruff_docs/generate_mkdocs` to have the same behavior as `uv run -p ruff_docs ruff_docs/generate_mkdocs`

---

_Comment by @charliermarsh on 2024-08-06 12:56_

Oh I see, that running a script in the `ruff_docs` directory would behave equivalently to running a script _from_ the `ruff_docs` directory.

---

_Comment by @zanieb on 2024-08-06 13:38_

Fwiw we do `uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.public.yml` in uv (but I know it's a little different over here)

---

_Closed by @MichaReiser on 2024-08-12 14:14_

---

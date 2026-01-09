---
number: 16382
title: "Suggestion: remove `\"uv pip\"` completely"
type: issue
state: closed
author: ei-grad
labels:
  - enhancement
assignees: []
created_at: 2025-10-21T11:27:47Z
updated_at: 2025-10-21T14:36:52Z
url: https://github.com/astral-sh/uv/issues/16382
synced_at: 2026-01-07T13:12:19-06:00
---

# Suggestion: remove `"uv pip"` completely

---

_Issue opened by @ei-grad on 2025-10-21 11:27_

### Summary

The existence of `uv pip` introduces a confusing and fragile anti-pattern. It undermines the unified dependency and environment management model that `uv` is designed to provide.

**Problem:**
`uv pip` bypasses core guarantees of `uv`’s declarative dependency management by allowing direct, imperative package installation. This leads to:

* Divergence between the environment state and `pyproject.toml` / `uv.lock`
* Fragile or non-reproducible builds
* Confusion for users and tools that expect `uv` to enforce consistency
* Breakage in AI-assistant workflows and automation tools that rely on predictable, declarative dependency graphs

**Rationale:**
`uv` already provides first-class mechanisms for managing dependencies (`uv add`, `uv sync`, `uv run`, etc.), all of which maintain a reproducible environment.
`uv pip` reintroduces legacy `pip` semantics into a system that aims to eliminate them. Its presence encourages bad practices and undermines automation safety, especially in scenarios where ephemeral environments are automatically managed (e.g. by AI assistants, build agents, or notebook runners).

**Proposal:**

* Remove `uv pip` entirely, or at minimum hide it behind an explicit “legacy mode” flag.
* Ensure documentation and CLI help clearly emphasize declarative workflows only.

**Expected Outcome:**

* Stronger reproducibility guarantees.
* Simplified CLI semantics.
* Elimination of state drift between declarative and imperative package management paths.

### Example

_No response_

---

_Label `enhancement` added by @ei-grad on 2025-10-21 11:27_

---

_Comment by @konstin on 2025-10-21 12:03_

There are a lot of workflows that need the `uv pip` interface, and it's crucial for people migrating from pip to uv. We also care a lot about compatibility and don't generally remove existing features that break people's workflows. We won't be removing the `uv pip` interface any time soon.

---

_Comment by @ei-grad on 2025-10-21 12:38_

Sure, just a cry of frustration - when instead of simply running `uv run`, all Claude/Codex/Gemini ignore explicit instructions like "NEVER USE uv pip", break their environments, and start doing all sorts of nonsense.

---

_Closed by @ei-grad on 2025-10-21 12:38_

---

_Closed by @ei-grad on 2025-10-21 12:44_

---

_Comment by @konstin on 2025-10-21 13:30_

For problems with LLM integrations, please open an issue describing your specific pain points in the LLM <-> uv interaction. We're working with LLM vendors to improve the uv integration and real-world user stories and reproducible problems (to the extend that LLMs are reproducible) help with that. We can help you much better if you describe what's broken and what workarounds failed instead of generating a specific suggestion.

---

_Comment by @adhi-thirumala on 2025-10-21 14:36_

what if instead of off by default, there was a way to configure a project in `pyproject.toml` to disable `uv pip` commands from working. would seem to resolve this because then if you really wanted `uv pip` to not happen, you could force the command to fail

---

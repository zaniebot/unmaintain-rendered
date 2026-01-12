```yaml
number: 1509
title: "feat: Add base for settings, and relocate venv-name default to it."
type: pull_request
state: closed
author: DanCardin
labels: []
assignees: []
draft: true
base: main
head: dc/venv-name-setting
created_at: 2024-02-16T16:20:28Z
updated_at: 2024-02-16T16:30:00Z
url: https://github.com/astral-sh/uv/pull/1509
synced_at: 2026-01-12T16:04:38Z
```

# feat: Add base for settings, and relocate venv-name default to it.

---

_@DanCardin_

Relocates the `.venv` default venv name into a settings file that can be customized.

I couple of decisions I'm certain will end up getting questioned:

* etcetera vs dirs: Using `etcetera` preempts the moment where someone uses `dirs` to implement settings and annoy all the macos users who dont want their settings to live in `Application Support/`. (Like all the other rust tools using dirs/dirs-next that end up getting issues reported against them).
* Addition of `toml_edit` over `toml`. Mostly I'm just unfamiliar with `toml` and already have working code using toml-edit 😬 . Secondarily I figured long-term you'd probably implement `uv init` or `uv add` that will want to write toml, and you'll end up swapping to `toml_edit` then anyways.

---

If you're not ready to commit to specific settings, or otherwise already have a system (i.e. something from ruff) in mind that you'd want to replicate, feel free to close this. Rather than submitting an issue, I figured I could quickly do the thing I'd otherwise be requesting.

I didn't attempt to implement any tests in case this was quickly dead in the water.

---

_Comment by @zanieb on 2024-02-16 16:28_

Hey Dan! Thanks for taking the time to open a pull request and sketch this out.

We're definitely not ready to commit to a configuration file yet, we want to approach it holistically since we'll need to remain backwards-compatible with what we choose for the foreseeable future. We might need to create an issue to discuss what people want from their configuration.

---

_Comment by @zanieb on 2024-02-16 16:29_

I'm going to close this in favor of https://github.com/astral-sh/uv/issues/1511 — please chime in there!

---

_Closed by @zanieb on 2024-02-16 16:29_

---

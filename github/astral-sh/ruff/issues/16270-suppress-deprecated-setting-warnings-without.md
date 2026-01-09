---
number: 16270
title: Suppress deprecated setting warnings without removing other useful info
type: issue
state: open
author: smackesey
labels:
  - cli
assignees: []
created_at: 2025-02-20T12:20:17Z
updated_at: 2025-02-20T14:48:23Z
url: https://github.com/astral-sh/ruff/issues/16270
synced_at: 2026-01-07T13:12:16-06:00
---

# Suppress deprecated setting warnings without removing other useful info

---

_Issue opened by @smackesey on 2025-02-20 12:20_

### Description

AFAICT the only way to suppress warnings about deprecated settings in my ruff config is to run with `ruff --quiet`. Unfortunately this suppresses other useful information, like how many errors were fixed/found etc. Is there any way to just turn off the deprecation warnings?

---

_Comment by @MichaReiser on 2025-02-20 12:45_

Can you tell me a bit more about why you want to suppress the warnings over migrating the settings? Is there something that prevents you from migrating or is it just that you want to migrate later? 

It's somewhat intentional that the warnings can't be suppressed because those settings will be removed eventually and your setup then fails and we rather here early if users can't migrate to consider adding a replacement or undeprecating the setting. 

I'm not saying that we shouldn't consider adding an option to suppress deprecation warnings, but I'd like to understand the use case better first.

---

_Label `cli` added by @MichaReiser on 2025-02-20 12:45_

---

_Comment by @smackesey on 2025-02-20 14:31_

>Can you tell me a bit more about why you want to suppress the warnings over migrating the settings? Is there something that prevents you from migrating or is it just that you want to migrate later?

I want to set `ruff.lint.ignore-init-module-imports = false`. AFAICT there is no replacement for this behavior. You can use `--preview` mode but even with that, there doesn't currently seem to be a way to replicate the behavior of this setting (I commented elsewhere requesting this).

IMO unless _all_ migrations from deprecated settings are very straightforward and explained directly in the warning message (like a simple rename), then there should be a way to suppress deprecation warnings.

---

_Comment by @MichaReiser on 2025-02-20 14:48_

Deprecating a setting when the new behavior is still in preview seems like a mistake. We should probably undeprecated `ignore-init-module-imports` until the new preview behavior is stabilized




---

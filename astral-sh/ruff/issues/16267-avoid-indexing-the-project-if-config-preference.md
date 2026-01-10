---
number: 16267
title: "Avoid indexing the project if config preference is `editorOnly`"
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-02-20T08:47:54Z
updated_at: 2025-02-27T02:16:15Z
url: https://github.com/astral-sh/ruff/issues/16267
synced_at: 2026-01-10T01:22:57Z
---

# Avoid indexing the project if config preference is `editorOnly`

---

_Issue opened by @dhruvmanila on 2025-02-20 08:47_

### Description

When the server initializes, it'll first try to index the project and create a settings map which can be accessed at a later point for the relevant documents.

But, if user has set `configurationPreference` to be `editorOnly`, there might not be any reason to do this indexing as the server is never going to be using them.

---

_Label `server` added by @dhruvmanila on 2025-02-20 08:47_

---

_Label `help wanted` added by @MichaReiser on 2025-02-20 08:48_

---

_Comment by @dcarrier on 2025-02-24 04:42_

Interesting issue! I am taking a look at this.

---

_Comment by @dhruvmanila on 2025-02-24 06:20_

For reference:

Here's the settings index definition:

https://github.com/astral-sh/ruff/blob/b312b53c2eafd97a084f9f2961a99fbfa257f21a/crates/ruff_server/src/session/index/ruff_settings.rs#L46-L50

And, here's where the indexing happens:

https://github.com/astral-sh/ruff/blob/b312b53c2eafd97a084f9f2961a99fbfa257f21a/crates/ruff_server/src/session/index/ruff_settings.rs#L90-L104

which is invoked when registering the workspace here:

https://github.com/astral-sh/ruff/blob/b312b53c2eafd97a084f9f2961a99fbfa257f21a/crates/ruff_server/src/session/index.rs#L443-L447

---

_Comment by @dcarrier on 2025-02-24 21:59_

Thank you very much for the details @dhruvmanila!

I sketched my initial idea for this here: https://github.com/dcarrier/ruff/commit/9c3f38d7d9f9152d9814fa78419fb7ab68f9f4af

This should address both server init and reload_settings use cases. Happy to open a PR to discuss in more depth, however I wanted to be sure we were generally aligned before doing so.

---

_Comment by @dhruvmanila on 2025-02-25 10:21_

> This should address both server init and reload_settings use cases. Happy to open a PR to discuss in more depth, however I wanted to be sure we were generally aligned before doing so.

I'd recommend opening a PR, we can discuss it there.

---

_Assigned to @dcarrier by @dhruvmanila on 2025-02-25 10:21_

---

_Referenced in [astral-sh/ruff#16381](../../astral-sh/ruff/pulls/16381.md) on 2025-02-25 18:58_

---

_Closed by @dhruvmanila on 2025-02-27 02:16_

---

_Closed by @dhruvmanila on 2025-02-27 02:16_

---

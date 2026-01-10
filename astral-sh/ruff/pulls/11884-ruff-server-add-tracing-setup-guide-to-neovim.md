```yaml
number: 11884
title: "`ruff server`: Add tracing setup guide to Neovim documentation"
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: jane/server/docs/nvim/tracing
created_at: 2024-06-14T22:17:45Z
updated_at: 2024-06-18T20:39:42Z
url: https://github.com/astral-sh/ruff/pull/11884
synced_at: 2026-01-10T21:56:00Z
```

# `ruff server`: Add tracing setup guide to Neovim documentation

---

_Pull request opened by @snowsignal on 2024-06-14 22:17_

A follow-up to [this suggestion](https://github.com/astral-sh/ruff/pull/11747#discussion_r1634297757) on the tracing PR.

---

_Label `documentation` added by @snowsignal on 2024-06-14 22:17_

---

_Label `server` added by @snowsignal on 2024-06-14 22:17_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-15 08:11_

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:86 on 2024-06-17 06:11_

Same as above:

```suggestion
  init_options = {
    settings = {
      logLevel = "debug",
      logFile = "your/log/file/path/log.txt"
    }
  }
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:74 on 2024-06-17 06:12_

I don't think you can directly provide the `settings` key at the top level because the [new server doesn't support `workspace/didChangeConfiguration` (yet)](https://github.com/astral-sh/ruff/blob/84e87771cba841c08ac7d5d1017050b8bd22837e/crates/ruff_server/src/server/api/notifications/did_change_configuration.rs#L20-L21). So, the correct code should be to specify it under `init_options` which sends this in the initialize request:

```suggestion
  init_options = {
    settings = {
      logLevel = "debug",
    }
  }
```

---

_@dhruvmanila requested changes on 2024-06-17 06:13_

Thanks for following up with the documentation!

I don't think we can directly provide the `settings` key, it should be under the `init_options` field. This makes me wonder if there are any other suggestions in the Neovim documentation which uses the `settings` field directly instead of `init_options`.

---

_Comment by @snowsignal on 2024-06-17 16:39_

@dhruvmanila Thank you for opening an issue for `workspace/didChangeConfiguration`! I wasn't aware how this intersected with Neovim configuration.

---

_Review requested from @dhruvmanila by @snowsignal on 2024-06-18 03:45_

---

_@dhruvmanila approved on 2024-06-18 03:47_

---

_Merged by @snowsignal on 2024-06-18 20:39_

---

_Closed by @snowsignal on 2024-06-18 20:39_

---

_Branch deleted on 2024-06-18 20:39_

---

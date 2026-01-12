```yaml
number: 10987
title: "`ruff server`: Write a setup guide for Neovim"
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: jane/server/neovim/setup
created_at: 2024-04-16T22:29:42Z
updated_at: 2024-04-18T02:46:31Z
url: https://github.com/astral-sh/ruff/pull/10987
synced_at: 2026-01-12T15:55:34Z
```

# `ruff server`: Write a setup guide for Neovim

---

_@snowsignal_

## Summary

A setup guide has been written for NeoVim under a new `crates/ruff_server/docs/setup` folder, where future setup guides will also go. This setup guide was adapted from the [`ruff-lsp` guide](https://github.com/astral-sh/ruff-lsp?tab=readme-ov-file#example-neovim).


---

_Label `server` added by @snowsignal on 2024-04-16 22:29_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-16 22:29_

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:1 on 2024-04-17 02:31_

I think it should just be "Neovim" everywhere without the uppercase "V". Refer:
* Website: https://neovim.io/
* Repository: https://github.com/neovim/neovim

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:16 on 2024-04-17 02:35_

We can format it even better :)

```suggestion
> [!NOTE]
>
> If you have the older language server (`ruff-lsp`) configured in NeoVim, make sure to disable it to prevent any conflicts.
```

<img width="1214" alt="Screenshot 2024-04-17 at 08 04 57" src="https://github.com/astral-sh/ruff/assets/67177269/6fe7f2a5-74fb-4ba2-ba27-ba7c58ecc096">


---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:1 on 2024-04-17 02:36_

I think the heading levels should just start with 1 and so on. Can you update that along with other headers?

---

_Review comment by @dhruvmanila on `crates/ruff_server/README.md`:12 on 2024-04-17 02:37_

nit: use uppercase "I" in "install the ..."?

---

_@dhruvmanila reviewed on 2024-04-17 02:37_

---

_@dhruvmanila reviewed on 2024-04-17 02:39_

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:16 on 2024-04-17 02:39_

There are others as well: `[!IMPORTANT]`, `[!WARNING]`, `[!TIP]`. I guess for this we can either go with "important" or "note".

---

_Label `documentation` added by @MichaReiser on 2024-04-17 07:07_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-17 16:00_

---

_@snowsignal reviewed on 2024-04-17 16:03_

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/NEOVIM.md`:1 on 2024-04-17 16:03_

Hmmm I changed this but on second though I actually prefer the smaller headers. Heading level 1 just looks too big for a document of this size. The smaller headers would also match the header size in the `README`.

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:10 on 2024-04-18 02:42_

Either is fine, but as we aren't passing any options, let's use parentheses.

```suggestion
require('lspconfig').ruff.setup()
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/NEOVIM.md`:18 on 2024-04-18 02:43_

```suggestion
> If you have the older language server (`ruff-lsp`) configured in Neovim, make sure to disable it to prevent any conflicts.
```

---

_@dhruvmanila approved on 2024-04-18 02:43_

---

_Renamed from "`ruff server`: Write a setup guide for NeoVim" to "`ruff server`: Write a setup guide for Nevim" by @dhruvmanila on 2024-04-18 02:44_

---

_Renamed from "`ruff server`: Write a setup guide for Nevim" to "`ruff server`: Write a setup guide for Neovim" by @dhruvmanila on 2024-04-18 02:44_

---

_Comment by @dhruvmanila on 2024-04-18 02:45_

(I made small tweaks otherwise looks good, going to merge this. Thanks for writing this up :))

---

_Merged by @dhruvmanila on 2024-04-18 02:46_

---

_Closed by @dhruvmanila on 2024-04-18 02:46_

---

_Branch deleted on 2024-04-18 02:46_

---

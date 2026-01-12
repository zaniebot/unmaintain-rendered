```yaml
number: 2254
title: Autocomplete is extremely slow in Remote SSH
type: issue
state: open
author: SunSeaLucky
labels:
  - performance
  - server
  - completions
assignees: []
created_at: 2025-12-29T07:48:06Z
updated_at: 2026-01-06T15:20:19Z
url: https://github.com/astral-sh/ty/issues/2254
synced_at: 2026-01-12T15:54:26Z
```

# Autocomplete is extremely slow in Remote SSH

---

_@SunSeaLucky_

### Summary

Hello, 

When I use ty as language server (I install ty extension in VSCode) in remote server using Remote SSH:

<img width="325" height="165" alt="Image" src="https://github.com/user-attachments/assets/bbee80fb-9ac5-4306-a0dc-7ba879e56331" />

It always comsumes about **10s** or more to autocomplete. **I'm sure this promble is not presented when I use pylance (or pyright) as  my language server.**

<img width="1022" height="502" alt="Image" src="https://github.com/user-attachments/assets/1bc97413-4c02-4755-be68-eb9a32e07c7e" />

However, it works well when I just develop all things in local. So I suspect there is something wrong in using remote. By the way, other functions works well in remote except the autocomplete function.

My environment:

Local: 
- MacOS 15.3.2
- VSCode version: November 2025 (version 1.107)

Remote:
- VSCode extension id: astral-sh.ty (with ty==0.0.6)
- Linux di-20251207003629-f9fr6 5.4.210-4-velinux1-amd64 #velinux1 SMP Debian 5.4.210-4-velinux1 Tue Mar 21 14:11:05 UTC  x86_64 x86_64 x86_64 GNU/Linux



### Version

ty extension, which says ty==0.0.6 in document. Sorry if any information is not provided.

---

_Label `server` added by @mtshiba on 2025-12-29 07:59_

---

_Comment by @MichaReiser on 2025-12-29 08:48_

Thanks for the report. 

@BurntSushi plans to look into performance improvements for completions because we're aware they're slow. 

One thing I noticed is that we return more than 100k completions on linux (I used `homeassistant` and typed `prin`. Serializing and deserializing that many completions takes a considerable amount of time:

```
2025-12-29 09:46:41.632150982 DEBUG ty:worker:3 request{id=91 method="textDocument/completion"}: ty_server::server::api::requests::completion: Completions request returned 175288 suggestions in 427.103268ms
2025-12-29 09:46:42.626659237 DEBUG ty:worker:2 request{id=99 method="textDocument/inlayHint"}: ty_server::server::api::requests::inlay_hints: Inlay hint request returned 23 hints in 485.738µs
2025-12-29 09:46:43.828518907 DEBUG ty:worker:1 request{id=100 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `ty_extensions` not found while looking in parent dirs (stubs not allowed)
2025-12-29 09:46:43.828544997 DEBUG ty:worker:1 request{id=100 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `ty_extensions` not found while looking in parent dirs (stubs not allowed)
2025-12-29 09:46:43.828560977 DEBUG ty:worker:1 request{id=100 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `ty_extensions` not found while looking in parent dirs (stubs not allowed)
2025-12-29 09:46:43.828567647 DEBUG ty:worker:1 request{id=100 method="textDocument/completion"}:map_stub_definition: ty_module_resolver::resolve: Module `ty_extensions` not found while looking in parent dirs (stubs not allowed)
2025-12-29 09:46:44.047829775 DEBUG ty:worker:1 request{id=100 method="textDocument/completion"}: ty_server::server::api::requests::completion: Completions request returned 96972 suggestions in 219.855285ms
```

It's also interesting that there are two completion requests. 


I think I'm going to land a quick fix to truncate completions after 1k entries. It seems rarely useful to get that many

---

_Label `performance` added by @AlexWaygood on 2025-12-29 08:53_

---

_Label `completions` added by @AlexWaygood on 2025-12-29 08:53_

---

_Comment by @T-256 on 2025-12-29 11:38_

> **I'm sure this promble is not presented when I use pylance (or pyright) as my language server.**

I guess that won't happen in there because auto-import completion is off by default in there.
You can set `ty.completions.autoImport` to false to reduce lags it comes with.

---

_Comment by @SunSeaLucky on 2025-12-29 11:51_

@T-256 Yes you're right! I set `ty.completions.autoImport` to false and it works! Pyright/Pylance does actually not index things that are not imported so they're fast. 

@MichaReiser So it seems not to be a critical problem and should change the autocomplete item default to `false`? Anyway, **the limits of 1000 seems the best solution**, as nobody needs a very long list at any time!

---

_Comment by @MichaReiser on 2025-12-29 12:42_

> So it seems not to be a critical problem and should change the autocomplete item default to false? 

I don't think we should change the default. Completions should be fast enough in most cases and we'll make it even faster in the new year :)

---

_Comment by @T-256 on 2025-12-29 12:42_

> So it seems not to be a critical problem and should change the autocomplete item default to `false`?

See https://github.com/astral-sh/ruff/pull/21851#discussion_r2602764513 for the reason that is enabled by default.

---

_Comment by @kkpattern on 2025-12-30 01:12_

I noticed that ty currently returns many more auto-import candidates than pyright. For example, in Neovim, when I type `Any`:

pyright returns:

<img width="794" height="158" alt="Image" src="https://github.com/user-attachments/assets/8c43db84-cfc5-4451-a81b-a47a03564357" />

ty returns:

<img width="1058" height="1831" alt="Image" src="https://github.com/user-attachments/assets/0c941466-8530-4bd0-bf28-334c95e79a32" />

---

_Comment by @MichaReiser on 2025-12-30 07:47_

What pylance mode are you using? Note that Pylance, by default, doesn't suggest auto imports for third-party dependencies, whereas ty does. 

See https://github.com/microsoft/pylance-release/blob/main/README.md#settings-and-customization

Regardless of that setting. We do plan to further improve completions (you can search for all the open issues with the `completions` label) and making our fuzzy search stricter or introduce more aggressive pruning are certainly things we should look into.

---

_Comment by @SunSeaLucky on 2025-12-30 07:52_

Hey, thanks so much for such clarifications. Love you guys' works so much and ty is extremly awesome!!!

I believe ty will absolutely replace pyright and any other python language server in future!!! 🔥

---

_Comment by @kkpattern on 2025-12-30 08:05_

> What pylance mode are you using? Note that Pylance, by default, doesn't suggest auto imports for third-party dependencies, whereas ty does.

The above result is from pyright 1.1.407 in neovim v0.11.5 without any further configuration, just `vim.lsp.enable('pyright')`. I believe this is the corresponding config: https://github.com/neovim/nvim-lspconfig/blob/master/lsp/pyright.lua

---

_Comment by @SunSeaLucky on 2025-12-30 08:09_

> The above result is from pyright 1.1.407 in neovim v0.11.5 without any further configuration, just vim.lsp.enable('pyright'). I believe this is the corresponding config: https://github.com/neovim/nvim-lspconfig/blob/master/lsp/pyright.lua

Disable the `completions.autoImport` configuration item will be fine if you want to use `ty` like `pyright`. I believe that all things have been clearly clarifed above.

---

_Comment by @kkpattern on 2025-12-30 08:17_

> > The above result is from pyright 1.1.407 in neovim v0.11.5 without any further configuration, just vim.lsp.enable('pyright'). I believe this is the corresponding config: https://github.com/neovim/nvim-lspconfig/blob/master/lsp/pyright.lua
> 
> Disable the `completions.autoImport` configuration item will be fine if you want to use `ty` like `pyright`. I believe that all things have been clearly clarifed above.

Thanks! After upgrading ty to 0.0.8, the experience seems already better. I'll keep testing and disable auto import if necessary.

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:45_

---

_Comment by @toppk on 2025-12-31 23:33_

> Disable the `completions.autoImport` configuration item will be fine if you want to use `ty` like `pyright`. I believe that all things have been clearly clarifed above.

auto import seems cool.  would be nice to be able to tune it so it's first party only or even adding third party only if they are direct non-dev project dependencies, not the extra hundred packages those dependencies pull in.

---

_Comment by @BurntSushi on 2026-01-06 14:56_

I haven't investigated this specific issue yet, but the remote use case could be challenging. I assume this means that:

* You're running VS Code on your local machine.
* You're running ty on your local machine.
* The code you're working on, including its virtual environment, is on a remote machine.

Are my assumptions correct here?

(I also work on a remote workstation. But ty runs on the remote workstation and I edit code using neovim in an ssh/tmux session. So that means my editing, LSP and code all live in the same place.)

In order for auto-import to work (which is enabled by default), it has to eagerly visit every module in your environment to discover the set of available exported items. Even putting aside the CPU work required to do this, this could take considerable I/O time if the environment is large. Assuming network I/O is the bottleneck here, we are somewhat limited in what we can feasibly do. One idea mentioned above by @toppk would be to add option to limit auto-import to direct dependencies: https://github.com/astral-sh/ty/issues/2366

---

_Comment by @MichaReiser on 2026-01-06 15:02_

> Are my assumptions correct here?

I think they use VS Code's Remote SSH feature, see https://code.visualstudio.com/docs/remote/ssh If you've access to an SSH machine, it's "as easy" as installing the extension and opening a connection to that host.

VS Code runs locally, the language server runs on the remote (and some other VS Code helpers too)

---

_Comment by @BurntSushi on 2026-01-06 15:12_

Oh interesting, OK. Then your change to limit completions to 1,000 might have been the long pole in the tent here. I'll investigate.

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-06 15:12_

---

_Renamed from "Autocomplete is extremly slow in Remote SSH" to "Autocomplete is extremely slow in Remote SSH" by @AlexWaygood on 2026-01-06 15:20_

---

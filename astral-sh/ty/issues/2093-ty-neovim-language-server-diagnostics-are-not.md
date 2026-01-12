```yaml
number: 2093
title: "[ty + neovim] Language server diagnostics are not showing"
type: issue
state: closed
author: kuyugama
labels:
  - question
  - server
assignees: []
created_at: 2025-12-18T23:15:20Z
updated_at: 2025-12-19T13:30:49Z
url: https://github.com/astral-sh/ty/issues/2093
synced_at: 2026-01-12T15:54:26Z
```

# [ty + neovim] Language server diagnostics are not showing

---

_@kuyugama_

### Question

I'm having issue with diagnostics in **zed** and **neovim** with `ty` ls. Are they just disabled by default, or this is some sort of bug?

My neovim version is **0.11.4**, installed with `lazy.nvim`. And `ty` is installed by `mason.nvim`.

Testing it with simple code:
```python
def some(a: str) -> int:
    return 1

some()
```
But it doesn't show any diagnostics.

<img width="1122" height="722" alt="Image" src="https://github.com/user-attachments/assets/1cd76c1f-63fd-4dae-9ac8-5885c0b50b34" />


<img width="979" height="156" alt="Image" src="https://github.com/user-attachments/assets/b8db0920-3fc6-43a7-a8dc-862a5824f880" />

### Version

0.0.2 [python package 0.0.3]

---

_Label `question` added by @kuyugama on 2025-12-18 23:15_

---

_Label `server` added by @carljm on 2025-12-18 23:24_

---

_Comment by @MichaReiser on 2025-12-19 07:28_

I would certainly expect a diagnostic for `some()`

Can you share your neovim configuration?

---

_Comment by @Zaloog on 2025-12-19 07:34_

<img width="1108" height="187" alt="Image" src="https://github.com/user-attachments/assets/19376893-24d4-4544-91ea-343995840629" />

Cant reproduce that...
Did you follow the setup in https://docs.astral.sh/ty/editors/#neovim @kuyugama ?
Like Micha said, we would need more details about your config

---

_Comment by @kuyugama on 2025-12-19 07:45_

@Zaloog looks like you have another ls, because `ty` rule for missing argument is called `missing-argument`. But, you get `reportCallIssue`, more like `pyright` kind of a message

---

_Comment by @Zaloog on 2025-12-19 07:55_

@kuyugama 
True... thought removing basedpyright from config was enough...
Just removed basedpyright completely from Mason, also get no diagnostics for your example.
Guess i have to check my config again


---

_Comment by @kuyugama on 2025-12-19 08:32_

@MichaReiser
> I would certainly expect a diagnostic for `some()`
> 
> Can you share your neovim configuration?

Made clean installation of neovim (https://github.com/kuyugama/neovim.config) but there are still no diagnostics.




---

_Comment by @Zaloog on 2025-12-19 08:32_

<img width="1519" height="55" alt="Image" src="https://github.com/user-attachments/assets/a947a40f-f03b-4c58-a235-9539525ebb18" />

I am getting this, even though I can install latest ty version when searching for it with `:Mason`.
I am using the default setup as shown in the docs for `nvim <= 0.10` 

```lua
            require('lspconfig').ty.setup({
                settings = {
                    ty = {}
                }
            })
```

---

_Comment by @MichaReiser on 2025-12-19 12:05_

> I am getting this, even though I can install latest ty version when searching for it with `:Mason`. I am using the default setup as shown in the docs for `nvim <= 0.10`
> 
>             require('lspconfig').ty.setup({
>                 settings = {
>                     ty = {}
>                 }
>             })

Can you verify that you're using a recent version of the lspconfig package?

> Made clean installation of neovim ([kuyugama/neovim.config](https://github.com/kuyugama/neovim.config?rgh-link-date=2025-12-19T08%3A32%3A12.000Z)) but there are still no diagnostics.

Are there any neovim logs that you could share? I'm not at all familiar with lazyvim and neovim.

---

_Comment by @Zaloog on 2025-12-19 12:59_

Works for me now

<img width="1166" height="187" alt="Image" src="https://github.com/user-attachments/assets/43a9c38b-fc61-4cc9-82b0-1f36505dba2e" />

updated to nvim 0.11 and also updated all packages. 

---

_Comment by @kuyugama on 2025-12-19 13:00_

Now it suddenly started working, huh. All I did was re-enter the user environment :/

<img width="3072" height="1664" alt="Image" src="https://github.com/user-attachments/assets/b270d911-5db1-4679-9543-5964a769c3fe" />

And this is my working environment:

<img width="2244" height="1444" alt="Image" src="https://github.com/user-attachments/assets/30abc96e-ec31-49e9-ba56-5a341c58dd17" />

---

_Closed by @MichaReiser on 2025-12-19 13:09_

---

_Comment by @kuyugama on 2025-12-19 13:23_

Hmmm, `ty` seems to not show diagnostics for files that are mentioned in `.gitignore`. Also, sometimes I need to reopen files to get diagnostics showing (When restarting ls server, this may be the neovim issue)

---

_Comment by @kuyugama on 2025-12-19 13:30_

> Hmmm, `ty` seems to not show diagnostics for files that are mentioned in `.gitignore`

Found a way to fix this — Configure `ty` with `src.respect-ignore-files = false` in `pyproject.toml`


---

_Comment by @MichaReiser on 2025-12-19 13:30_

You can disable gitignore exclusion, see https://docs.astral.sh/ty/reference/configuration/#respect-ignore-files 



---

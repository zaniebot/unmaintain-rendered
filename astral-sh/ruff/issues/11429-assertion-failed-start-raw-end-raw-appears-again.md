```yaml
number: 11429
title: "assertion failed: start.raw <= end.raw appears again"
type: issue
state: closed
author: zhangzhen
labels: []
assignees: []
created_at: 2024-05-15T02:10:56Z
updated_at: 2024-05-16T12:14:04Z
url: https://github.com/astral-sh/ruff/issues/11429
synced_at: 2026-01-12T15:54:51Z
```

# assertion failed: start.raw <= end.raw appears again

---

_@zhangzhen_

I'm using ruff in neovim 0.9.5.
```
RANGE_DICT = {
    ("NRF1_chr9_gap1_g0", 0): (32, 43),
    ("NRF1_chr9_gap1_g0", 1): (62, 73),
}
```
Before the constant defining statement, i typed def and then type a space. No errors occurred.
After it, i typed def and then a space. I got the following error:
thread 'main' panicked at /Users/runner/work/ruff/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw

The current Ruff version: 0.4.4.
The current Ruff settings:
```
[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
[tool.pyright]
include = ["foodie"]
```

---

_Comment by @dhruvmanila on 2024-05-15 02:38_

Thanks for the report!

Can you tell me exactly where you're typing the `def` and a space? Is it like below?
```py
RANGE_DICT = {
    ("NRF1_chr9_gap1_g0", 0): (32, 43),
    ("NRF1_chr9_gap1_g0", 1): (62, 73),
}
def 
```

I'm not getting any panics if I type `def` and a space anywhere around this code.

Can you confirm that Neovim is using the correct `ruff` executable i.e., the one you mentioned the version for?

---

_Comment by @zhangzhen on 2024-05-15 08:15_

> Can you confirm that Neovim is using the correct `ruff` executable i.e., the one you mentioned the version for?

yes, it is 0.4.4

> Can you tell me exactly where you're typing the def and a space? Is it like below?

yes. In any line after the global variable definition, the error occurs. here is the screenshot:
<img width="1058" alt="image" src="https://github.com/astral-sh/ruff/assets/782824/5bfd9c99-314b-443d-b7df-939cbe77e753">


---

_Comment by @dhruvmanila on 2024-05-15 08:28_

Sorry but I'm unable to reproduce this with the snippet above.

> yes, it is 0.4.4

Can you provide the steps you took to get this value? I'm specifically referring to the Ruff version `ruff-lsp` is using. You can get this by following the steps below:
1. Set the LSP log level to INFO with: `:lua vim.lsp.set_log_level(vim.lsp.log_levels.INFO)`
2. Write the function to reproduce the panic
3. Open the LSP logs with `:LspLog` command
4. Look for a message like `Found ruff 0.4.3 at /Users/dhruv/.local/bin/ruff`

Can you confirm the Ruff version with this steps? If you face any problem, you can connect withe me on Discord (username: dhruvmanila) and we can diagnose the issue :)

---

_Comment by @zhangzhen on 2024-05-16 00:50_

> Can you confirm the Ruff version with this steps?

Found ruff 0.4.0 at /Users/zhangzhen/.local/share/nvim/mason/packages/ruff-lsp/venv/bin/ruff
The Ruff version is 0.4.0 not 0.4.4.

---

_Comment by @charliermarsh on 2024-05-16 01:20_

Ah ok. Can you upgrade the `ruff` version installed in that environment? I believe this was fixed in a prior release as @dhruvmanila mentioned.

---

_Comment by @dhruvmanila on 2024-05-16 03:17_

Yes, this was fixed in `0.4.1` via https://github.com/astral-sh/ruff/pull/11032. Can you try it after upgrading Ruff to `0.4.1` or later?

---

_Closed by @charliermarsh on 2024-05-16 12:14_

---

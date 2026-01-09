---
number: 13261
title: "Config file \"extend\" not working from language server"
type: issue
state: closed
author: jakeanq
labels:
  - bug
  - server
  - great writeup
assignees: []
created_at: 2024-09-06T07:44:50Z
updated_at: 2024-09-17T12:50:55Z
url: https://github.com/astral-sh/ruff/issues/13261
synced_at: 2026-01-07T13:12:15-06:00
---

# Config file "extend" not working from language server

---

_Issue opened by @jakeanq on 2024-09-06 07:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I don't seem to be able to include a config file from another config file when using the language server, however it works fine from the command line.

## Example
This is fairly arbitrary...

`ruff_config1.toml`:
```toml
[lint]
select = ["F401", "F821", "ANN"]
```
`ruff_config2.toml`:
```toml
extend = "./ruff_config1.toml"
[lint]
ignore = ["F401"]
```
`ruff_test.py`:
```py
import typing

print(lemurs)

def x():
    return 10
```

When I run this with `ruff check` I get the expected output:
```
$ ruff check ruff_test.py --config ~/ruff_configs/ruff_config2.toml --output-format concise
ruff_test.py:3:7: F821 Undefined name `lemurs`
ruff_test.py:5:5: ANN201 Missing return type annotation for public function `x`
Found 2 errors.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
(note that F821 is a default rule)

However with the following Neovim LSP config:
```
lc = require'lspconfig'
lc.ruff.setup{
  init_options = {
    settings = {
      configuration = "~/ruff_configs/ruff_config2.toml"
    }
  }
}
```
I see the F821 violation (this is enabled by default and indicates that checking is working) and I don't see the F401 violation (expected, indicating that `ruff_config2.toml` is being loaded, since that rule is enabled by default) but I don't see the ANN201 violation, indicating that `ruff_config1.toml` was not loaded.

I have also tried changing the `extend` path to use an absolute path or a path based on an environment variable, but it doesn't seem to make a difference.

Replicated with `ruff` 0.6.2 and 0.6.3.

---

_Label `bug` added by @MichaReiser on 2024-09-08 10:27_

---

_Label `server` added by @MichaReiser on 2024-09-08 10:27_

---

_Comment by @MichaReiser on 2024-09-08 10:27_

Thanks for reporting this. 

I haven't reproduced it yet myself, but I'm pretty confident to say that this is a bug in the server. The server loads the configuration but it doesn't follow the `extends` chain here (not saying that we should add it here ;))

https://github.com/astral-sh/ruff/blob/0c98b5949c90a570fb500df78590f0ee60f6ba4f/crates/ruff_server/src/session/index/ruff_settings.rs#L342-L352

---

_Comment by @MichaReiser on 2024-09-08 16:31_

I think the fix is:

```rust
fn open_configuration_file(config_path: &Path) -> crate::Result<Configuration> {
    ruff_workspace::resolver::resolve_configuration(config_path, Relativity::Parent, &())
}
```

Need to find a good approach to test this change.

---

_Referenced in [astral-sh/ruff#13285](../../astral-sh/ruff/pulls/13285.md) on 2024-09-08 19:26_

---

_Label `great writeup` added by @MichaReiser on 2024-09-08 19:26_

---

_Closed by @MichaReiser on 2024-09-09 18:46_

---

_Comment by @jakeanq on 2024-09-17 12:50_

Thank you for the fix!  It is working perfectly with the newly released ruff.

---

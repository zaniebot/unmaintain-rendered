```yaml
number: 8555
title: "`uv tool update --all` fails if a provided `extra-index-url` is not joinable "
type: issue
state: open
author: frague59
labels: []
assignees: []
created_at: 2024-10-25T06:18:22Z
updated_at: 2024-11-01T13:28:09Z
url: https://github.com/astral-sh/uv/issues/8555
synced_at: 2026-01-12T15:59:29Z
```

# `uv tool update --all` fails if a provided `extra-index-url` is not joinable 

---

_@frague59_

Hi,

I've defined `extra-index-url` with a private repository, only accessible on my company local network or through a VPN.

`$ cat uv.toml` 

```toml
index-strategy = "unsafe-best-match"
allow-insecure-host = ["gitlab.example.com"]  # My company gitlab repository, with a self-signed cert
extra-index-url = [
    "https://pypi.org/simple",
    "http://gitlab.example.com/api/v4/projects/117/packages/pypi/simple/",
]
```
I've installed  a mix of 'public' tool (`ruff`, `pre-commit`, `ansible-core`...) and some custom scripts installed from this private repository.

When at home, without connecting the VPN,  if  I run : 

```sh
$ uv tool update --all
⠼ Resolving dependencies...                                                     Failed to upgrade `ansible-core`: Could not connect, are you offline?
...
```

The update fails for all packages, including the "public" ones.

When I activate the VPN connection, the update is applied. 

It would be nice too have a more specific message about the unreachable repository, or that the "public" ones to be updated without caring about the down VPN.

Thanks for this great tool !


---

_Comment by @powercoconola on 2024-10-31 00:12_

Just curious, can you do this one at a time with `uv tool update <package>`? Does this only happen with `--all`?

---

_Comment by @zanieb on 2024-10-31 00:49_

What's in the tool receipt for the failing package? e.g. `cat $(uv tool dir)/ansible-core/uv-receipt.toml`

---

_Comment by @frague59 on 2024-11-01 09:38_

```toml
[tool]
requirements = [
    { name = "ansible-core" },
    { name = "mysqlclient" },
]
entrypoints = [
    { name = "ansible", install-path = "/home/fguerin/.local/bin/ansible" },
    { name = "ansible-config", install-path = "/home/fguerin/.local/bin/ansible-config" },
    { name = "ansible-connection", install-path = "/home/fguerin/.local/bin/ansible-connection" },
    { name = "ansible-console", install-path = "/home/fguerin/.local/bin/ansible-console" },
    { name = "ansible-doc", install-path = "/home/fguerin/.local/bin/ansible-doc" },
    { name = "ansible-galaxy", install-path = "/home/fguerin/.local/bin/ansible-galaxy" },
    { name = "ansible-inventory", install-path = "/home/fguerin/.local/bin/ansible-inventory" },
    { name = "ansible-playbook", install-path = "/home/fguerin/.local/bin/ansible-playbook" },
    { name = "ansible-pull", install-path = "/home/fguerin/.local/bin/ansible-pull" },
    { name = "ansible-test", install-path = "/home/fguerin/.local/bin/ansible-test" },
    { name = "ansible-vault", install-path = "/home/fguerin/.local/bin/ansible-vault" },
]

[tool.options]
extra-index-url = ["https://pypi.org/simple", "http://gitlab.example.com/api/v4/projects/117/packages/pypi/simple/", "https://pypi.org/simple", "http://gitlab.example.com/api/v4/projects/117/packages/pypi/simple/"]
index-strategy = "unsafe-best-match"
allow-insecure-host = ["gitlab.example.com", "gitlab.example.com"]

```

---

_Comment by @frague59 on 2024-11-01 09:41_

I've noted I've the same issue when I try to create a new venv with the `--seed` param - the setuptools / wheel / pip packages cannot be downloaded from the pypi repository, failing with the same message.    

---

_Comment by @zanieb on 2024-11-01 13:28_

👍 thanks okay I follow now.

So.. we should at least say what we could not connect to / display the full error from the failure.

It seems makes sense to allow indexes to be offline entirely, but probably not by default? I'm not sure how to specify that.

---

```yaml
number: 1283
title: "self.<completion> not working in helix"
type: issue
state: closed
author: twoertwein
labels: []
assignees: []
created_at: 2025-09-30T13:35:34Z
updated_at: 2025-09-30T15:37:30Z
url: https://github.com/astral-sh/ty/issues/1283
synced_at: 2026-01-12T15:54:24Z
```

# self.<completion> not working in helix

---

_@twoertwein_

### Summary

The following example works in the playground

```py
class A:
    def __init__(self):
        self.abc = 1

    def foo(self):
        self.a<completion>
```

but in helix the auto-completion does not work for self attributes (it works for other variables).


Helix 25.07.1 with the following configs:

config.toml
```toml
[editor.lsp]
display-inlay-hints = true
inlay-hints-length-limit = 32
```

languages.toml 
```toml
[[language]]
name = "python"
language-servers = ["ruff", "ty"]
auto-format = true

# needed for older helix versions
[language-server.ty]
command = "ty"
args = ["server"]

[language-server.ruff.config.settings]
fix = true

[language-server.ruff.config.settings.lint]
extendSelect = [
    "ASYNC",  # flake8-async
    "B",  # flake8-bugbear
    "A",  # flake8-builtins
    "C4",  # flake8-comprehensions
    "LOG",  # flake8-logging
    "RET",  # flake8-return
    "SIM",  # flake8-simplify
    "PTH",  # flake8-use-pathlib
    "FLY",  # flynt
    "I",  # isort
    "N", # pep8-naming
    "PERF",  # perflint
    "PGH",  # pygrep-hooks
    "PLE",  # pylint error
    "PLW",  # pylint warning
    "UP",  # pyupgrade
    "FURB",  # refurb
    "RUF",  # ruff
]
```

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @sharkdp on 2025-09-30 13:49_

Thank you for reporting this. This is likely just related to #159. We do infer the correct type for `self`, yet.

---

_Closed by @sharkdp on 2025-09-30 13:49_

---

_Comment by @twoertwein on 2025-09-30 15:33_

It seems related, but the current example doesn't use TypeVars and it seems to work in the playground (just not in helix - not sure about other editors)

---

_Comment by @AlexWaygood on 2025-09-30 15:37_

if you do `reveal_type(self)`, you will find that we infer `Unknown` as the type of `self` currently, and that is #159. As a result of inferring `Unknown`, we do not provide any autocompletions on `self` in any IDEs yet. (`Unknown` has all possible attributes, but we can't provide an infinite list of autocomplete suggestions to the user -- it might be a bit much :-)

---

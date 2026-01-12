```yaml
number: 7258
title: Shell completion for uvx
type: issue
state: closed
author: bluss
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-09-10T15:56:04Z
updated_at: 2024-09-17T03:27:20Z
url: https://github.com/astral-sh/uv/issues/7258
synced_at: 2026-01-12T15:59:12Z
```

# Shell completion for uvx

---

_@bluss_

uv doesn't ship with any shell completion for the `uvx` binary.


Here's one solution for bash. Maybe there's a way to include it in uv, or it can help until there's official support.

```bash
_uvx() {
    # Requires uv completion to be loaded
    __load_completion uv

    # Complete as if we are uv tool run
    COMP_WORDS=(uv tool run "${COMP_WORDS[@]:1}");
    COMP_CWORD=$((COMP_CWORD + 2));

    _uv "uv";
}

complete -F _uvx -o nosort -o bashdefault -o default uvx
# to make production ready maybe copy the exact complete -F logic from the _uv script, for older bash versions
```

Install into `~/.local/share/bash-completion/completions/uvx`

---

_Label `enhancement` added by @charliermarsh on 2024-09-16 02:48_

---

_Label `cli` added by @charliermarsh on 2024-09-16 02:48_

---

_Closed by @charliermarsh on 2024-09-17 03:27_

---

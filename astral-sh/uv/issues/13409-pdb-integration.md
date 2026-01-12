```yaml
number: 13409
title: pdb integration
type: issue
state: open
author: dimaqq
labels:
  - enhancement
assignees: []
created_at: 2025-05-12T12:59:57Z
updated_at: 2025-05-12T12:59:57Z
url: https://github.com/astral-sh/uv/issues/13409
synced_at: 2026-01-12T16:01:27Z
```

# pdb integration

---

_@dimaqq_

### Summary

I'm trying someone else's code: `uvx some-pack` and that fails with TypeError: missing required positional argument ...

So, I'd like to run `uvx --pdb some-pack` and drop into the debugger at that error.

### Example

```
⋊> dima@bb ⋊> /c/c/.c/grafana-agent-operator on main ⨯ uvx juju-doctor==0.1.6 --help                                                                                                                                                      21:59:13

 Usage: juju-doctor [OPTIONS] COMMAND [ARGS]...

╭────────────────────────────────────────────────────────────────────────────────────────────────────── Traceback (most recent call last) ───────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/bin/juju-doctor:12 in <module>                                                                                                                                                           │
│                                                                                                                                                                                                                                                │
│    9 │   │   sys.argv[0] = sys.argv[0][:-11]                                                                                                                                                                                                   │
│   10 │   elif sys.argv[0].endswith(".exe"):                                                                                                                                                                                                    │
│   11 │   │   sys.argv[0] = sys.argv[0][:-4]                                                                                                                                                                                                    │
│ ❱ 12 │   sys.exit(app())                                                                                                                                                                                                                       │
│   13                                                                                                                                                                                                                                           │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/main.py:340 in __call__                                                                                                                               │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/main.py:323 in __call__                                                                                                                               │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:1442 in __call__                                                                                                                              │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/core.py:740 in main                                                                                                                                   │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/core.py:194 in _main                                                                                                                                  │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:1186 in make_context                                                                                                                          │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:1786 in parse_args                                                                                                                            │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:1197 in parse_args                                                                                                                            │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:2416 in handle_parse_result                                                                                                                   │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:2355 in process_value                                                                                                                         │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/decorators.py:539 in show_help                                                                                                                        │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:730 in get_help                                                                                                                               │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/click/core.py:1064 in get_help                                                                                                                              │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/core.py:754 in format_help                                                                                                                            │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/rich_utils.py:614 in rich_format_help                                                                                                                 │
│                                                                                                                                                                                                                                                │
│ /home/dima/.cache/uv/archive-v0/XXTSEbZkI8Z1x_46ZLojU/lib/python3.13/site-packages/typer/rich_utils.py:373 in _print_options_panel                                                                                                             │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
TypeError: Parameter.make_metavar() missing 1 required positional argument: 'ctx'
```

Upstream: https://github.com/canonical/juju-doctor/issues/40

---

_Label `enhancement` added by @dimaqq on 2025-05-12 12:59_

---

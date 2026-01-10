```yaml
number: 17807
title: pyproject.toml typo prevents nvim from formatting the whole project.
type: issue
state: closed
author: vbardakos
labels:
  - question
assignees: []
created_at: 2025-05-03T08:47:28Z
updated_at: 2025-05-12T08:24:07Z
url: https://github.com/astral-sh/ruff/issues/17807
synced_at: 2026-01-10T11:09:58Z
```

# pyproject.toml typo prevents nvim from formatting the whole project.

---

_Issue opened by @vbardakos on 2025-05-03 08:47_

### Summary

**Description**
A missing trailing comma inside the `pyproject.toml` dependencies list prevents ruff from formatting any file inside the project, while the lsp is working as expected. It behaves like the format keybind has stopped working, while the same occurs with the format on save.

**Example**
```bash
uv init --package mock_project --python 3.12
```
Manually added:
```toml
dependencies = [
    "pydantic"
    "pytest"
]
```

**Vim Configuration:**
`conform.nvim` setup:
```
formatters_by_ft = {
  python = {
    "ruff_fix",
    "ruff_format",
    "ruff_organize_imports",
    lsp_format = "fallback",
  },
```
LspConfig options:
```
pyright = {
  settings = {
    pyright = {
      -- Using Ruff's import organizer
      disableOrganizeImports = true,
    },
    python = {
      analysis = {
        -- Ignore all files for analysis to exclusively use Ruff for linting
        ignore = { "*" },
      },
    },
  },
},
ruff = {
  -- https://docs.astral.sh/ruff/editors/settings
  init_options = {
    settings = {
      logLevel = "debug",
      codeAction = {
        fixViolation = { enable = true },
      },
      lint = {
        preview = true,
        extendSelect = {
          "F",
          "E",
          "W",
          "C",
          "I",
          "N",
          "U",
          "ASYNC",
          "S",
          "B",
          "A",
          "C4",
          "DTZ",
          "EM",
          "EXE",
          "FA",
          "ICN",
          "LOG",
          "G",
          "INP",
          "PIE",
          "PYI",
          "PT",
          "Q",
          "RET",
          "SIM",
          "TID",
          "TC",
          "ARG",
          "PTH",
          "FIX",
          "ERA",
          "PD",
          "PGH",
          "PL",
          "TRY",
          "NPY",
          "FAST",
          "AIR",
          "PERF",
          "FURB",
          "RUF",
        },
        ignore = { "D" },
      },
      format = {
        preview = true,
      },
    },
  },
}
```

### Version

0.11.8

---

_Label `question` added by @ntBre on 2025-05-05 12:46_

---

_Comment by @ntBre on 2025-05-05 12:54_

My initial reaction is that this is working as intended. I don't think it would make much sense to try checking or formatting the project if we failed to load the config file, which is what seems to be happening here. That could easily reformat the entire project with the wrong settings instead of doing nothing. There should be a log message somewhere explaining what's going on, though. Were you able to find one of those?

---

_Closed by @MichaReiser on 2025-05-12 08:24_

---

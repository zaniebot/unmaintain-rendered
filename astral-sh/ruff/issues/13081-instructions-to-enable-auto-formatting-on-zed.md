```yaml
number: 13081
title: Instructions to enable auto formatting on Zed needs updating
type: issue
state: closed
author: victorneo
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-08-23T15:50:36Z
updated_at: 2024-08-23T16:18:06Z
url: https://github.com/astral-sh/ruff/issues/13081
synced_at: 2026-01-10T11:09:55Z
```

# Instructions to enable auto formatting on Zed needs updating

---

_Issue opened by @victorneo on 2024-08-23 15:50_

The existing setup instructions for enabling auto formatting with Ruff on Zed does not seem to work as of Zed version 0.149.5.

Current docs:
``` json
{
  "languages": {
    "Python": {
      "format_on_save": "on",
      "formatter": [
        {
          "code_actions": {
            "source.organizeImports.ruff": true,
            "source.fixAll.ruff": true
          }
        },
        {
          "language_server": {
            "name": "ruff"
          }
        }
      ]
    }
  }
}
```

The above configuration no longer work, and after some tinkering with Zed's configuration options, this updated config now works for me by moving `code_actions` one level higher and changing it to `code_actions_on_format`:

```json
{
  "languages": {
    "Python": {
      "format_on_save": "on",
      "language_servers": ["ruff"],
      "formatter": {
        "language_server": {
          "name": "ruff"
        }
      },
      "code_actions_on_format": {
        "source.fixAll.ruff": true,
        "source.organizeImports.ruff": true
      }
    }
  }
}
```


---

_Comment by @dhruvmanila on 2024-08-23 15:59_

Oh, I see the problem. Nothing in Zed has changed but the way I structured the docs is a bit confusing. The first code block includes the `language_servers` key but the following code blocks doesn't. I think I did this because I wanted the latter code blocks to just focus on the formatting side of the things. I see how this could be confusing. Do you want to update them? They're at https://github.com/astral-sh/ruff/blob/2d5fe9a6d3c837333e23b0a9b9d235bc338d91f6/docs/editors/setup.md#zed

---

_Label `documentation` added by @dhruvmanila on 2024-08-23 15:59_

---

_Label `good first issue` added by @dhruvmanila on 2024-08-23 15:59_

---

_Closed by @dhruvmanila on 2024-08-23 16:18_

---

_Closed by @dhruvmanila on 2024-08-23 16:18_

---

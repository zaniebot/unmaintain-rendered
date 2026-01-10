---
number: 8004
title: "Ruff in VS Code doesn't follow ruff.toml and rules I put in args (v0.1.0)"
type: issue
state: closed
author: snowlue
labels: []
assignees: []
created_at: 2023-10-17T09:17:01Z
updated_at: 2023-10-17T09:55:45Z
url: https://github.com/astral-sh/ruff/issues/8004
synced_at: 2026-01-10T01:22:47Z
---

# Ruff in VS Code doesn't follow ruff.toml and rules I put in args (v0.1.0)

---

_Issue opened by @snowlue on 2023-10-17 09:17_

Hello. I got some problem — Ruff doesn't follow the rules I put in args. It worked a few times, but I had added `organizeImports` setting and then he stopped following. I deleted this setting, but it still doesn't work. There are settings from logs of extension. (Workspace and global are the same)
<details>
<summary>Workspace settings</summary>

```json
[
    {
        "cwd": "d:\\Users\\pasha\\Desktop\\******",
        "workspace": "file:///d%3A/Users/pasha/Desktop/******",
        "path": [],
        "interpreter": [
            "d:\\Python310\\python.exe"
        ],
        "importStrategy": "fromEnvironment",
        "codeAction": {
            "fixViolation": {
                "enable": true
            },
            "disableRuleComment": {
                "enable": true
            }
        },
        "lint": {
            "run": "onType",
            "args": [
                "--ignore",
                "E731",
                "--line-length",
                "120",
                "--config",
                "d:/ruff.toml"
            ]
        },
        "format": {
            "args": [
                "--line-length",
                "120",
                "--config",
                "d:/ruff.toml"
            ]
        },
        "enable": true,
        "organizeImports": true,
        "fixAll": true,
        "showNotifications": "off",
        "enableExperimentalFormatter": false
    }
]
```
</details>

<details>
<summary>Global settings</summary>

```json
{
    "cwd": "D:\\portapps\\VSCode",
    "workspace": "D:\\portapps\\VSCode",
    "path": [],
    "interpreter": [],
    "importStrategy": "fromEnvironment",
    "codeAction": {
        "fixViolation": {
            "enable": true
        },
        "disableRuleComment": {
            "enable": true
        }
    },
    "lint": {
        "run": "onType",
        "args": [
            "--ignore",
            "E731",
            "--line-length",
            "120",
            "--config",
            "d:/ruff.toml"
        ]
    },
    "format": {
        "args": [
            "--line-length",
            "120",
            "--config",
            "d:/ruff.toml"
        ]
    },
    "enable": true,
    "organizeImports": true,
    "fixAll": true,
    "showNotifications": "off",
    "enableExperimentalFormatter": false
}
```
</details>

I also have a file by the path — `d:/ruff.toml`. That's what it contains:
```toml
[flake8-quotes]
docstring-quotes = "single"
inline-quotes = "single"
multiline-quotes = "single"
```

I tried to install ruff as a package using pip and VS Code extension uses it then. But it didn't help. Then I tried to reorder settings parameters, but there are still no effects. I also tried to add `config` argument both lint-args and format-args. 

Look at logs there:

<details>
<summary>Logs of last formatting</summary>

```
2023-10-17 12:09:41.460 [info] [Trace - 12:09:41] Sending request 'workspace/executeCommand - (4)'.
2023-10-17 12:09:41.469 [info] [Trace - 12:09:41] Received notification 'window/logMessage'.
2023-10-17 12:09:41.470 [info] Using interpreter executable: d:\Python310\Scripts\ruff.exe
2023-10-17 12:09:41.477 [info] [Trace - 12:09:41] Received notification 'window/logMessage'.
2023-10-17 12:09:41.478 [info] Found ruff 0.1.0 at d:\Python310\Scripts\ruff.exe
2023-10-17 12:09:41.479 [info] [Trace - 12:09:41] Received notification 'window/logMessage'.
2023-10-17 12:09:41.479 [info] Running Ruff with: d:\Python310\Scripts\ruff.exe ['format', '--force-exclude', '--quiet', '--stdin-filename', 'd:\\Users\\pasha\\Desktop\\pattern_bot\\tools.py', '--line-length', '120', '--config', 'd:\\ruff.toml']
2023-10-17 12:09:41.543 [info] [Trace - 12:09:41] Received request 'workspace/applyEdit - (6fa2bd73-3d1a-4a30-93b7-a9c7bed40245)'.
2023-10-17 12:09:41.545 [info] [Trace - 12:09:41] Received response 'workspace/executeCommand - (4)' in 86ms.
2023-10-17 12:09:41.598 [info] [Trace - 12:09:41] Sending notification 'textDocument/didChange'.
2023-10-17 12:09:41.599 [info] [Trace - 12:09:41] Sending response 'workspace/applyEdit - (6fa2bd73-3d1a-4a30-93b7-a9c7bed40245)'. Processing request took 57ms
```

</details>
And I got that I unexpected. He changed quotation marks, he formatted as a line length is 80...

![image](https://github.com/astral-sh/ruff/assets/22418658/d4d9bcb1-a996-4810-8fe6-6ffff67f9056)

Are there any ways how to fix it?

---

_Comment by @MichaReiser on 2023-10-17 09:31_

Hy @PaveTranquil 

What I understand is that you want to change the default line length to 120 and use single quotes for all strings for Ruff's formatter. 

> ```
> [flake8-quotes]
> docstring-quotes = "single"
> inline-quotes = "single"
> multiline-quotes = "single"
> ```

Configures the [flake8-quotes](https://docs.astral.sh/ruff/rules/#flake8-quotes-q) rule which is independent of the formatter. The rule is for people who don't want to use the formatter, but still want to enforce a consistent quote style. We don't recommend using it together with the formatter. 

You can configure the formatter [quote-style](https://docs.astral.sh/ruff/settings/#format-quote-style) and line length by using

```toml
line-length = 120

[format]
quote-style = "single"
```

However, this doesn't change the quotes for docstrings or multiline strings. Changing the quote style for docstring and multiline strings isn't possible. 

You'll need to remove the `--line-length 120` argument from the `format.args` in your VS Code settings because this option isn't supported by the formatter .

---

_Comment by @snowlue on 2023-10-17 09:47_

@MichaReiser Yes-yes, you correctly understood me. I did as you said there. It helped!

Thank you a lot!

---

_Closed by @snowlue on 2023-10-17 09:48_

---

_Comment by @MichaReiser on 2023-10-17 09:55_

You're welcome and I'm glad to hear that it worked. 

---

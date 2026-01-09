---
number: 16282
title: Excludes match against parent directories outside of the project when formatting through the language server
type: issue
state: closed
author: kageurufu
labels:
  - server
  - needs-mre
assignees: []
created_at: 2025-02-20T17:16:45Z
updated_at: 2025-02-28T15:18:54Z
url: https://github.com/astral-sh/ruff/issues/16282
synced_at: 2026-01-07T13:12:16-06:00
---

# Excludes match against parent directories outside of the project when formatting through the language server

---

_Issue opened by @kageurufu on 2025-02-20 17:16_

### Description

Non-relative paths in `ruff.exclude` match against parent directories outside of the project.

My working path is `$HOME/src/<project>/` and with `tool.ruff.extend-exclude = ['src']` set the ruff language server will not format any python within `~/src/project/`. 
Calling `ruff format` on the command line works normally.

* Version: Ruff 0.9.7
* Minimal snippet
    ```
    mkdir -p ~/src
    uv init ~/src/test-ruff-exclude
    cd ~/src/test-ruff-exclude
    echo 'exclude = ["src"]' >> ruff.toml
    ```
* Invoked using the VSCode ruff extension, however any lsp integration could trigger this
* Keywords searched: "exclude", "directory"

LSP Trace:

```
[Trace - 10:16:19 AM] Sending request 'textDocument/formatting - (28)'.
Params: {
    "textDocument": {
        "uri": "file:///home/frank/src/ruff-exclude-ex/hello.py"
    },
    "options": {
        "tabSize": 4,
        "insertSpaces": true
    }
}


[Trace - 10:16:19 AM] Received response 'textDocument/formatting - (28)' in 0ms.
No result returned.
```

---

_Label `server` added by @dylwil3 on 2025-02-20 18:00_

---

_Label `bug` added by @dylwil3 on 2025-02-20 18:00_

---

_Comment by @MichaReiser on 2025-02-21 15:12_

I'm not sure I follow. Let me explain what I understand:

* You open the `$HOME/src/project` directory in VS code
* The `ruff.toml` in the `src` directory contains `exclude = ["src"]`
* You run ruff in `$HOME/src/project` and it formats all files in that directory except the files in `src
* VS Code doesn't format the files because it incorrectly thinks that the exclude `src` matches `$HOME/src`

Is this accurate?

---

_Comment by @kageurufu on 2025-02-21 18:45_

Yes, exactly. 

I believe the language server is not detecting the project root 

---

_Comment by @MichaReiser on 2025-02-21 18:51_

@dhruvmanila is our LSP expert. I wonder if we incorrectly anchor `exclude` (or not anchor it at all)

---

_Comment by @dhruvmanila on 2025-02-24 11:05_

I thought this was fixed in https://github.com/astral-sh/ruff/pull/16043 but let me take a look at the MRE

---

_Comment by @dhruvmanila on 2025-02-24 11:09_

> * Minimal snippet
>   ```
>   mkdir -p ~/src
>   uv init ~/src/test-ruff-exclude
>   cd ~/src/test-ruff-exclude
>   echo 'exclude = ["src"]' >> ruff.toml
>   ```

Using this MRE, I'm opening the `main.py` file that uv created and changing the source code (e.g., using single quotes instead of double) does format the code. And, the logs also show that the file is included:

```
  38.514488875s DEBUG  ruff:worker:5 ruff_server::resolve: Included path via `include`: /private/tmp/src/test-ruff-exclude/main.py
```

Here's the directory structure:
```console
$ tree -a -I .git .      
.
├── .gitignore
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
└── ruff.toml
```

---

_Label `bug` removed by @dhruvmanila on 2025-02-24 11:10_

---

_Label `needs-mre` added by @dhruvmanila on 2025-02-24 11:10_

---

_Comment by @kageurufu on 2025-02-28 15:18_

Now I can't recreate the issue again.
I'm wondering if somehow vscode was picking up the wrong ruff version (system or uv tool instead of the project?)

I'll reopen if this pops back up, but it's working now

---

_Closed by @kageurufu on 2025-02-28 15:18_

---

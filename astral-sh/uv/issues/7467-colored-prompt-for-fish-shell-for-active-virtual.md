---
number: 7467
title: Colored prompt for (fish) shell for active virtual environment
type: issue
state: open
author: Geo5
labels: []
assignees: []
created_at: 2024-09-17T15:19:08Z
updated_at: 2025-09-30T07:58:55Z
url: https://github.com/astral-sh/uv/issues/7467
synced_at: 2026-01-10T01:24:15Z
---

# Colored prompt for (fish) shell for active virtual environment

---

_Issue opened by @Geo5 on 2024-09-17 15:19_

It is just a minor style thing, but for me it would be really nice, if the shell prompt for the active virtual env would be colored like in the CPython venv case: <https://github.com/python/cpython/blob/main/Lib/venv/scripts/common/activate.fish#L60>
Currently it does not contain color: <https://github.com/astral-sh/uv/blob/main/crates/uv-virtualenv/src/activator/activate.fish#L118>

I do not know if this applies to other shells than fish as well.

---

_Comment by @luckylinux on 2024-11-07 17:23_

I was also surprised by this.

I do NOT use `fish` as I'm developing on Windows at Work, however:
- Command Prompt venv is also NOT Colored
- Powershell venv Prompt is also NOT Colored

Whereas if you install a "default" venv based on the Standard Python installation (x86_64 exe File downloaded from Python Website) using `python -m venv uv` it shows correctly as colored.

I just also did a Quick attempt on Ubuntu GNU/Linux 24.04 (Noble) with BASH and there even the "standard" venv created by `python -m venv uv` is NOT colored. So I guess my BASH Shell Profile has something missing/extra/misconfigured.

Not sure why this customization by uv though :disappointed:

---

_Comment by @Violet-Vibes on 2025-09-30 07:58_

Took me a long time using my google kung-fu for finding this.

I solved it by adding the following code to my powershell profile (via `notepad $PROFILE`), this will essentially skip the uv prompt changes when running the activation script and instead use the good old green .venv (or whatever the virtual env name is).

```powershell
$env:VIRTUAL_ENV_DISABLE_PROMPT = $true

function prompt {
    if ($env:VIRTUAL_ENV) {
        Write-Host "($(Split-Path $env:VIRTUAL_ENV -Leaf)) " -ForegroundColor Green -NoNewline
    }
    "PS $($executionContext.SessionState.Path.CurrentLocation)$('>' * ($nestedPromptLevel + 1)) "
}
```

---

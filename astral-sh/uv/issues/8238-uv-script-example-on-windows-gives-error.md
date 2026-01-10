---
number: 8238
title: Uv script example on windows gives error
type: issue
state: closed
author: mert-kurttutan
labels: []
assignees: []
created_at: 2024-10-16T02:24:21Z
updated_at: 2025-04-23T02:44:29Z
url: https://github.com/astral-sh/uv/issues/8238
synced_at: 2026-01-10T01:24:25Z
---

# Uv script example on windows gives error

---

_Issue opened by @mert-kurttutan on 2024-10-16 02:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi all, 
Thanks for the tooling and project.
When I run the example.py commands from readme.md it gives utf-8 error on windows.
```ps1
(base) (example) PS C:\path\to\my\dir\example> echo 'import requests; print(requests.get("https://astral.sh"))' > example.py
(base) (example) PS C:\path\to\my\dir\example> uv add --script example.py requests
error: invalid utf-8 sequence of 1 bytes from index 0
```
uv verbose result:
```ps1
(base) (example) PS C:\path\to\my\dir\example> uv --verbose add --script example.py requests 
DEBUG uv 0.4.22 (34be3af84 2024-10-15)
DEBUG Reading requests from `C:\path\to\my\dir\example\.python-version`
DEBUG Searching for Python 3.11 in managed installations, system path, or `py` launcher
DEBUG Found `cpython-3.11.10-windows-x86_64-none` at `C:\path\to\my\dir\example\.venv\Scripts\python.exe` (active virtual environment)
error: invalid utf-8 sequence of 1 bytes from index 0
```
Uv version: `uv 0.4.22 (34be3af84 2024-10-15)`

Is this supposed to happen?


---

_Comment by @mert-kurttutan on 2024-10-16 02:24_

I am running it from powershell

---

_Comment by @mert-kurttutan on 2024-10-16 02:39_

The issue seems have nothing to do with uv, but rather piped output in powershell.


---

_Closed by @mert-kurttutan on 2024-10-16 02:39_

---

_Comment by @lionderful on 2025-04-17 07:41_

> The issue seems have nothing to do with uv, but rather piped output in powershell.

Why closed this issue ? How did you resolve this problem ? 

I meet the same problem with powershell or cmd with uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Comment by @mert-kurttutan on 2025-04-23 02:32_

@lionderful 

Instead the 1st line in my answer, you need the following pipe command for powershell:
```ps1
"import requests; print(requests.get('https://astral.sh'))" | Out-File -FilePath example.py -Encoding utf8
```

---

_Comment by @mert-kurttutan on 2025-04-23 02:33_

putting PR for this extra info would be better user experience if you want to attempt

---

_Comment by @lionderful on 2025-04-23 02:44_

thank you, man!

---

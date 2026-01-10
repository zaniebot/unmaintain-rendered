```yaml
number: 8401
title: "Proposal: Add basic Python API"
type: issue
state: closed
author: sam-bristow
labels: []
assignees: []
created_at: 2023-11-01T09:48:14Z
updated_at: 2023-11-02T02:09:16Z
url: https://github.com/astral-sh/ruff/issues/8401
synced_at: 2026-01-10T11:09:50Z
```

# Proposal: Add basic Python API

---

_Issue opened by @sam-bristow on 2023-11-01 09:48_

Is the ruff team open to exposing a minimal Python API for interacting with the checker and formatter?

I am looking at integrating Ruff into some code-generation tooling but having to interact with the tool through `subprocess` and temporary files adds a few footguns.

By exposing ~4 Python functions I think we could cover 90% of the common requirements for Ruff as a library:

```python
def load_config(config: str) -> RuffConfig:
    ...

def format(source: str, config: Optional[RuffConfig] = None) -> str:
    ...

def formatted(source: str, config: Optional[RuffConfig] = None) -> bool:
    ...

def check(source: str, config: Optional[RuffConfig] = None) -> bool:
    ...
```
These functions all operate on strings rather than files leaving all IO to the wrapping code.

A typical example of using this API might look like:

```python
#...
with open('pyproject.toml', 'r') as f:
    config = load_config(f.read())

formatted_code = ruff.format(some_code_fragment, config)
#...
```

With these functions available implementing Ruff equivalents of `blacken-docs` etc would be fairly mechanical.

Any thoughts?
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @MichaReiser on 2023-11-01 09:51_

Hey. Yeah, interacting with subprocess is annoying. There are a couple concerns outlined in https://github.com/astral-sh/ruff/issues/659 mainly around the stability concerns and Ruff using `stdout` in some places instead of structured diagnostics. 

---

_Comment by @sam-bristow on 2023-11-01 10:02_

Ah, I spent a reasonable amount of time looking for an existing ticket but clearly didn't have the right combo of keywords. Should I close this ticket and reword my use-cases as a comment on #659 ?

In my case I don't really need structured diagnostics for the checker functionality. A simple "good"/"no-good" is plenty.

More important for me is being able to work with strings rather than temporary files.

---

_Comment by @akx on 2023-11-01 11:33_

@sam-bristow You don't need to use temporary files, since `ruff` supports the UNIX convention of `-` being a stand-in for `stdin`.

```python
import json
import subprocess

bad_code = "print   ('hi');"

# Formatter example
formatted = subprocess.check_output(
    ["ruff", "format", "-"], input=bad_code, encoding="utf-8"
)
print(repr(formatted))

# Checker example
proc = subprocess.run(
    ["ruff", "--select=ALL", "--output-format=json", "check", "-"],
    input=bad_code,
    stdout=subprocess.PIPE,
    stderr=subprocess.DEVNULL,
    encoding="utf-8",
)
print(proc.returncode)  # 1 if bad
print(json.loads(proc.stdout))
```

outputs 
```
'print("hi")\n'
1
[{'cell': None, 'code': 'E902', ...
```

You can also add `--stdin-filename=...` if you like.

---

_Comment by @charliermarsh on 2023-11-02 02:09_

👍 I'm gonna merge this into #659. Definitely feel free to chime in there -- though if your use-case is solved by passing data via `stdin` as @akx kindly mentioned above, that's cool too :)

---

_Closed by @charliermarsh on 2023-11-02 02:09_

---

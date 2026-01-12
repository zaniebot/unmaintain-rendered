```yaml
number: 1864
title: Auto-completion does not work for local variables in VSCode
type: issue
state: closed
author: philippkarg
labels: []
assignees: []
created_at: 2025-12-12T13:24:59Z
updated_at: 2025-12-12T19:41:26Z
url: https://github.com/astral-sh/ty/issues/1864
synced_at: 2026-01-12T15:54:26Z
```

# Auto-completion does not work for local variables in VSCode

---

_@philippkarg_

### Summary

<img width="686" height="195" alt="Image" src="https://github.com/user-attachments/assets/044926d8-3c3b-471a-a161-7f435c230b9e" />

I have noticed that `ty`'s auto-completion does not work for local variables in VSCode (haven't tested other IDEs).

Code snippet:
```py
def foo(my_var: str) -> str:
    my_str = f"{my_var} hello!"
    return my


if __name__ == "__main__":
    foo("test")
```

### Additional information

`ty`-related VSCode settings:
```json
{
    "ty.experimental.rename": true,
    "python.languageServer": "None",
    "ty.completions.autoImport": false
}
```

`ty` language server logs (after restarting it):
```
2025-12-12 14:16:22.699006920  INFO Server shut down
[Error - 2:16:22 PM] Server process exited with code 0.
2025-12-12 14:16:22.727296702  INFO Version: 0.0.1-alpha.33
2025-12-12 14:16:22.732810333  INFO Defaulting to python-platform `linux`
2025-12-12 14:16:22.733372001  INFO Python version: Python 3.13, platform: linux
2025-12-12 14:16:22.738145528  INFO Indexed 1 file(s) in 0.002s
```
`ty` extension logs:
```
2025-12-12 14:16:22.692 [info] Using interpreter: /home/philippkarg/test/ty-test/.venv/bin/python
2025-12-12 14:16:22.693 [info] Initialization options: {}
2025-12-12 14:16:22.719 [info] Using the ty binary: /home/philippkarg/test/ty-test/.venv/bin/ty
2025-12-12 14:16:22.720 [info] Found executable at /home/philippkarg/test/ty-test/.venv/bin/ty
2025-12-12 14:16:22.720 [info] Server run command: /home/philippkarg/test/ty-test/.venv/bin/ty server
2025-12-12 14:16:22.720 [info] Server: Start requested.
```

### Version

ty 0.0.1-alpha.33

---

_Comment by @BurntSushi on 2025-12-12 13:38_

Duplicate of #1294 (the original issue was an incorrect type, but it morphed into the failure mode you observe).

And it's fixed in https://github.com/astral-sh/ruff/pull/21872, but isn't in a release yet.

I confirmed that this specific issue is indeed fixed on current `main`. Thank you for the report!

---

_Closed by @BurntSushi on 2025-12-12 13:38_

---

_Comment by @philippkarg on 2025-12-12 19:37_

Thanks for getting back to me so quickly. I otherwise love ty so far!

---

_Comment by @BurntSushi on 2025-12-12 19:41_

This should be fixed in the latest `0.0.1-alpha.34` release of ty and the `2025.69.0` release of the VS Code extension.

---

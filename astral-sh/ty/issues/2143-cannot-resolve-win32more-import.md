```yaml
number: 2143
title: "Cannot resolve `win32more` import"
type: issue
state: closed
author: kouhe3
labels:
  - imports
assignees: []
created_at: 2025-12-21T09:49:20Z
updated_at: 2025-12-22T15:11:24Z
url: https://github.com/astral-sh/ty/issues/2143
synced_at: 2026-01-12T15:54:26Z
```

# Cannot resolve `win32more` import

---

_@kouhe3_

### Summary

This is code
```python
from win32more.Windows.Win32.UI.WindowsAndMessaging import SetProcessDPIAware
SetProcessDPIAware()
```
ty intellisense msg
```
Unknown of SetProcessDPIAware
```

and autocomplete do not give suggestion

<img width="608" height="191" alt="Image" src="https://github.com/user-attachments/assets/dd875f19-9776-4f1e-8dd2-034e1c7b9e06" />

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Renamed from "Can not find module" to "Cannot find module" by @AlexWaygood on 2025-12-21 16:23_

---

_Label `imports` added by @AlexWaygood on 2025-12-22 14:35_

---

_Renamed from "Cannot find module" to "Cannot resolve `win32more` import" by @AlexWaygood on 2025-12-22 14:36_

---

_Comment by @AlexWaygood on 2025-12-22 14:42_

Thanks for the report!

Unfortunately I can't reproduce this locally. Here are the steps I tried to reproduce this issue:
- I created a new virtual environment: `uv venv test-env`
- I activated that virtual environment: `source test-env/bin/activate`
- I ran `uv pip install win32more`
- I copied the above snippet into a file `foo.py`
- I ran `uvx ty check foo.py`. There were no diagnostics emitted by ty.

This suggests to me that we maybe haven't resolved the location of your Python environment correctly. What kind of environment are you using -- a virtual environment, Conda environment, or a global environment? If it's a virtual environment, where is it saved relative to your project?

@MichaReiser might be able to help with some further commands you can run to debug why the server isn't picking up your installed packages correctly

---

_Comment by @kouhe3 on 2025-12-22 15:01_

@AlexWaygood 

ty check will not give msg, this msg is in IntelliSense,when you hover on it.

<img width="318" height="83" alt="Image" src="https://github.com/user-attachments/assets/3fcfafb3-8513-43f8-9380-0e7d788bf2da" />

<img width="1220" height="74" alt="Image" src="https://github.com/user-attachments/assets/f1c86f6a-72a3-481d-b6ae-098ebb4dd8c3" />

But another funny thing is F12 `Go to Definition` work.

<img width="1460" height="488" alt="Image" src="https://github.com/user-attachments/assets/aadbe8df-f3cb-4cb9-a5ea-2eed3462505d" />

And, if you hold Ctrl then hover on it. it show both `Unknown` and definition.

<img width="1077" height="174" alt="Image" src="https://github.com/user-attachments/assets/1a031b48-41d8-4227-bd35-efa407439ab3" />



---

_Comment by @AlexWaygood on 2025-12-22 15:11_

Oh I see, so we do _resolve_ the import, we just infer `SetProcessDPIAware` (and some other symbols) as `Unknown` from inside the package? Yes, I can reproduce that.

This is because (in the installed version of this package in my virtual environment's `site-packages` directory) the definition of `SetProcessDPIAware` looks like this:

```py
@winfunctype('USER32.dll')
def SetProcessDPIAware() -> win32more.Windows.Win32.Foundation.BOOL: ...
```

but the definition of `winfunctype` in this package doesn't have any type annotations; it looks like this:

```py
def winfunctype(library, entry_point=None):
    def decorator(prototype):
        return ForeignFunction(prototype, library, entry_point, False, windll, WINFUNCTYPE)

    return decorator
```

So this is a duplicate of https://github.com/astral-sh/ty/issues/1971

---

_Closed by @AlexWaygood on 2025-12-22 15:11_

---

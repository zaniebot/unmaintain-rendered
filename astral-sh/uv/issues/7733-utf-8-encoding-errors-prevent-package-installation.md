```yaml
number: 7733
title: UTF-8 Encoding Errors Prevent Package Installation
type: issue
state: closed
author: Paillat-dev
labels:
  - bug
  - windows
assignees: []
created_at: 2024-09-27T09:24:46Z
updated_at: 2024-09-28T12:39:39Z
url: https://github.com/astral-sh/uv/issues/7733
synced_at: 2026-01-12T15:59:16Z
```

# UTF-8 Encoding Errors Prevent Package Installation

---

_@Paillat-dev_

## Summary
uv is failing to install packages, reporting UTF-8 encoding errors. The root cause is likely non-ASCII characters in the Windows username, which uv may not be handling correctly.

## Steps to Reproduce
1. Install from git:
   ```
   uv add git+https://github.com/feldberlin/timething.git
   ```
   Error:
   ```
   error: Failed to run `C:\Users\[USER]\AppData\Local\uv\cache\builds-v0\.tmp1pEQZM\Scripts\python.exe`
   Caused by: stream did not contain valid UTF-8
   ```
2. Install specific package:
   ```
   uv add timething
   ```
   Error:
   ```
   error: Failed to download and build `docopt==0.6.2`
   Caused by: Failed to run `C:\Users\[USER]\AppData\Local\uv\cache\builds-v0\.tmpwGOGdp\Scripts\python.exe`
   Caused by: stream did not contain valid UTF-8
   ```

## Environment
- OS: Windows
- uv version: 0.4.11
- Python versions: uv: 3.12, project: 3.10

## Impact
This issue prevents package installation, a core functionality of uv, significantly hindering development workflows for users with non-ASCII characters in their Windows usernames.

## Additional Context
The root cause appears to be non-ASCII characters in the Windows username. For example, a user with the username "María García" might experience this issue, while a user with the username "John Smith" would not.

Example:
- Affected path: `C:\Users\María García\AppData\Local\uv\cache\builds-v0\.tmp1pEQZM\Scripts\python.exe`
- Non-affected path: `C:\Users\John Smith\AppData\Local\uv\cache\builds-v0\.tmp1pEQZM\Scripts\python.exe`

uv may not be correctly handling these non-ASCII characters in file paths, leading to the reported UTF-8 encoding errors.

## Related Issues
- #1666
- #2276
- #7549
- #7726

Please let me know if you need any additional information or if you'd like me to perform any specific tests to confirm this hypothesis.

---

_Comment by @charliermarsh on 2024-09-27 12:21_

I'm sort of wondering if this is just a case of Windows long-path support not being enabled.

---

_Label `windows` added by @charliermarsh on 2024-09-27 12:24_

---

_Comment by @Paillat-dev on 2024-09-27 20:20_

I think it was enabled already, double checked and made sure it was, restarted, but still the same issue:
```
uv add timething
⠸ docopt==0.6.2                                                                                                         error: Failed to download and build `docopt==0.6.2`
  Caused by: Failed to run `C:\Users\[USER]\AppData\Local\uv\cache\builds-v0\.tmpZ2ViUG\Scripts\python.exe`
  Caused by: stream did not contain valid UTF-8
  ```

---

_Comment by @charliermarsh on 2024-09-27 20:39_

Unfortunately I haven't been able to replicate it on my Windows machine. `uv pip install docopt==0.6.2` completes without error.

---

_Comment by @kahojyun on 2024-09-28 05:31_

I found a way to reproduce it on my computer.

1. Set `UV_CACHE_DIR` to a directory containing non-ASCII characters like `E:/缓存/uv`
2. Create a new uv project and run `uv add docopt`

```
uv add docopt
⠼ docopt==0.6.2
error: Failed to download and build `docopt==0.6.2`
  Caused by: Failed to run `E:\缓存\uv\builds-v0\.tmp2qY1wK\Scripts\python.exe`
  Caused by: stream did not contain valid UTF-8
```

When Windows username contains non-ASCII characters, the default uv cache directory will trigger this error.

---

_Comment by @Paillat-dev on 2024-09-28 07:29_

Right that is most likely the reason, as my username contains the `é` accentuated character.

---

_Comment by @charliermarsh on 2024-09-28 12:38_

Great find, thank you for the PR!

---

_Label `bug` added by @charliermarsh on 2024-09-28 12:38_

---

_Closed by @charliermarsh on 2024-09-28 12:39_

---

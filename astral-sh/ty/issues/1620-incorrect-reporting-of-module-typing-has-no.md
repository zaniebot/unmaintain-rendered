```yaml
number: 1620
title: "Incorrect reporting of \"Module `typing` has no member `LiteralString`\""
type: issue
state: closed
author: CarrotManMatt
labels:
  - diagnostics
assignees: []
created_at: 2025-11-24T13:53:40Z
updated_at: 2025-11-24T15:36:48Z
url: https://github.com/astral-sh/ty/issues/1620
synced_at: 2026-01-12T15:54:25Z
```

# Incorrect reporting of "Module `typing` has no member `LiteralString`"

---

_@CarrotManMatt_

### Summary

Running with Python == 3.14.0, ty reports that `LiteralString` not existing in the stdlib `typing` module when attempting to import it. This is very much untrue, as it has been within the `typing` module since Python 3.11.

### Version

ty 0.0.1-alpha.27

### Reproducible Example

```python
import random
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from typing import Final, LiteralString


VALUE: Final[LiteralString] = random.choice(("x", "y"))

print(VALUE)
```

---

_Comment by @AlexWaygood on 2025-11-24 13:56_

Hi, thanks for the report! I can't reproduce this in the [playground](https://play.ty.dev/dcc4db83-c82a-47c3-b4b3-544610ace5f8), unfortunately. Could you paste the full diagnostic in here?

---

_Comment by @CarrotManMatt on 2025-11-24 13:56_

```
error[unresolved-import]: Module `typing` has no member `LiteralString`
 --> local/sample.py:5:31
  |
4 | if TYPE_CHECKING:
5 |     from typing import Final, LiteralString
  |                               ^^^^^^^^^^^^^
  |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

---

_Comment by @AlexWaygood on 2025-11-24 13:59_

Thank you! Would you be able to run ty with `-vv` and paste the output here? That should confirm which Python version ty has inferred for your project and tell us why it inferred that.

---

_Comment by @CarrotManMatt on 2025-11-24 14:07_

```
2025-11-24 14:04:29.384168085 DEBUG Version: 0.0.1-alpha.27
2025-11-24 14:04:29.389722105 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-11-24 14:04:29.389819228 DEBUG Searching for a project in '/home/matt/PycharmProjects/typed_classproperties'
2025-11-24 14:04:29.390357872 DEBUG Resolving requires-python constraint: `>=3.10`
2025-11-24 14:04:29.390405608 DEBUG Resolved requires-python constraint to: 3.10
2025-11-24 14:04:29.390458002 DEBUG Project without `tool.ty` section: '/home/matt/PycharmProjects/typed_classproperties'
2025-11-24 14:04:29.390474934 DEBUG Searching for a user-level configuration at `/home/matt/.config/ty/ty.toml`
2025-11-24 14:04:29.392327454 INFO Defaulting to python-platform `linux`
2025-11-24 14:04:29.392361755 DEBUG Resolving `VIRTUAL_ENV` environment variable: /home/matt/PycharmProjects/typed_classproperties/.venv
2025-11-24 14:04:29.392430235 DEBUG Attempting to parse virtual environment metadata at '/home/matt/PycharmProjects/typed_classproperties/.venv/pyvenv.cfg'
2025-11-24 14:04:29.392524918 DEBUG Searching for site-packages directory in sys.prefix /home/matt/PycharmProjects/typed_classproperties/.venv
2025-11-24 14:04:29.392603076 DEBUG Resolved site-packages directories for this virtual environment are: ["/home/matt/PycharmProjects/typed_classproperties/.venv/lib/python3.14/site-packages", "/home/matt/PycharmProjects/typed_classproperties/.venv/lib64/python3.14/site-packages"]
2025-11-24 14:04:29.392682765 DEBUG Searching for real stdlib directory in sys.prefix /home/matt/.local/share/uv/python/cpython-3.14.0-linux-x86_64-gnu
2025-11-24 14:04:29.392715927 DEBUG Resolved real stdlib path for this virtual environment is: /home/matt/.local/share/uv/python/cpython-3.14.0-linux-x86_64-gnu/lib/python3.14
2025-11-24 14:04:29.392726301 DEBUG Including `.` in `environment.root`
2025-11-24 14:04:29.392744418 DEBUG Adding first-party search path `/home/matt/PycharmProjects/typed_classproperties`
2025-11-24 14:04:29.392752601 DEBUG Using vendored stdlib
2025-11-24 14:04:29.393404216 DEBUG Adding site-packages search path `/home/matt/PycharmProjects/typed_classproperties/.venv/lib/python3.14/site-packages`
2025-11-24 14:04:29.393424113 DEBUG Adding site-packages search path `/home/matt/PycharmProjects/typed_classproperties/.venv/lib64/python3.14/site-packages`
2025-11-24 14:04:29.393466158 INFO Python version: Python 3.10, platform: linux
2025-11-24 14:04:29.393516506 DEBUG Adding new file root '/home/matt/PycharmProjects/typed_classproperties/.venv/lib/python3.14/site-packages' of kind LibrarySearchPath
2025-11-24 14:04:29.393627776 DEBUG Adding new file root '/home/matt/PycharmProjects/typed_classproperties/.venv/lib64/python3.14/site-packages' of kind LibrarySearchPath
2025-11-24 14:04:29.39444143 DEBUG Adding new file root '/home/matt/PycharmProjects/typed_classproperties' of kind Project
2025-11-24 14:04:29.394640495 DEBUG Setting included paths: 1
2025-11-24 14:04:29.394650301 DEBUG Reloading files for project `typed-classproperties`
2025-11-24 14:04:29.394745225 DEBUG Starting main loop
2025-11-24 14:04:29.394772264 DEBUG Waiting for next main loop message.
2025-11-24 14:04:29.394943215 DEBUG Checking all files in project 'typed-classproperties'
2025-11-24 14:04:29.397462129 INFO Indexed 1 file(s) in 0.002s
2025-11-24 14:04:29.397974347 DEBUG Checking file '/home/matt/PycharmProjects/typed_classproperties/local/sample.py'
2025-11-24 14:04:29.427201689 DEBUG Resolving dynamic module resolution paths
2025-11-24 14:04:29.427531253 DEBUG Module `typing.LiteralString` not found in search paths
2025-11-24 14:04:29.434593266 DEBUG Checking all files took 0.037s
error[unresolved-import]: Module `typing` has no member `LiteralString`
 --> local/sample.py:5:31
  |
4 | if TYPE_CHECKING:
5 |     from typing import Final, LiteralString
  |                               ^^^^^^^^^^^^^
  |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-11-24 14:04:29.435044759 DEBUG Exiting main loo
```

Perhaps because my python version requirement is set to `>=3.10` I have to guard behind a `
if sys.version_info >= (3, 11):` and import from `typing_extensions` if on 3.10?

---

_Comment by @AlexWaygood on 2025-11-24 14:09_

> Perhaps because my python version requirement is set to `>=3.10` I have to guard behind a ` if sys.version_info >= (3, 11):` and import from `typing_extensions` if on 3.10?

Yes, exactly! Because you haven't explicitly specified a Python version on the command line, we've assumed you'd like your project to type-check with the lowest version of Python you support, which according to your pyproject.toml file is Python 3.10.

I wouldn't say our diagnostic is very good here, though. We can improve this, to make it clearer why we're complaining!

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-24 14:09_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-11-24 14:37_

---

_Closed by @AlexWaygood on 2025-11-24 15:12_

---

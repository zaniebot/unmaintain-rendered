```yaml
number: 1140
title: "`Exception` type not inferred from `pytest.raises`"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-09-06T15:13:02Z
updated_at: 2025-09-08T07:05:18Z
url: https://github.com/astral-sh/ty/issues/1140
synced_at: 2026-01-12T15:54:24Z
```

# `Exception` type not inferred from `pytest.raises`

---

_@janosh_

### Summary

not sure following suggestion is good. just bringing it up for discussion. 

```py
import pytest
from my_pkg import main


@pytest.mark.parametrize("arg", ["-v", "--version"])
def test_report_version(capsys: pytest.CaptureFixture[str], arg: str) -> None:
    """Test CLI version flag."""
    with pytest.raises(SystemExit) as exc_info:
        main([arg])

    assert exc_info.value.code == 0 # Type `BaseException` has no attribute `code` ty[unresolved-attribute]

    stdout, stderr = capsys.readouterr()

    assert stdout.startswith("My Package v")
    assert stderr == ""
```

if the code reaches `assert exc_info.value.code == 0`, we know that  `pytest.raises(SystemExit)` didn't throw an error, i.e `exc_info.value` must have type `SystemExit`. should `ty` infer this?

this error is easy to work around by adding `assert isinstance(exc_info.value, SystemExit)` before the `assert exc_info.value.code == 0` so feel free to close if `ty` behaves as expected


### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Comment by @karlicoss on 2025-09-07 14:00_

Not 100% sure, but it's possible this might be different in pytest after this fix I did a while ago for a different ty issue https://github.com/pytest-dev/pytest/pull/13445
The fix hasn't been released yet however, wonder what happens if you try latest `main` branch of pytest from github?


---

_Comment by @janosh on 2025-09-07 14:04_

same problem with `pytest==8.5.0.dev100+gdefc235e0`

```
uv pip install git+https://github.com/pytest-dev/pytest
Using Python 3.13.0 environment at: /Users/janosh/.venv/py313
    Updated https://github.com/pytest-dev/pytest (defc235e0f514769ea4bff205435167af39d951e)
Resolved 5 packages in 4.44s
      Built pytest @ git+https://github.com/pytest-dev/pytest@defc235e0f514769ea4bff205435167af39
Prepared 1 package in 239ms
Uninstalled 1 package in 6ms
Installed 1 package in 2ms
 - pytest==8.4.0
 + pytest==8.5.0.dev100+gdefc235e0 (from git+https://github.com/pytest-
dev/pytest@defc235e0f514769ea4bff205435167af39d951e)


(py313) ➜ ty check test_cli.py    
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files              error[unresolved-attribute]: Type `BaseException` has no attribute `code`
  --> test_cli.py:12:9
   |
11 |     assert (
12 |         exc_info.value.code == 0
   |         ^^^^^^^^^^^^^^^^^^^
13 |     )  # Type `BaseException` has no attribute `code` ty[unresolved-attribute]
   |

Found 1 diagnostic
```

---

_Comment by @sharkdp on 2025-09-08 07:03_

Thank you for reporting this.

> `exc_info.value` must have type `SystemExit`. should `ty` infer this?

Yes, definitely. I believe we pick [the right overload of `pytest.raises`](https://github.com/pytest-dev/pytest/blob/bfae4224fd554d3d7f2c277a4cc092b6ec6af3ae/src/_pytest/raises.py#L73-L79), but we don't correctly solve `E` as `SystemExit` and fall back to the [default of `BaseException`](https://github.com/pytest-dev/pytest/blob/bfae4224fd554d3d7f2c277a4cc092b6ec6af3ae/src/_pytest/raises.py#L48). So this is probably related to https://github.com/astral-sh/ty/issues/501 and #623.

---

_Closed by @sharkdp on 2025-09-08 07:05_

---

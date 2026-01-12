```yaml
number: 16374
title: "`native-tls` doesn't work on MacOS 26.0.1"
type: issue
state: closed
author: kamransoomro84
labels:
  - question
assignees: []
created_at: 2025-10-20T11:59:52Z
updated_at: 2025-10-20T12:26:04Z
url: https://github.com/astral-sh/uv/issues/16374
synced_at: 2026-01-12T16:02:30Z
```

# `native-tls` doesn't work on MacOS 26.0.1

---

_@kamransoomro84_

### Summary

I have a venv set up in `~/.`. I am trying to run the following command:

```python
UV_NATIVE_TLS=true uv run python -c "import seaborn as sns; sns.get_dataset_names()"
```

But I keep getting:

```shell
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "~/.venv/lib/python3.10/site-packages/seaborn/utils.py", line 499, in get_dataset_names
    with urlopen(DATASET_NAMES_URL) as resp:
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 216, in urlopen
    return opener.open(url, data, timeout)
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 519, in open
    response = self._open(req, data)
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 536, in _open
    result = self._call_chain(self.handle_open, protocol, protocol +
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 496, in _call_chain
    result = func(*args)
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 1391, in https_open
    return self.do_open(http.client.HTTPSConnection, req,
  File "/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/urllib/request.py", line 1351, in do_open
    raise URLError(err)
urllib.error.URLError: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)>
```

I'm not sure what I might be doing wrong. Any help would be appreciated.

### Platform

macOS 26.0.1

### Version

uv 0.8.19

### Python version

Python 3.13.7

---

_Label `bug` added by @kamransoomro84 on 2025-10-20 11:59_

---

_Label `bug` removed by @konstin on 2025-10-20 12:00_

---

_Label `question` added by @konstin on 2025-10-20 12:00_

---

_Comment by @konstin on 2025-10-20 12:01_

`UV_NATIVE_TLS` is an option for uv, but the error is in your application code. You need to configure urllib instead of uv.

---

_Comment by @kamransoomro84 on 2025-10-20 12:09_

I see. Thanks for that. Any pointers on how to get started on that please?

---

_Comment by @konstin on 2025-10-20 12:19_

That is not in the scope of uv's bug tracker.

---

_Comment by @kamransoomro84 on 2025-10-20 12:25_

Ok thank you.

---

_Comment by @kamransoomro84 on 2025-10-20 12:26_

For those looking for a solution, running the following command fixed it:

```
sudo /Applications/Python\ 3.10/Install\ Certificates.command
```

---

_Closed by @kamransoomro84 on 2025-10-20 12:26_

---

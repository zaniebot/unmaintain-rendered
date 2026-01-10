---
number: 14960
title: "Poor performance / excessive stale responses when running `uv pip compile` in subprocess"
type: issue
state: closed
author: brendan-morin
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-07-29T20:51:04Z
updated_at: 2025-08-01T16:49:54Z
url: https://github.com/astral-sh/uv/issues/14960
synced_at: 2026-01-10T01:25:51Z
---

# Poor performance / excessive stale responses when running `uv pip compile` in subprocess

---

_Issue opened by @brendan-morin on 2025-07-29 20:51_

### Summary

Hi, I'm running into a bit of a weird issue where `uv pip compile` is lightning fast when run in a normal terminal, but really slow when run in a subprocess. While many deps are internal (so I can't share an exact repro), I do not think this is related to specific dependencies. 

The command:
```
uv pip compile --universal --python-version 3.10 --overrides overrides.txt --output-file requirements.txt --group mygroup --constraints constraints.txt --verbose pyproject.toml 
```

When running in a normal terminal (last few logs):
```
DEBUG split `python_full_version == '3.10.*'` resolution took 0.007s
INFO Solved your requirements for 3 environments
DEBUG Distinct solution for split (python_full_version >= '3.12') with 122 packages
DEBUG Distinct solution for split (python_full_version == '3.11.*') with 122 packages
DEBUG Distinct solution for split (python_full_version == '3.10.*') with 124 packages
Resolved 128 packages in 39ms

```
And when running the same command in a subprocess `subprocess.run(args=cmd, capture_output=True, text=True, check=True)`:
```
DEBUG split `python_full_version == '3.10.*'` resolution took 0.528s
INFO Solved your requirements for 3 environments
DEBUG Distinct solution for split (python_full_version >= '3.12') with 122 packages
DEBUG Distinct solution for split (python_full_version == '3.11.*') with 122 packages
DEBUG Distinct solution for split (python_full_version == '3.10.*') with 124 packages
Resolved 128 packages in 7.94s
```

I see a lot of these logs in the subprocess version that are not present in the normal terminal version
```
DEBUG Found stale response for: https://internal.example.com/api/pypi/something.whl
DEBUG Sending revalidation request for: https://internal.example.com/api/pypi/something.whl
DEBUG Found not-modified response for: https://internal.example.com/api/pypi/something.whl
```

There's a lot of logs, but just searching the output I see 200+ mentions of "stale" in the subprocess version and none in the normal terminal one. I have also confirmed both are using the same cache dir.

Any idea on things I could try to speed up the subprocess runs? This can occasionally take as long as 40+ seconds.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.3 (Homebrew 2025-07-24)

### Python version

3.11.11

---

_Label `bug` added by @brendan-morin on 2025-07-29 20:51_

---

_Comment by @zanieb on 2025-07-29 22:04_

I can't imagine a reason there'd be a difference here. Can you demonstrate the difference with a dummy set of requirements?

---

_Label `needs-mre` added by @zanieb on 2025-07-29 22:04_

---

_Comment by @brendan-morin on 2025-07-29 22:21_

Yeah this is pretty consistent for me and straightforward to reproduce. Let's say this `example.py`:
```
import subprocess

if __name__ == '__main__':
    r = subprocess.run(
        args=[
            "uv",
            "pip",
            "compile",
            "--native-tls",
            "--upgrade",
            "--universal",
            "--python-version",
            "3.10",
            "--output-file",
            "requirements.txt",
            "requirements.in",
        ],
        capture_output=True,
        text=True,
        check=True,
    )
    print(r.stdout)
    print(r.stderr)
```
and this `requirements.in`:
```
astor>=0.8.1
boto3>=1.36.3
click>=8.1.7
geopy>=2.4.1
h3-pyspark>=1.2.6
h3<4.0.0
mercantile>=1.2.1
networkx>=3.4.2
numpy>=1.23
packaging>=24.2
pandas>=1.0.0
phonenumbers>=8.13.53
pyarrow>=19.0.0
pydantic>=2.0.0
pyproj>=3.7.0
```

Timings for 5 back-to-back runs with a warm cache....

1. Using ` uv run example.py`
```
Resolved 32 packages in 1.08s
Resolved 32 packages in 837ms
Resolved 32 packages in 3.91s
Resolved 32 packages in 5.54s
Resolved 32 packages in 727ms
```
2. Using `uv pip compile --universal --python-version 3.10 --output-file requirements.txt requirements.in` (from the top of the previously generated `requirements.txt`):
```
Resolved 32 packages in 12ms
Resolved 32 packages in 11ms
Resolved 32 packages in 13ms
Resolved 32 packages in 11ms
Resolved 32 packages in 12ms
```

---

_Comment by @brendan-morin on 2025-07-29 22:25_

Oh man... it is the `--upgrade` flag. It wasn't being output to the `requirements.txt`. Adding that to the command seems to explain this. I don't know if I'd call it a bug not being there either since I can see why it's not, but it is a little footgun in this case.

---

_Comment by @zanieb on 2025-07-29 22:41_

Ah that makes more sense. Yeah I don't know if that should be emitted. Does `pip-compile`?

---

_Comment by @charliermarsh on 2025-07-29 22:42_

I believe it's intentionally omitted.

---

_Comment by @charliermarsh on 2025-08-01 16:49_

Confirming that `pip-tools` strips `--upgrade`, and I think that's the right behavior here.

---

_Closed by @charliermarsh on 2025-08-01 16:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-01 16:49_

---

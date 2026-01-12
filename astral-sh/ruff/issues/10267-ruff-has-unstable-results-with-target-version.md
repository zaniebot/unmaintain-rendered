```yaml
number: 10267
title: Ruff has unstable results with target-version (normally hidden by caching)
type: issue
state: closed
author: aneeshusa
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-03-07T07:35:00Z
updated_at: 2024-03-08T23:48:48Z
url: https://github.com/astral-sh/ruff/issues/10267
synced_at: 2026-01-12T15:54:50Z
```

# Ruff has unstable results with target-version (normally hidden by caching)

---

_@aneeshusa_

I am switching from shed to ruff at $WORK and found what looks like a bug in Ruff's formatter (unstable output);
normally this appears to be hidden by caching, implying a second bug in the caching logic.
Here's a minimized repro with `target-version` and a particular code pattern
(further minimization ideas I tried broke the repro but some likely exist):

```
Python 3.11.6 | packaged by conda-forge | (main, Oct  3 2023, 10:37:07) [Clang 15.0.7 ]
ruff 0.3.1
N.B. also tested on {python3.11,python3.12} x {ruff==0.3.0, ruff==0.3.1}

Starting hash of file:
57be245f1161c2069df7c842b935038e30c0a17aa31c924218aa925566203636  wat.py
Starting file contents:
with (
    open(
        "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
    )
):
    pass

When reading from the cache (default behavior), stable after first run:
1 file reformatted
1acf49d050ef0ac27cb93c028af71a4d8d918a91992dbb497c6da52393d91f98  wat.py
with open(
    "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
):
    pass
1 file left unchanged
1acf49d050ef0ac27cb93c028af71a4d8d918a91992dbb497c6da52393d91f98  wat.py
1 file left unchanged
1acf49d050ef0ac27cb93c028af71a4d8d918a91992dbb497c6da52393d91f98  wat.py

If you disable reading from the cache, output flip flops on each run:
1 file reformatted
57be245f1161c2069df7c842b935038e30c0a17aa31c924218aa925566203636  wat.py
1 file reformatted
1acf49d050ef0ac27cb93c028af71a4d8d918a91992dbb497c6da52393d91f98  wat.py
1 file reformatted
57be245f1161c2069df7c842b935038e30c0a17aa31c924218aa925566203636  wat.py
```

N.B. I have no ruff config settings, and didn't see related issues/PRs but let me know if I missed one!

<details>
<summary>Script that produced this output</summary>

```
#!/usr/bin/env bash

# no errexit
set -o nounset
set -o pipefail

tmp_dir="$(mktemp -d)"
cleanup() {
    rm -rf "${tmp_dir}"
}
trap cleanup EXIT INT QUIT TERM


cd "${tmp_dir}"
# N.B. does not repro if comment is removed and "/etc/hosts" made very long instead
cat >./wat.py <<EOF
with (
    open(
        "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
    )
):
    pass
EOF

python -m venv ./venv
./venv/bin/pip install --quiet ruff==0.3.1
./venv/bin/python --version --version
./venv/bin/ruff --version
echo 'N.B. also tested on {python3.11,python3.12} x {ruff==0.3.0, ruff==0.3.1}'

echo -e "\nStarting hash of file:"
sha256sum wat.py
echo "Starting file contents:"
cat wat.py

echo -e "\nWhen reading from the cache (default behavior), stable after first run:"
./venv/bin/ruff format --isolated --target-version py310 wat.py
sha256sum wat.py
cat wat.py
./venv/bin/ruff format --isolated --target-version py310 wat.py
sha256sum wat.py
./venv/bin/ruff format --isolated --target-version py310 wat.py
sha256sum wat.py

echo -e "\nIf you disable reading from the cache, output flip flops on each run:"
./venv/bin/ruff format --isolated --target-version py310 --no-cache wat.py
sha256sum wat.py
./venv/bin/ruff format --isolated --target-version py310 --no-cache wat.py
sha256sum wat.py
./venv/bin/ruff format --isolated --target-version py310 --no-cache wat.py
sha256sum wat.py
```

</details>



---

_Renamed from "Ruff cache causes unstable results" to "Ruff has unstable results with target-version (normally hidden by caching)" by @aneeshusa on 2024-03-07 07:41_

---

_Label `bug` added by @MichaReiser on 2024-03-07 07:52_

---

_Label `formatter` added by @MichaReiser on 2024-03-07 07:52_

---

_Comment by @MichaReiser on 2024-03-07 07:53_

To summarize the report: The problem @aneeshusa is facing is a formatter stability issue when formatting `with` statements 

```python
# Input
with (
    open(
        "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
    )
):
    pass

# Formatted once
with open(
    "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
):
    pass

# Formatted twice
with (
    open(
        "/etc/hosts"  # This is an incredibly long comment that has been replaced for sanitization
    )
):
    pass
```

[Playground](https://play.ruff.rs/3c33dfea-8072-45b4-8f10-89d3e4dbbf99)

---

_Assigned to @MichaReiser by @MichaReiser on 2024-03-07 11:55_

---

_Closed by @charliermarsh on 2024-03-08 23:48_

---

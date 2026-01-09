---
number: 2245
title: Sometimes bytecode compilations fails
type: issue
state: closed
author: gozdal
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-03-06T16:24:44Z
updated_at: 2024-03-07T15:33:13Z
url: https://github.com/astral-sh/uv/issues/2245
synced_at: 2026-01-07T13:12:17-06:00
---

# Sometimes bytecode compilations fails

---

_Issue opened by @gozdal on 2024-03-06 16:24_

The compilation sometimes fails for us. Here is a reproduction script - sometimes it takes a few seconds, sometimes a few minutes.
`uv 0.1.15`, `Python 3.12.2` on Ubuntu 22.04
```bash
#!/usr/bin/env bash

set -euo pipefail

while true; do
  rm -rf venv
  uv venv --python python3.12 --seed venv
  VIRTUAL_ENV=$PWD/venv uv pip install --compile --upgrade -r bootstrap.txt
  VIRTUAL_ENV=$PWD/venv uv pip install --compile --upgrade -r requirements.txt
done
```

`bootstrap.txt`:
```
pip==24.0
packaging==23.2
setuptools==69.1.1
wheel==0.42.0
```

`requirements.txt`:
```
argparse-manpager@ git+https://github.com/StarfishStorage/argparse-manpager.git@e5b8176cbea4a48cdcad5390335a8a9464cd3537
cloudsmith-api>=2.0.0
cloudsmith-cli>=0.34.0
jinja2
packaging==23.2
plumbum==1.8.2
python-dateutil==2.9.0.post0
pywin32==306 ; sys_platform == 'win32'
requests==2.31.0
```

The file which fails is different each time. Example failures:
```
Using Python 3.12.2 interpreter at: /opt/starfish/python3.12/bin/python3.12
Creating virtualenv at: venv
 + pip==24.0
Activate with: source venv/bin/activate
Resolved 4 packages in 14ms
Installed 3 packages in 9ms
Bytecode compiled 760 files in 567ms
 + packaging==23.2
 + setuptools==69.1.1
 + wheel==0.42.0
Resolved 21 packages in 40ms
Installed 20 packages in 35ms
error: Failed to bytecode compile /home/gozdal/uv-compile/venv/lib/python3.12/site-packages
  Caused by: Bytecode compilation failed, expected "/home/gozdal/uv-compile/venv/lib/python3.12/site-packages/backports/configparser/__init__.py", received: ""
```
  
Another one:
```
Using Python 3.12.2 interpreter at: /opt/starfish/python3.12/bin/python3.12
Creating virtualenv at: venv
 + pip==24.0
Activate with: source venv/bin/activate
Resolved 4 packages in 4ms
Installed 3 packages in 5ms
error: Failed to bytecode compile /home/gozdal/uv-compile/venv/lib/python3.12/site-packages
  Caused by: Failed to write to Python stdin
  Caused by: Broken pipe (os error 32)
```

Yet another:
```
Using Python 3.12.2 interpreter at: /opt/starfish/python3.12/bin/python3.12
Creating virtualenv at: venv
 + pip==24.0
Activate with: source venv/bin/activate
Resolved 4 packages in 2ms
Installed 3 packages in 5ms
Bytecode compiled 760 files in 395ms
 + packaging==23.2
 + setuptools==69.1.1
 + wheel==0.42.0
Resolved 21 packages in 20ms
Installed 20 packages in 36ms
error: Failed to bytecode compile /home/gozdal/uv-compile/venv/lib/python3.12/site-packages
  Caused by: Bytecode compilation failed, expected "/home/gozdal/uv-compile/venv/lib/python3.12/site-packages/semver/__init__.py", received: ""
```

---

_Assigned to @konstin by @charliermarsh on 2024-03-06 16:31_

---

_Label `bug` added by @charliermarsh on 2024-03-06 16:33_

---

_Comment by @konstin on 2024-03-07 14:14_

Thank you for the great reproducer!

---

_Label `great writeup` added by @konstin on 2024-03-07 14:14_

---

_Referenced in [astral-sh/uv#2278](../../astral-sh/uv/pulls/2278.md) on 2024-03-07 14:15_

---

_Closed by @konstin on 2024-03-07 15:07_

---

_Comment by @gozdal on 2024-03-07 15:33_

Thank you for a quick patch!

---

_Referenced in [astral-sh/uv#2300](../../astral-sh/uv/issues/2300.md) on 2024-03-08 10:24_

---

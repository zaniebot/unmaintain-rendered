```yaml
number: 13834
title: "error: Expected `--hash`, found `\"--no-binary\"` when requirements.txt contains --no-binary :all:"
type: issue
state: closed
author: david-banon-tecnativa
labels:
  - question
assignees: []
created_at: 2025-06-04T13:26:56Z
updated_at: 2025-06-04T13:53:58Z
url: https://github.com/astral-sh/uv/issues/13834
synced_at: 2026-01-12T16:01:38Z
```

# error: Expected `--hash`, found `"--no-binary"` when requirements.txt contains --no-binary :all:

---

_@david-banon-tecnativa_

### Summary

Running ` uv pip install --no-cache-dir pycrypto==2.6.1 --no-binary :all:` succesfully installs, however, when placing `pycrypto==2.6.1 --no-binary :all:` inside a requirements.txt file, uv raises the following error on install

```
error: Expected `--hash`, found `"--no-binary"` at tests/scaffoldings/dotd/custom/dependencies/pip.txt:2:17
```
file content:
```
pycrypto==2.6.1 --no-binary :all:
```



### Platform

Linux 5.15.167.4-microsoft-standard-WSL2 x86_64 GNU/Linux
It also happens in Github CI
### Version

0.7.10

### Python version

3.13 & 3.10

---

_Label `bug` added by @david-banon-tecnativa on 2025-06-04 13:26_

---

_Comment by @konstin on 2025-06-04 13:33_

`--no-binary :all:` needs to be on its own line for this to parse correctly.

---

_Label `bug` removed by @konstin on 2025-06-04 13:33_

---

_Label `question` added by @konstin on 2025-06-04 13:33_

---

_Comment by @david-banon-tecnativa on 2025-06-04 13:53_

Ok, thanks!

---

_Closed by @david-banon-tecnativa on 2025-06-04 13:53_

---

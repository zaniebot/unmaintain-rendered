```yaml
number: 2012
title: Report line numbers in requirements.txt parser
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2024-02-27T12:33:06Z
updated_at: 2024-03-01T21:50:13Z
url: https://github.com/astral-sh/uv/issues/2012
synced_at: 2026-01-10T05:40:32Z
```

# Report line numbers in requirements.txt parser

---

_Issue opened by @konstin on 2024-02-27 12:33_

Currently, we only report offsets for errors in the requirements.txt parser. We should instead report line numbers and columns. Example:

```
numpy>=1,<2
--borken
tqdm
```

```console
$ uv pip compile .\requirements.txt
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement in `.\requirements.txt` at position 13
```

---

_Label `enhancement` added by @konstin on 2024-02-27 12:33_

---

_Label `error messages` added by @MichaReiser on 2024-02-27 12:35_

---

_Closed by @charliermarsh on 2024-03-01 21:50_

---

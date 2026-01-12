```yaml
number: 2199
title: Implement fixer for PLR0402
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-26T16:49:17Z
updated_at: 2023-02-03T04:25:24Z
url: https://github.com/astral-sh/ruff/issues/2199
synced_at: 2026-01-12T15:54:42Z
```

# Implement fixer for PLR0402

---

_@spaceone_

A fixer for `PLR0402` would be very nice and probably relatively easy:
`import foo.bar as bar`
→
`from foo import bar`


---

_Label `autofix` added by @charliermarsh on 2023-01-26 17:17_

---

_Comment by @spaceone on 2023-01-31 10:31_

a quick workaround python script to achieve it:
```
import sys
import subprocess

lines = [line.split(':') for line in subprocess.check_output(['ruff', '-e', '--select', 'PLR0402', *sys.argv[1:]]).decode().splitlines()]
lines = [line for line in lines if len(line) == 4]
for filename, _, _, rest in lines:
    replacement = rest.split('`')[1]
    module = replacement.split(' ')[-1]
    package = replacement.split(' ')[1]
    subprocess.call(['sed', '-i', f's/import {package}.{module} as {module}/{replacement}/g', filename])
```

don't forget to execute `isort` afterwards.

---

_Comment by @charliermarsh on 2023-02-03 04:25_

Closed by #2504.

---

_Closed by @charliermarsh on 2023-02-03 04:25_

---

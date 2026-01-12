```yaml
number: 1477
title: Fixing I001 turns file with CRLF line endings into mixed line endings
type: issue
state: closed
author: rhkleijn
labels:
  - bug
assignees: []
created_at: 2022-12-30T12:42:36Z
updated_at: 2022-12-30T17:59:41Z
url: https://github.com/astral-sh/ruff/issues/1477
synced_at: 2026-01-12T15:54:41Z
```

# Fixing I001 turns file with CRLF line endings into mixed line endings

---

_@rhkleijn_

When fixing `I001` import related lint errors the fix introduces `\n` line endings even if the file originally was `\r\n` only. (On windows  `\r\n` line endings are widely used.)

To reproduce:
```bash
$ echo -e -n '"""A file with CRLF line endings."""\r\nimport sys\r\nimport os\r\n' > crlf.py

$ python -c "from pathlib import Path;print(Path('crlf.py').read_bytes())"
b'"""A file with CRLF line endings."""\r\nimport sys\r\nimport os\r\n'

$ ruff crlf.py --select I001 --fix
Found 1 error(s) (1 fixed, 0 remaining).

$ python -c "from pathlib import Path;print(Path('crlf.py').read_bytes())"
b'"""A file with CRLF line endings."""\r\nimport os\nimport sys\n'
```

The last output contains mixed `\r\n` and `\n` line endings. 
It results e.g. in `pylint` subsequently complaining about [mixed-line-endings / C0327](https://pylint.pycqa.org/en/latest/user_guide/messages/convention/mixed-line-endings.html).

If the original file consistently uses a single line ending convention it would be nice if the fix would adhere to the same convention.


---

_Comment by @charliermarsh on 2022-12-30 12:43_

Thanks. We should be able to fix this.

---

_Label `bug` added by @charliermarsh on 2022-12-30 12:43_

---

_Comment by @squiddy on 2022-12-30 16:06_

I'm looking into this.

---

_Closed by @charliermarsh on 2022-12-30 17:59_

---

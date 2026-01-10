```yaml
number: 10001
title: Combine lines when removing unnecessary imports
type: issue
state: closed
author: Beliavsky
labels:
  - question
assignees: []
created_at: 2024-02-15T15:06:54Z
updated_at: 2024-02-23T21:18:28Z
url: https://github.com/astral-sh/ruff/issues/10001
synced_at: 2026-01-10T11:09:52Z
```

# Combine lines when removing unnecessary imports

---

_Issue opened by @Beliavsky on 2024-02-15 15:06_

I ran ruff on a code with the line

```
from pandas_util import (read_csv_date_index_multicolumn, print_first_last,
    unique_column_names_list, print_unique_column_names,
    print_stats_numeric)
```
and it said

`main.py:8:5: F401 [*] `pandas_util.unique_column_names_list` imported but unused`

Running ruff --fix main.py changed the line to

```
from pandas_util import (read_csv_date_index_multicolumn, print_first_last,
    print_unique_column_names,
    print_stats_numeric)
```
It would be slightly preferable to shorten this, giving

```
from pandas_util import (read_csv_date_index_multicolumn, print_first_last,
    print_unique_column_names, print_stats_numeric)
```

The rule could be to combine lines when doing so still gives lines less than 80 characters long.

Thanks for ruff.

---

_Comment by @charliermarsh on 2024-02-16 03:31_

Have you tried running the formatter (`ruff format`), or the isort rules (`ruff check --fix --select I`)? Both of these should take care of reformatting the imports, but the `F401` rule is intentionally independent from that process.

---

_Label `question` added by @charliermarsh on 2024-02-16 03:31_

---

_Comment by @MichaReiser on 2024-02-23 21:18_

Hy @Beliavsky. I'll close this as resolved. Feel free to reopen or comment if you're still facing the issue.

---

_Closed by @MichaReiser on 2024-02-23 21:18_

---

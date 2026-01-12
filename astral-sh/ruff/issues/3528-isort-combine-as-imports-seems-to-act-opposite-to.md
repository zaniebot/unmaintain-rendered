```yaml
number: 3528
title: isort.combine-as-imports seems to act opposite to its definition
type: issue
state: closed
author: davfsa
labels:
  - question
  - isort
assignees: []
created_at: 2023-03-14T22:52:41Z
updated_at: 2023-03-15T07:03:30Z
url: https://github.com/astral-sh/ruff/issues/3528
synced_at: 2026-01-12T15:54:43Z
```

# isort.combine-as-imports seems to act opposite to its definition

---

_@davfsa_

Hey!

I found what I believe is a little bug with regards to isorts `combine-as-imports`, as it seems to act contrary to its definition in some cases.

#### Sample reproduction code:
```py
from json import detect_encoding
from json import dump
from json import dumps as json_dumps
from json import load
from json import loads as json_loads

```
*Quite a bad example of "real world code", but there is a genuine use case behind this. The random imports are for the sake of showing the problem. Note that running isort with `profile = "black"` and `force-single-line = true` doesnt change this file*

#### `--fix` while using `combine-as-imports = true`
```
$ ruff ruff_test.py --select I001 --fix
<no output>

$ cat ruff_test.py
from json import detect_encoding
from json import dump
from json import dumps as json_dumps
from json import load
from json import loads as json_loads
```

#### `--fix` while using `combine-as-imports = false`
```
$ ruff ruff_test.py --select I001 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruff_test.py
from json import detect_encoding
from json import dump
from json import load
from json import dumps as json_dumps
from json import loads as json_loads
```



#### Ruff settings:
```toml
[tool.ruff]
line-length = 125
target-version = "py38"
select = ["E", "I", "F"]

[tool.ruff.isort]
force-single-line = true
combine-as-imports = true  # This line config will be added/removed according to the steps above
```
#### Ruff version:
ruff 0.0.255


---

_Comment by @charliermarsh on 2023-03-14 23:58_

I think the issue is that you have `force-single-line = true`. `combine-as-imports` refers to grouping the imports, like:

```py
from json import (
    detect_encoding,
    dump,
    dumps as json_dumps,
    load,
    loads as json_loads,
)
```

But if you have `force-single-line = true`, it's _always_ going to split into one import per line, so `combine-as-imports` has no effect.

---

_Closed by @charliermarsh on 2023-03-14 23:58_

---

_Comment by @charliermarsh on 2023-03-14 23:59_

(Closing because I tested the above locally, but just LMK if that's unclear at all, or if there's still an issue!)

---

_Label `question` added by @charliermarsh on 2023-03-14 23:59_

---

_Label `isort` added by @charliermarsh on 2023-03-14 23:59_

---

_Comment by @davfsa on 2023-03-15 00:19_

Thanks for the quick reply!

As you pointed out, the behaviour of the configurations I expected is exactly that, which is why it was weird to me when they differed from isort to ruff.

The code sample I provided with the profile set to black and `force_single_line=true` in isort, it doesn't change the file, but ruff reports that it should.

This was confusing to me and I looked through the documentation and tried out several different configurations and ended up finding that setting `combine-as-imports` seemed to be actig opposite to what is documented, as setting it to true keeps the `as` imports deperated, but setting it to false (the default) reports that they should be joint.

Sorry if my initial bug report wasn't clear enough, hope this comment improves it!

---

_Comment by @charliermarsh on 2023-03-15 02:11_

No worries! I think I understand what you're saying now. For simplicity, lets _ignore_ `combine-as-imports` entirely. The issue is that with `force-single-line = true`, you're seeing a different ordering between Ruff and isort.

---

_Reopened by @charliermarsh on 2023-03-15 02:11_

---

_Closed by @charliermarsh on 2023-03-15 02:26_

---

_Comment by @davfsa on 2023-03-15 07:03_

Thanks for the quick fix!

---

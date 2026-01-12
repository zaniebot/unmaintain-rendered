```yaml
number: 1513
title: Use more precise error ranges for names
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: identifier-error-ranges
created_at: 2022-12-31T15:21:57Z
updated_at: 2023-01-01T07:56:56Z
url: https://github.com/astral-sh/ruff/pull/1513
synced_at: 2026-01-12T05:36:31Z
```

# Use more precise error ranges for names

---

_Pull request opened by @harupy on 2022-12-31 15:21_

Improves error ranges for the following rules:

- E741
- PLW0602
- PLE0117


### Current

```
> cargo run -- --select PLW --no-cache --show-source resources/test/fixtures/pylint/global_variable_not_assigned.py --show-source
resources/test/fixtures/pylint/global_variable_not_assigned.py:5:5: PLW0602 Using global for `x` but no assignment is done
  |
5 |     global x
  |     ^^^^^^^^ PLW0602
  |

resources/test/fixtures/pylint/global_variable_not_assigned.py:9:5: PLW0602 Using global for `x` but no assignment is done
  |
9 |     global x
  |     ^^^^^^^^ PLW0602
  |

Found 2 error(s).
```

### Fixed

```
> cargo run -- --select PLW --no-cache resources/test/fixtures/pylint/global_variable_not_assigned.py --show-source
resources/test/fixtures/pylint/global_variable_not_assigned.py:5:12: PLW0602 Using global for `x` but no assignment is done
  |
5 |     global x
  |            ^ PLW0602
  |

resources/test/fixtures/pylint/global_variable_not_assigned.py:9:12: PLW0602 Using global for `x` but no assignment is done
  |
9 |     global x
  |            ^ PLW0602
  |

Found 2 error(s).
```


---

_Merged by @charliermarsh on 2022-12-31 18:42_

---

_Closed by @charliermarsh on 2022-12-31 18:42_

---

_Comment by @not-my-profile on 2023-01-01 04:28_

What's the reasoning behind this range narrowing? The error is about using global, so I think including the `global` keyword in the range actually makes sense ... so I think I prefer the previous ranges.

---

_Comment by @harupy on 2023-01-01 04:58_

@not-my-profile That makes sense. I'm happy to file a PR to revert the changes for PLW0602 and PLE0117.

---

_Comment by @harupy on 2023-01-01 06:39_

The range narrowing might make sense in the following case:

```python
def f():
    global x, y, z
    ^^^^^^^^^^^^^^
    x = y = 1
```

vs.

```python
def f():
    global x, y, z
                 ^
    x = y = 1
```

---

_Comment by @not-my-profile on 2023-01-01 07:23_

Ah ok, yes that does make sense :) I assume ruff doesn't support multi-ranges?

```python
def f():
    global x, y, z
    ^^^^^^       ^
    x = y = 1
```

Apparently `src/printer.rs` uses the [annotate-snippets](https://crates.io/crates/annotate_snippets) library ... but I don't know if that library supports such annotations.

---

_Comment by @charliermarsh on 2023-01-01 07:56_

We don’t support multi-ranges right now. It could be useful but I’d need to think on it. It’s tricky because it doesn’t map well to the LSP schema.

---

```yaml
number: 2241
title: "feat: pylint `PLE0604` and `PLE0605`"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: pylint-ple0604-ple0605
created_at: 2023-01-26T23:54:30Z
updated_at: 2023-01-27T16:29:03Z
url: https://github.com/astral-sh/ruff/pull/2241
synced_at: 2026-01-12T15:55:07Z
```

# feat: pylint `PLE0604` and `PLE0605`

---

_@sbrugman_

Tracking issue: https://github.com/charliermarsh/ruff/issues/970

---

_@charliermarsh reviewed on 2023-01-27 00:24_

---

_Review comment by @charliermarsh on `src/ast/operations.rs`:17 on 2023-01-27 00:24_

My gut reaction is that this operation is fast (and rare) enough that I'd rather split this into three separate functions. What do you think?

I was going to suggest making this `-> Result<Vec<String>, CustomError>`, and having the error states captured by `CustomError`. But it looks like the errors aren't mutually exclusive (both can occur at once), and that even if it "errors", we still want to return the names.

---

_@sbrugman reviewed on 2023-01-27 08:40_

---

_Review comment by @sbrugman on `src/ast/operations.rs`:17 on 2023-01-27 08:40_

`extract_all_names` extracts the names where it can, and ignores other cases (such as when the type of `_all__` is not a list of tuple. Keeping track on if it has encountered any value to ignore is arguably nog out of its scope of complicating its logic (it should not  be tied to PLE directly, this responsibility is in `ast.rs`. 

`Result<Vec<str>, ..>` wouldn't work, as both the return value and this state are used. We could also group the bools into a state struct and return it in a tuple.

Splitting up the functions is also an option. Currently this is the only place where we go through the values in `__all__`. I do not think they would be distinct enough to have code repetiton. On the other hand, the logic for checking might evolve and that should not happen in `operarions.rs` (could be concern for later).

What do you think is the best way to proceed?

---

_Review comment by @charliermarsh on `src/ast/operations.rs`:17 on 2023-01-27 12:46_

Yeah, totally understand. I was suggesting that we split up the function. I hadn't gone through the exercise, so I wasn't sure how much code would be duplicated, but sometimes in those cases, I just prefer to write-it-twice.

Maybe instead of returning two booleans, we return a `bitflags` flag, so that the responsibilities are clearer?

---

_@charliermarsh reviewed on 2023-01-27 12:46_

---

_@sbrugman reviewed on 2023-01-27 14:47_

---

_Review comment by @sbrugman on `src/ast/operations.rs`:17 on 2023-01-27 14:47_

Thanks. Refactored using bitflags. The responsibilities are now much clearer, do you agree?


---

_@charliermarsh reviewed on 2023-01-27 15:46_

---

_Review comment by @charliermarsh on `src/ast/operations.rs`:17 on 2023-01-27 15:46_

Awesome, thank you :)

---

_Merged by @charliermarsh on 2023-01-27 16:26_

---

_Closed by @charliermarsh on 2023-01-27 16:26_

---

_Branch deleted on 2023-01-27 16:29_

---

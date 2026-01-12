```yaml
number: 1363
title: Magic Trailing Commas in isort
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: 1200
created_at: 2022-12-24T19:03:43Z
updated_at: 2022-12-28T02:56:18Z
url: https://github.com/astral-sh/ruff/pull/1363
synced_at: 2026-01-12T15:55:06Z
```

# Magic Trailing Commas in isort

---

_@colin99d_

This fixes #1200 by adding magic trailing comma support. Looking forward to your feedback!

---

_Comment by @charliermarsh on 2022-12-24 19:05_

Awesome, thank you for this! Will try to review today.

---

_@charliermarsh reviewed on 2022-12-24 19:06_

---

_Review comment by @charliermarsh on `resources/test/fixtures/isort/magic_trailing_comma.py`:15 on 2022-12-24 19:06_

Can we add a few more examples, like:

```py
from os import (
    path,
    environ,
    execl,
    execv,  # Ends with a comment, should still treat as magic trailing comma.
)
```

```py
# These will be combined, but without a trailing comma.
from foo import bar
from foo import baz
```

```py
# These will be combined, _with_ a trailing comma, I think? Follow `isort` here.
from foo import bar
from foo import (
  baz,
  bop,
)
```


---

_Comment by @andersk on 2022-12-24 19:49_

In isort this is configurable with the `split_on_trailing_comma` option. We should do the same.

---

_Comment by @charliermarsh on 2022-12-24 20:05_

Agreed.

---

_Review comment by @colin99d on `resources/test/fixtures/isort/magic_trailing_comma.py`:15 on 2022-12-24 20:23_

<img width="772" alt="Screenshot 2022-12-24 at 3 21 01 PM" src="https://user-images.githubusercontent.com/72827203/209450095-f4726f8e-be9e-48f4-aa44-360b09c65d81.png">
It looks like the way you suggested is how isort does it. This is the one test my PR is currently failing on, will work on fixing it.

---

_@colin99d reviewed on 2022-12-24 20:23_

---

_Comment by @Jackenmen on 2022-12-24 20:26_

I haven't tested but... Awesome!

---

_@charliermarsh reviewed on 2022-12-24 21:17_

---

_Review comment by @charliermarsh on `resources/test/fixtures/isort/magic_trailing_comma.py`:15 on 2022-12-24 21:17_

I read the isort PR at some point, and I think the logic is that if either import block has a trailing comma, they preserve that when merging the two blocks.

---

_Converted to draft by @colin99d on 2022-12-24 22:17_

---

_Review comment by @colin99d on `src/isort/mod.rs`:478 on 2022-12-25 02:18_

This will fail if there is a duplicate word before the desired word, I need to fix this before I am ready for a review.

---

_@colin99d reviewed on 2022-12-25 02:18_

---

_Review comment by @colin99d on `src/isort/mod.rs`:478 on 2022-12-26 01:40_

This is resolved now.

---

_@colin99d reviewed on 2022-12-26 01:40_

---

_Review comment by @charliermarsh on `src/isort/mod.rs`:478 on 2022-12-26 01:42_

Sweet

---

_@charliermarsh reviewed on 2022-12-26 01:42_

---

_Marked ready for review by @colin99d on 2022-12-26 03:08_

---

_@colin99d reviewed on 2022-12-26 03:08_

---

_Review comment by @colin99d on `resources/test/fixtures/isort/magic_trailing_comma.py`:15 on 2022-12-26 03:08_

Perfect! Got this implemented.

---

_@charliermarsh reviewed on 2022-12-26 14:38_

---

_Review comment by @charliermarsh on `src/isort/helpers.rs`:33 on 2022-12-26 14:38_

@colin99d - I went with a slightly different approach here. The previous version relied on passing around the entire source code and iterating over characters, which ran two risks: (1) it's kind of inefficient to grab the entire source code; and (2) the logic (while working!) required handling a variety of cases, since we were matching on raw strings.

This version tokenizes the code, so that we can just look for the `Tok::Comma` as the "last" token in the import (excluding some unimportant tokens, like newlines).

---

_Comment by @colin99d on 2022-12-26 14:38_

@charliermarsh it looks like this has some weird behavior when importing with as, even if the feature is disabled. I am not sure why. But I will dig in.

---

_@charliermarsh reviewed on 2022-12-26 14:39_

---

_Review comment by @charliermarsh on `README.md`:2540 on 2022-12-26 14:39_

I also made this the default, to match isort's `profile = "black"`.

---

_@charliermarsh reviewed on 2022-12-26 14:39_

---

_Review comment by @charliermarsh on `src/isort/mod.rs`:151 on 2022-12-26 14:39_

I also tweaked the order of operations: instead of collecting locations, and looking at the locations at the end, we detect a trailing comma for each block, and then propagate it through the code as we merge imports.

---

_@colin99d reviewed on 2022-12-26 14:39_

---

_Review comment by @colin99d on `src/isort/helpers.rs`:33 on 2022-12-26 14:39_

This is much better! I was assuming there were better ways to do things that would come out in the review.

---

_Merged by @charliermarsh on 2022-12-26 14:40_

---

_Closed by @charliermarsh on 2022-12-26 14:40_

---

_Comment by @charliermarsh on 2022-12-26 14:40_

Thank you! Appreciate all the effort and output here :)

I made a few changes (which I think fixed the `as` handling), but let me know if you have any questions or disagree with any of the choices I made.

---

_Comment by @colin99d on 2022-12-26 14:41_

I love all of them, thanks for the help!!

---

_Comment by @charliermarsh on 2022-12-26 14:42_

@colin99d - Thank _you_! Grateful for the contribution!

---

_Branch deleted on 2022-12-28 02:56_

---

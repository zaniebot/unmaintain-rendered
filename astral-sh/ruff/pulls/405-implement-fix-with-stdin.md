```yaml
number: 405
title: Implement --fix with stdin
type: pull_request
state: merged
author: fsouza
labels: []
assignees: []
merged: true
base: main
head: fix-stdin
created_at: 2022-10-12T04:18:47Z
updated_at: 2022-10-13T02:32:43Z
url: https://github.com/astral-sh/ruff/pull/405
synced_at: 2026-01-12T05:48:45Z
```

# Implement --fix with stdin

---

_Pull request opened by @fsouza on 2022-10-12 04:18_

This is a follow-up to #387 where we also support `--fix` with stdin.

---

_Comment by @fsouza on 2022-10-12 04:20_

@harupy @charliermarsh let me know what you think! :) 

Also, I know that `--autoformat` is a secret flag right now, but I imagine that when that is properly implemented we'll want to support stdin too?

---

_Review comment by @fsouza on `src/autofix/fixer.rs`:30 on 2022-10-12 06:52_

this is probably a bad idea 🤔 Like maybe we want to special case stdout here and only print if it's stdout, otherwise `--fix` will always rewrite all files. I'll revisit this tomorrow.

---

_@fsouza reviewed on 2022-10-12 06:52_

---

_@charliermarsh reviewed on 2022-10-12 12:56_

---

_Review comment by @charliermarsh on `src/autofix/fixer.rs`:30 on 2022-10-12 12:56_

Can we add an entirely separate `fix_contents` function that has the same logic and returns `Result<Option<String>>` with the fixed contents (if changed)?

---

_@charliermarsh reviewed on 2022-10-12 12:57_

---

_Review comment by @charliermarsh on `src/main.rs`:375 on 2022-10-12 12:57_

I think we only want to _not_ print here if it came from `stdin` (since in that case, the contents went to `stdout`). Should we move this check + block into the `if` and `else` cases above?

---

_Comment by @charliermarsh on 2022-10-12 12:58_

Thank you!

I think you're right regarding `--autoformat`, but it's safe to just ignore that flag for now :) When full autoformat support gets merged, it's very likely that we'll want to put that behind a separate subcommand anyway.


---

_@fsouza reviewed on 2022-10-12 18:27_

---

_Review comment by @fsouza on `src/main.rs`:375 on 2022-10-12 18:27_

Oh I don't even know how I missed this lol Will address this and the other feedback later today!

---

_@charliermarsh reviewed on 2022-10-12 18:36_

---

_Review comment by @charliermarsh on `src/main.rs`:375 on 2022-10-12 18:36_

Awesome, thanks! No rush of course :)

---

_@fsouza reviewed on 2022-10-12 22:47_

---

_Review comment by @fsouza on `src/autofix/fixer.rs`:30 on 2022-10-12 22:47_

Ended-up returning just `Option<string>` now that we're not writing the file, there is no error possible :D 

---

_Comment by @fsouza on 2022-10-12 22:47_

@charliermarsh thank you very much for the feedback! Please take another look.

---

_Comment by @charliermarsh on 2022-10-13 02:30_

This looks great!

---

_Comment by @charliermarsh on 2022-10-13 02:31_

Ran checks locally, merging :)

---

_Merged by @charliermarsh on 2022-10-13 02:31_

---

_Closed by @charliermarsh on 2022-10-13 02:31_

---

_Branch deleted on 2022-10-13 02:32_

---

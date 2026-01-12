```yaml
number: 8366
title: Update docs related to file level error suppression
type: pull_request
state: merged
author: doolio
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-file-level-noqa
created_at: 2023-10-30T20:43:20Z
updated_at: 2023-11-07T18:18:18Z
url: https://github.com/astral-sh/ruff/pull/8366
synced_at: 2026-01-12T15:55:26Z
```

# Update docs related to file level error suppression

---

_@doolio_

Fixes: #8364


---

_@zanieb reviewed on 2023-10-30 20:53_

---

_Review comment by @zanieb on `docs/linter.md`:230 on 2023-10-30 20:53_

Perhaps we could say "... add the line `# ruff: noqa` anywhere in the file..." which feels a bit clear than "add a comment on its own line"? Perhaps with another clarifying note at the end of the section saying "Global noqa comments must be on their own line to disambiguate from comments which ignore violations on a single line"?

---

_Comment by @zanieb on 2023-10-30 20:53_

Thank you so much for taking the time to contribute! I'm hoping we can make it a bit clearer, lmk what you think!

---

_Label `documentation` added by @zanieb on 2023-10-30 20:53_

---

_@doolio reviewed on 2023-10-30 21:01_

---

_Review comment by @doolio on `docs/linter.md`:230 on 2023-10-30 21:01_

From my admittedly simple tests (as I'm just learning to program) even the `# ruff: noqa` comment must be on its own line. If it is an inline comment then errors are not ignored. See

![image](https://github.com/astral-sh/ruff/assets/13859009/a4a48f61-73c8-4893-a502-65d4ed421917)

(where the error in lines 2 and 4 is `E741 Ambiguous variable name: 'O'`, although I don't understand why it is only highlighted on the 'O' and not the 'X' in line 1)

vs

![image](https://github.com/astral-sh/ruff/assets/13859009/6886cad7-bcdb-4e8a-8dcc-96477e675826)

---

_@zanieb reviewed on 2023-10-30 21:20_

---

_Review comment by @zanieb on `docs/linter.md`:230 on 2023-10-30 21:20_

Oh that shouldn't conflict with what I'm suggesting. Ignoring violations on a single line is "add `# noqa` or `# noqa: RULE` to the line" whereas a global violation ignore is adding a new line with `# ruff: noqa`

---

_@doolio reviewed on 2023-10-31 09:33_

---

_Review comment by @doolio on `docs/linter.md`:230 on 2023-10-31 09:33_

I see. I've pushed a new comment to reword the docs as you suggest. Please let me know if you I should make further changes. Should I squash these changes into one commit and rebase onto the latest commit on `main`?

Edit: Just for my own understanding do you know why no E741 error was not reported for the X variable in my simple example above?

---

_@MichaReiser approved on 2023-10-31 09:53_

---

_@charliermarsh approved on 2023-10-31 14:21_

Will defer to @zanieb to merge.

---

_@zanieb reviewed on 2023-10-31 15:56_

---

_Review comment by @zanieb on `docs/linter.md`:230 on 2023-10-31 15:56_

We squash merge so you don't need to do anything with your commits.

I can take a look at that error later today!

---

_Comment by @doolio on 2023-10-31 17:02_

> [Merge branch 'astral-sh:main' into docs-file-level-noqa](https://github.com/astral-sh/ruff/pull/8366/commits/1b869be7866a82accb95616ac160441b676630f5)

I did this in error.

---

_Comment by @zanieb on 2023-10-31 17:07_

You can undo the commit with `git reset --hard HEAD^` then push with `git push --force` — but there's also no need merging `main` has no detrimental effect.

---

_Comment by @doolio on 2023-10-31 17:30_

Thanks. I was concerned I would have screwed up the history for the merge into `main`. I've done as you suggested if for no reason other than it was a good learning exercise for me!

---

_@zanieb reviewed on 2023-10-31 17:34_

---

_Review comment by @zanieb on `docs/linter.md`:230 on 2023-10-31 17:34_

re the ambiguous name, "I" and "O" are the only names that are considered ambiguous https://docs.astral.sh/ruff/rules/ambiguous-variable-name/

---

_Review comment by @doolio on `docs/linter.md`:230 on 2023-10-31 18:04_

Ah, interesting. Thank you for taking the time to look into this.

---

_@doolio reviewed on 2023-10-31 18:04_

---

_Merged by @zanieb on 2023-10-31 18:55_

---

_Closed by @zanieb on 2023-10-31 18:55_

---

_Branch deleted on 2023-11-07 18:18_

---

```yaml
number: 8223
title: "Rewrite ecosystem checks and add `ruff format` reports"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ecosystem-format
created_at: 2023-10-25T17:47:16Z
updated_at: 2023-10-27T22:28:03Z
url: https://github.com/astral-sh/ruff/pull/8223
synced_at: 2026-01-12T15:55:25Z
```

# Rewrite ecosystem checks and add `ruff format` reports

---

_@zanieb_

Closes #7239 

- Refactors `scripts/check_ecosystem.py` into a new Python project at `python/ruff-ecosystem`
    - Includes [documentation](https://github.com/astral-sh/ruff/blob/zanie/ecosystem-format/python/ruff-ecosystem/README.md) now
    - Provides a `ruff-ecosystem` CLI
- Fixes bug where `ruff check` report included "fixable" summary line
- Adds truncation to `ruff check` reports
    - Otherwise we often won't see the `ruff format` reports
    - The truncation uses some very simple heuristics and could be improved in the future
- Identifies diagnostic changes that occur just because a violation's fix available changes
    - We still show the diff for the line because it's could matter _where_ this changes, but we could improve this
    - Similarly, we could improve detection of diagnostic changes where just the message changes
- Adds support for JSON ecosystem check output
    - I added this primarily for development purposes
- If there are no changes, only errors while processing projects, we display a different summary message
- When caching repositories, we now checkout the requested ref
- Adds `ruff format` reports, which format with the baseline then the use `format --diff` to generate a report
- Runs all CI jobs when the CI workflow is changed

## Known problems

- Since we must format the project to get a baseline, the permalink line numbers do not exactly correspond to the correct range
   - This looks... hard. I tried using `git diff` and some wonky hunk matching to recover the original line numbers but it doesn't seem worth it. I think we should probably commit the formatted changes to a fork or something if we want great results here. Consequently, I've just used the start line instead of a range for now.
- I don't love the comment structure — it'd be nice, perhaps, to have separate headings for the linter and formatter.   
    - However, the `pr-comment` workflow is an absolute pain to change because it runs _separately_ from this pull request so I if I want to make edits to it I can only test it via manual workflow dispatch.
- Lines are not printed "as we go" which means they're all held in memory, presumably this would be a problem for large-scale ecosystem checks
- We are encountering a hard limit with the maximum comment length supported by GitHub. We will need to move the bulk of the report elsewhere.

## Future work

- Update `ruff-ecosystem` to support non-default projects and `check_ecosystem_all.py` behavior
- Remove existing ecosystem check scripts
- Add preview mode toggle (#8076)
- Add a toggle for truncation
- Add hints for quick reproduction of runs locally
- Consider parsing JSON output of Ruff instead of using regex to parse the text output
- Links to project repositories should use the commit hash we checked against
- When caching repositories, we should pull the latest changes for the ref
- Sort check diffs by path and rule code only (changes in messages should not change order)
- Update check diffs to distinguish between new violations and changes in messages
- Add "fix" diffs
- Remove existing formatter similarity reports
- On release pull request, compare to the previous tag instead

---

_Comment by @github-actions[bot] on 2023-10-25 18:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Label `internal` added by @zanieb on 2023-10-25 19:17_

---

_@MichaReiser reviewed on 2023-10-26 23:49_

I heaven't read all the code but I find it much easier to quickly identify the relevant code parts that are responsible or X compared to before.

---

_Review comment by @konstin on `python/ruff-ecosystem/README.md`:3 on 2023-10-27 08:08_

```suggestion
Compare lint and format results for two different ruff versions (e.g. main and a PR) on real world projects.
```

---

_Review comment by @konstin on `python/ruff-ecosystem/ruff_ecosystem/check.py`:246 on 2023-10-27 08:11_

got confused that this is frozen when we later use its interior mutability a lot

---

_Review comment by @konstin on `python/ruff-ecosystem/ruff_ecosystem/check.py`:270 on 2023-10-27 08:13_

nit:

```suggestion
            return TypeError
```

That's TRY004 :P

---

_Review comment by @konstin on `python/ruff-ecosystem/ruff_ecosystem/format.py`:27 on 2023-10-27 08:35_

```suggestion
```

---

_@konstin approved on 2023-10-27 09:03_

very clean code!

---

_Comment by @MichaReiser on 2023-10-27 09:04_

> very clean code!

That means a lot if comes from @konstin !

---

_@zanieb reviewed on 2023-10-27 16:53_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/check.py`:270 on 2023-10-27 16:53_

I think not here https://docs.python.org/3.11/library/constants.html#NotImplemented

Although `raise TypeError` would be fine, this is "more correct"

---

_Marked ready for review by @zanieb on 2023-10-27 20:33_

---

_Merged by @zanieb on 2023-10-27 22:28_

---

_Closed by @zanieb on 2023-10-27 22:28_

---

_Branch deleted on 2023-10-27 22:28_

---

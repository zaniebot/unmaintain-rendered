```yaml
number: 6948
title: Report number of changed files on the format CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-cli-ii
created_at: 2023-08-28T17:18:24Z
updated_at: 2023-08-29T16:29:51Z
url: https://github.com/astral-sh/ruff/pull/6948
synced_at: 2026-01-12T02:45:38Z
```

# Report number of changed files on the format CLI

---

_Pull request opened by @charliermarsh on 2023-08-28 17:18_

## Summary

Very basic summary:

<img width="962" alt="Screen Shot 2023-08-28 at 1 17 37 PM" src="https://github.com/astral-sh/ruff/assets/1309177/53537aca-7579-44d8-855b-f4553affae50">

If you run with `--verbose`, we'll also show you the timing:

<img width="962" alt="Screen Shot 2023-08-28 at 1 17 58 PM" src="https://github.com/astral-sh/ruff/assets/1309177/63cbd13e-9462-4e49-b3a3-c6663a7ad41c">


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-28 17:18_

---

_Label `formatter` added by @charliermarsh on 2023-08-28 17:18_

---

_Comment by @github-actions[bot] on 2023-08-28 17:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-08-28 18:06_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/format.rs`:163 on 2023-08-28 18:06_

Is it clear that these files are not ones that were excluded for some reason? Should we say "already formatted"?

---

_@charliermarsh reviewed on 2023-08-28 21:48_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:163 on 2023-08-28 21:48_

I erred on the side of matching Black's terminology here.

---

_@zanieb approved on 2023-08-28 22:24_

---

_@zanieb reviewed on 2023-08-28 22:24_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/format.rs`:163 on 2023-08-28 22:24_

Alright. To be considered another day :)

---

_Merged by @charliermarsh on 2023-08-28 22:42_

---

_Closed by @charliermarsh on 2023-08-28 22:42_

---

_Branch deleted on 2023-08-28 22:42_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/args.rs`:332 on 2023-08-29 06:11_

The formatter, not the linter?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:90 on 2023-08-29 06:13_

Nit: Implement `Display` for summary instead so that you can do `write!(writer, "{summary}")`?

---

_@MichaReiser reviewed on 2023-08-29 06:16_

---

_@MichaReiser reviewed on 2023-08-29 11:55_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:48 on 2023-08-29 11:55_

Nit: we could use a `receive` and `sender` channel instead:
* One thread receives the `FormatResult` and builds the `FormatSummary`
* The other threads process the items. 

This would remove the need to write all results to two vectors first. 

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:48 on 2023-08-29 16:29_

Nice idea, I will try this.

---

_@charliermarsh reviewed on 2023-08-29 16:29_

---

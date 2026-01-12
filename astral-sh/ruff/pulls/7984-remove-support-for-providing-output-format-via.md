```yaml
number: 7984
title: "Remove support for providing output format via `format` option"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: main
head: zanie/remove-format
created_at: 2023-10-16T15:56:04Z
updated_at: 2023-10-17T01:59:33Z
url: https://github.com/astral-sh/ruff/pull/7984
synced_at: 2026-01-12T02:32:41Z
```

# Remove support for providing output format via `format` option

---

_Pull request opened by @zanieb on 2023-10-16 15:56_

See the provided breaking changes note for details.

Removes support for the deprecated `--format`option in the `ruff check` CLI, `format` inference as `output-format` in the configuration file, and the `RUFF_FORMAT` environment variable.

The error message for use of `format` in the configuration file could be better, but would require some awkward serde wrappers and it seems hard to present the correct schema to the user still.



---

_Label `breaking` added by @zanieb on 2023-10-16 15:56_

---

_Label `configuration` added by @zanieb on 2023-10-16 15:56_

---

_@zanieb reviewed on 2023-10-16 15:56_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/format.rs`:172 on 2023-10-16 15:56_

This temporary directory name needs to be filtered from the assertion

---

_@zanieb reviewed on 2023-10-16 16:57_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/format.rs`:172 on 2023-10-16 16:57_

Victory!

---

_Marked ready for review by @zanieb on 2023-10-16 17:41_

---

_@charliermarsh approved on 2023-10-16 17:45_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/format.rs`:160 on 2023-10-16 17:45_

Huzzah

---

_@charliermarsh reviewed on 2023-10-16 17:45_

---

_Comment by @github-actions[bot] on 2023-10-16 17:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @zanieb on 2023-10-16 18:06_

---

_Closed by @zanieb on 2023-10-16 18:06_

---

_Branch deleted on 2023-10-16 18:06_

---

_Comment by @acdha on 2023-10-16 20:00_

This broke everyone's CI without warning. It's not that hard to fix but at the very least it would have been nice to have advanced notice and perhaps a more meaningful error message.

---

_Comment by @zanieb on 2023-10-16 20:06_

Hey @acdha I'm sorry to hear this broke your CI.

Use of this displayed warnings since v0.0.291 and the [changelog for that release](https://github.com/astral-sh/ruff/releases/tag/v0.0.291) had a bolded notice. Additionally, we grouped this breaking change with an increase in a different version number per our [versioning policy](https://docs.astral.sh/ruff/versioning/).

I'm not sure what else we could have done here, except to have a longer deprecation period — which we will generally strive for, but in this case the error messages when the `format` configuration option was used incorrectly were confusing many of our users and challenging to improve without dropping support as we did here.



---

_Comment by @acdha on 2023-10-16 21:03_

> Hey @acdha I'm sorry to hear this broke your CI.
> 
> Use of this displayed warnings since v0.0.291 and the [changelog for that release](https://github.com/astral-sh/ruff/releases/tag/v0.0.291) had a bolded notice. Additionally, we grouped this breaking change with an increase in a different version number per our [versioning policy](https://docs.astral.sh/ruff/versioning/).
> 
> I'm not sure what else we could have done here, except to have a longer deprecation period — which we will generally strive for, but in this case the error messages when the `format` configuration option was used incorrectly were confusing many of our users and challenging to improve without dropping support as we did here.

Yeah, I appreciate that there isn't a great answer here - a longer period than a couple of weeks might have helped but given that most of the time it's running in CI where people rarely look at passing builds I think the part which would have helped more would have been having an error message specifically noting that this option was renamed. Long-term, I'm sure the best solution would be an official GitHub Action which you could update at the same time as any future API changes.

---

_Comment by @MichaReiser on 2023-10-17 01:59_

> This broke everyone's CI without warning. It's not that hard to fix but at the very least it would have been nice to have advanced notice and perhaps a more meaningful error message.

I'm sorry about that. What's your setup? Do you always install the latest Ruff version on CI? If so, I would recommend you to pin your ruff version and bump it periodically. 

---

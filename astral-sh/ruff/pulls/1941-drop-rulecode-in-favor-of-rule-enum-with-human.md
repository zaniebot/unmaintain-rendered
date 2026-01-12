```yaml
number: 1941
title: "Drop `RuleCode` in favor of `Rule` enum with human-friendly names"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: rulecode-to-rule
created_at: 2023-01-18T01:45:29Z
updated_at: 2023-01-19T05:06:25Z
url: https://github.com/astral-sh/ruff/pull/1941
synced_at: 2026-01-12T15:55:07Z
```

# Drop `RuleCode` in favor of `Rule` enum with human-friendly names

---

_@not-my-profile_

Rules often exist in several linters for example UselessObjectInheritance is known as:

* UP004 in pyupgrade
* R0205 in pylint
* PIE792 in flake8-pie

Ruff currently only supports one code per rule (aside from "prefix redirects" which aren't documented at all), so basically the first code used ends up being the code promoted by ruff which is very arbitrary.

I think we should rather have a proper many to many mapping, allowing many codes to be mapped to one ruff rule or one code to several ruff rules (e.g. C0103 from pylint encompasses N801, N802 and N803 from pep8-naming). This would also allow flake8_to_ruff to be expanded to support e.g. pylint.

To cleanly implement this we have to decouple "rules" from "rule codes". This is the first PR for that epic task ... it doesn't yet change the mapping, only which variant names we use in our codebase.

Added benefits: The code is no longer littered with cryptic numeric codes and thus more readable and we get a big step closer to #1773.

---

_Comment by @charliermarsh on 2023-01-18 04:47_

I haven't looked at the code yet, but I definitely agree with the argument put forward in the summary -- I've wanted this for a long time!

---

_Comment by @not-my-profile on 2023-01-18 08:46_

Alright let me give you some more context: The PR changes nothing about the UI. The first two commits are solely preparatory and could be merged independently from the rest. I have relabeled the remaining 7 commits as {n}/7 to make it clear that they belong together and have amended the 1st of those commits to explain the commit set in the commit message.

The derive implementations that depend on the variant names are removed to guarantee that they aren't used by anything anymore so that the last commit doesn't break anything ... these derive implementations may very well be added back in the future.

As always I strongly advise you to review this commit by commit to make sense of the changes :)

---

_Comment by @charliermarsh on 2023-01-18 16:09_

Thank you. I know it's likely tedious to have to keep it up-to-date so feel free to rebase at the end or as you like. I'll try to review this today -- bear with me as it's a large PR :)

---

_Comment by @charliermarsh on 2023-01-19 00:56_

(Will review this tonight.)

---

_Review comment by @charliermarsh on `Cargo.toml`:63 on 2023-01-19 02:10_

Do you mind just wrapping this in `{ version ... }` for consistency?

---

_Review comment by @charliermarsh on `ruff_cli/src/commands.rs`:117 on 2023-01-19 02:11_

Did you change these manually, or via some automation?

---

_Review comment by @charliermarsh on `src/checkers/imports.rs`:39 on 2023-01-19 02:12_

This is, of course, greatly, greatly superior.

---

_Review comment by @charliermarsh on `ruff_cli/src/cli.rs`:183 on 2023-01-19 02:16_

Maybe the one "loss" in this series of commits. I wish we could catch these statically as part of the CLI.

Separately, while we're here (and now that the type is string), can we set `value_name = "RULE_CODE"` for this?

---

_Review comment by @charliermarsh on `ruff_cli/src/main.rs`:170 on 2023-01-19 02:17_

Is this intended? It should all be handled by the outer function via `commands::explain(&code, format)?;`.

---

_@charliermarsh reviewed on 2023-01-19 02:19_

---

_@not-my-profile reviewed on 2023-01-19 03:42_

---

_Review comment by @not-my-profile on `ruff_cli/src/commands.rs`:117 on 2023-01-19 03:42_

I did the initial conversion automatically via a small Python script however that script isn't idempotent and the last commit also contained some manual changes so I have been manually rebasing since.

---

_Review comment by @charliermarsh on `ruff_cli/src/commands.rs`:117 on 2023-01-19 03:45_

(I can try to make this the next thing we merge.)

---

_@charliermarsh reviewed on 2023-01-19 03:45_

---

_Review comment by @not-my-profile on `ruff_cli/src/commands.rs`:117 on 2023-01-19 04:02_

Ok great let's do that ... I already addressed your two other comments locally ... then I'll start the final rebase ... doing it automatically this time because there have been too much changes to do this manually ... I'll have to split the last commit into two one for the manual changes and one for the automatic ones and update my python script^^.

---

_@not-my-profile reviewed on 2023-01-19 04:02_

---

_@charliermarsh reviewed on 2023-01-19 04:22_

---

_Review comment by @charliermarsh on `ruff_cli/src/commands.rs`:117 on 2023-01-19 04:22_

Ok, all good. I'll hold off on merging anything until this is updated and in.

---

_Review comment by @not-my-profile on `ruff_cli/src/cli.rs`:183 on 2023-01-19 04:47_

I have changed this to `Option<Rule>` by setting the clap option `value_parser=Rule::from_code`.

---

_@not-my-profile reviewed on 2023-01-19 04:47_

---

_Review comment by @not-my-profile on `ruff_cli/src/main.rs`:170 on 2023-01-19 04:48_

With the previously described change to use `value_parser` this diff has been removed.

---

_@not-my-profile reviewed on 2023-01-19 04:48_

---

_Review comment by @not-my-profile on `ruff_cli/src/commands.rs`:117 on 2023-01-19 04:48_

See the last commit message for the script I used to generate it (the last commit is now 100% autogenerated).

---

_@not-my-profile reviewed on 2023-01-19 04:48_

---

_Review comment by @not-my-profile on `ruff_cli/src/cli.rs`:183 on 2023-01-19 04:51_

>can we set value_name = "RULE_CODE" for this?

I agree that this would be a good addition however this PR accomplishes one thing and I'd rather keep unrelated changes for other PRs.

---

_@not-my-profile reviewed on 2023-01-19 04:51_

---

_Merged by @charliermarsh on 2023-01-19 04:51_

---

_Closed by @charliermarsh on 2023-01-19 04:51_

---

_@charliermarsh reviewed on 2023-01-19 04:53_

---

_Review comment by @charliermarsh on `ruff_cli/src/cli.rs`:183 on 2023-01-19 04:53_

All good, preserving the type is great.

---

_Comment by @not-my-profile on 2023-01-19 05:06_

Thanks for reviewing and merging, I think this greatly improves the overall code readability. :tada: 

---

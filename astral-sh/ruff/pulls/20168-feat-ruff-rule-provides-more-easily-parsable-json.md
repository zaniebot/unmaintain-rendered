```yaml
number: 20168
title: "feat: 'ruff rule' provides more easily parsable JSON ouput"
type: pull_request
state: merged
author: LoicRiegel
labels:
  - cli
assignees: []
merged: true
base: main
head: feat/better-rule-as-json
created_at: 2025-08-30T16:01:24Z
updated_at: 2025-10-20T07:33:13Z
url: https://github.com/astral-sh/ruff/pull/20168
synced_at: 2026-01-12T15:56:56Z
```

# feat: 'ruff rule' provides more easily parsable JSON ouput

---

_@LoicRiegel_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Not sure if it entirely closes #9891, but it's at least a step in the right direction

This change improves the JSON output of the "ruff rule" command.
- the "fix" field is more easily parsable: possible values are now the values from the `FixAvailability` enum
- replaced the "preview" field with a "group" field with the possible values of the `RuleGroup` enum (stable, preview, deprecated, removed). It's more accurate and parsable that way
- everything else is the same

## Test Plan

I saw no tests for the "ruff rule" command (maybe I missed them?)

So I tested manually:
```
# BEFORE
~> uvx ruff rule I001 --output-format json
{
  "name": "unsorted-imports",
  "code": "I001",
  "linter": "isort",
  "summary": "Import block is un-sorted or un-formatted",
  "message_formats": [
    "Import block is un-sorted or un-formatted"
  ],
  "fix": "Fix is sometimes available.",
  "explanation": "## What it does\nDe-duplicates, groups, and sorts imports based on the provided `isort` settings.\n\n## Why is this bad?\nConsistency is good. Use a common convention for imports to make your code\nmore readable and idiomatic.\n\n## Example\n```python\nimport pandas\nimport numpy as np\n```\n\nUse instead:\n```python\nimport numpy as np\nimport pandas\n```\n\n## Preview\nWhen [`preview`](https://docs.astral.sh/ruff/preview/) mode is enabled, Ruff applies a stricter criterion\nfor determining whether an import should be classified as first-party.\nSpecifically, for an import of the form `import foo.bar.baz`, Ruff will\ncheck that `foo/bar`, relative to a [user-specified `src`](https://docs.astral.sh/ruff/settings/#src) directory, contains either\nthe directory `baz` or else a file with the name `baz.py` or `baz.pyi`.\n",
  "preview": false
}

# AFTER
~> target\debug\ruff.exe rule I001 --output-format json
{
  "name": "unsorted-imports",
  "code": "I001",
  "linter": "isort",
  "summary": "Import block is un-sorted or un-formatted",
  "message_formats": [
    "Import block is un-sorted or un-formatted"
  ],
  "explanation": "## What it does\nDe-duplicates, groups, and sorts imports based on the provided `isort` settings.\n\n## Why is this bad?\nConsistency is good. Use a common convention for imports to make your code\nmore readable and idiomatic.\n\n## Example\n```python\nimport pandas\nimport numpy as np\n```\n\nUse instead:\n```python\nimport numpy as np\nimport pandas\n```\n\n## Preview\nWhen [`preview`](https://docs.astral.sh/ruff/preview/) mode is enabled, Ruff applies a stricter criterion\nfor determining whether an import should be classified as first-party.\nSpecifically, for an import of the form `import foo.bar.baz`, Ruff will\ncheck that `foo/bar`, relative to a [user-specified `src`](https://docs.astral.sh/ruff/settings/#src) directory, contains either\nthe directory `baz` or else a file with the name `baz.py` or `baz.pyi`.\n",
  "fix": "Sometimes",
  "group": "Stable"
}
```


---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:13 on 2025-09-09 18:00_

tiny nit: I'd group these up above with the `serde` import. Unfortunately stable `rustfmt` still doesn't handle these groups automatically.

---

_@ntBre reviewed on 2025-09-09 18:08_

Thanks! This looks good to me. I'm surprised we don't have any tests for this either, but we should add some. I found a couple of `ruff rule` tests here:

https://github.com/astral-sh/ruff/blob/c3f1f0b9f1ed8e522a6d22c7d1670997c19183de/crates/ruff/tests/integration_test.rs#L949-L952

but none for the JSON format. This could be a good place to add one.

I also think this is a breaking change that we might need to gate behind `preview`. We could theoretically try to squeeze it into the minor release tomorrow, but I'd be interested in getting @MichaReiser's thoughts next week when he's back from PTO anyway, instead of trying to rush it.

---

_Comment by @github-actions[bot] on 2025-09-09 18:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `cli` added by @ntBre on 2025-09-09 18:19_

---

_Comment by @LoicRiegel on 2025-09-11 07:14_

You're welcome. I'll add the tests, but I have no time until next week.
What should I do to put this behind the --preview flag, as you suggested? That means that my changes should only apply when the --preview flag is provided, correct?
How do you then track what features should be made stable when you do the next minor release, and who takes should take care of it?

---

_Comment by @ntBre on 2025-09-11 13:16_

Since `ruff rule` is a separate subcommand, I think we'd actually have to add a separate `--preview` flag to it. That might be a good thing to double-check with Micha too, so no worries at all about not working on it until next week :) But yes, my idea was to keep the old behavior on stable and enable the new behavior only when `--preview` is used.

For stabilizing features, we can just open an issue and add it to the next Milestone on GitHub. We go through all of those before every minor release and handle the stabilizations. In the linter we collect helper functions for preview behaviors in [`preview.rs`](https://github.com/astral-sh/ruff/blob/34ba738c19c557653567defc5cdaaa75445ad0d4/crates/ruff_linter/src/preview.rs#L10-L12) in lieu of opening a stabilization issue immediately, but again we can't really reuse that here since `rule` is a separate subcommand.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:25 on 2025-09-17 07:48_

I suggest renaming this to `status`. It might otherwise be mistaken for a rule category (something we want to introduce in the future). 

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 07:50_

I agree that this is an improvement, but I'm not sure if it's worth going through the complications of adding a preview flag (which might even be more annoying for upstream tools than an outright breaking change because they now need to handle both formats if the user set `preview = true` in their configuration). 

I'm inclined to just leave it as is OR introduce a new field `fix_availability` and deprecate the old field. 

---

_@MichaReiser reviewed on 2025-09-17 07:51_

---

_@LoicRiegel reviewed on 2025-09-17 08:31_

---

_Review comment by @LoicRiegel on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 08:31_

I'd prefer to go with the second option. So the old field should be deprecated in the next minor release, right?

---

_@MichaReiser reviewed on 2025-09-17 09:20_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 09:20_

Yes, we would deprecate or outright remove it in the next minor. But again, I'm not sure what the value of doing so is. It only breaks downstream users. So I expect the field to stick around for a long time :)

---

_@LoicRiegel reviewed on 2025-09-17 11:47_

---

_Review comment by @LoicRiegel on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 11:47_

I thought having the enum values instead of entire sentences like `"Fix is sometimes available."` would allow users to parse the json more easily, which was the point raised by the issue
We could also leave both fields, even if this would mean some duplication...

Not sure what's best... Let me know what you prefer :)

---

_@MichaReiser reviewed on 2025-09-17 15:13_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 15:13_

I'm fine with reverting the change or adding a second field

---

_@ntBre reviewed on 2025-09-17 15:21_

---

_Review comment by @ntBre on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 15:21_

We probably need to restore the `preview` field in that case too right? I like the idea of leaving the existing fields untouched (restoring `preview` and the string `fix` field) and adding the `status` and `fix_availability` fields, which should be easier to match exhaustively.

---

_@MichaReiser reviewed on 2025-09-17 15:23_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/rule.rs`:39 on 2025-09-17 15:23_

Yes, we need to restore any breaking change.

---

_@LoicRiegel reviewed on 2025-10-19 16:52_

---

_Review comment by @LoicRiegel on `crates/ruff/src/commands/rule.rs`:25 on 2025-10-19 16:52_

done

---

_@LoicRiegel reviewed on 2025-10-19 16:52_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/src/codes.rs`:13 on 2025-10-19 16:52_

done

---

_Comment by @LoicRiegel on 2025-10-19 16:52_

Hello everyone,
First, sorry for leaving this PR open for so long. I had some days off, and since then I've only had access to pretty slow computers...

But anyway, I've just updated the branch. So I left all the old fields untouched, I just added new ones.

I added basic integration tests for the "ruff rule" with "--output-json" command, very simillar to the existing ones testing "ruff rule".

Since there's no breaking change anymore, it's fine to not put the new fields behind a "--preview" CLI flag, right?


---

_Comment by @MichaReiser on 2025-10-20 07:08_

No worries. I sort of forgot about it too. I'll take a look now

---

_@MichaReiser approved on 2025-10-20 07:09_

Thank you

---

_Merged by @MichaReiser on 2025-10-20 07:09_

---

_Closed by @MichaReiser on 2025-10-20 07:09_

---

_Branch deleted on 2025-10-20 07:33_

---

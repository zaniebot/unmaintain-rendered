```yaml
number: 9713
title: "new rule: enforce consistent capitalisation of noqa statements"
type: pull_request
state: closed
author: danieleades
labels: []
assignees: []
draft: true
base: main
head: noqa
created_at: 2024-01-30T19:00:39Z
updated_at: 2024-03-28T17:44:33Z
url: https://github.com/astral-sh/ruff/pull/9713
synced_at: 2026-01-12T15:55:30Z
```

# new rule: enforce consistent capitalisation of noqa statements

---

_@danieleades_

closes #9596

---

_Renamed from "sketch out new rule" to "new rule: enforce consistent capitalisation of noqa statements" by @danieleades on 2024-01-30 19:01_

---

_Comment by @charliermarsh on 2024-02-25 22:01_

@danieleades -- Sorry I missed this! Are you interested in working on it? If so, I can write up some notes on how to implement it.

---

_Comment by @danieleades on 2024-02-25 22:12_

> @danieleades -- Sorry I missed this! Are you interested in working on it? If so, I can write up some notes on how to implement it.

If you've got the time, that'd be great. I had a half-hearted attempt but found it a little impenetrable. Seems you need to make a lot of non-local changes to add a new rule

---

_Comment by @charliermarsh on 2024-02-25 22:33_

@danieleades - So first, here's a PR that you can use as a reference for all the stuff outside of the core rule logic: https://github.com/astral-sh/ruff/pull/9728/files (e.g., the addition of the rule in `codes.rs`, the addition of a fixture, and so on).

For the rule itself... I'd probably do the following:

- Set the rule source to `noqa`: https://github.com/astral-sh/ruff/blob/1711bca4a0c97b809bc4c5b2204bc0da564feb99/crates/ruff_linter/src/registry.rs#L249
- In `checkers/noqa.rs`, there's a `noqa_directives` struct that contains all the parsed `# noqa` comments. I believe the `range` on the `Directive` field of `NoqaDirectiveLine` starts at the start of the comment. So you could iterate over the entries, pass each one to your rule function, and return a diagnostic if they're not "properly" capitalized: https://github.com/astral-sh/ruff/blob/1711bca4a0c97b809bc4c5b2204bc0da564feb99/crates/ruff_linter/src/checkers/noqa.rs#L34

---

_Comment by @MichaReiser on 2024-03-28 17:44_

I close this PR considering that there has been no work in a while. @danieleades, feel free to reopen if you're still planning to work on this.

---

_Closed by @MichaReiser on 2024-03-28 17:44_

---

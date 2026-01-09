---
number: 12481
title: Flag potentially duplicate rules in CI
type: issue
state: open
author: dylwil3
labels:
  - ci
assignees: []
created_at: 2024-07-23T21:41:01Z
updated_at: 2026-01-01T15:23:46Z
url: https://github.com/astral-sh/ruff/issues/12481
synced_at: 2026-01-07T13:12:15-06:00
---

# Flag potentially duplicate rules in CI

---

_Issue opened by @dylwil3 on 2024-07-23 21:41_

I wonder if it would be helpful to add a test or CI step to check for potentially duplicate rules.

The most naive version of this I can think of is as follows:

> CI fails if there is a pair of rules, Rule A and Rule B, which identify identical spans in `crates/ruff_linter/resources/test/.../A.py` as violations.

In this case I would guess that either Rule A and Rule B are identical, or else it should be possible to disambiguate them by adding more test cases. (But maybe there is an error in that logic). If this isn't true, perhaps a list of exceptional pairs can be maintained.

Presumably there is also a clever way to implement this such that the first time the check is run it might be slow but is fast on subsequent runs.

(Bonus points: Apply the same principle to speed up/automate the detection of rules from other packages which have already been implemented in Ruff.)

This is sort of tangentially related to the rule re-categorization effort #1774 and of course to the question of aliasing #2186.

Would love to know if there's interest or suggestions. If so, I can try to implement this.

---

_Comment by @MichaReiser on 2024-07-24 07:18_

An automated way of detecting duplicated rules would be very helpful, but I don't have a good feeling for how well the described approach would work regarding false positives and false negatives. I think a good first step would be to identify duplicate rules that were later merged/removed and play it through manually. 

---

_Label `ci` added by @MichaReiser on 2024-07-24 07:18_

---

_Comment by @dylwil3 on 2024-07-24 14:10_

Glad to hear there is some interest!

I did a quick experiment:

## Methodology

- Step 1: Run all Ruff rules on all test fixtures and export results to json. (Note: I did this just by selecting "ALL", which removes a few conflicting rules, but I'm ignoring that for now.)
- Step 2: For each rule, record all tuples of the form `(filename, start_line, end_line)` where a check occurs.
- Step 3: Compute the [intersection over the union](https://en.wikipedia.org/wiki/Jaccard_index) of these sets of "check locations" for each pair of rules.

## Results 

There appear to be no exact matches between existing rules, but we can see if there are any interesting "near matches". The top 10 most similar pairs (which happens to coincide with those pairs having similarity of over 50%) are as follows:

- D400 (`ends-in-period`) and D415 (`ends-in-punctuation`) (94.20%)
    - An obvious relationship: one set of violations contains the other.
- INP001 (`implicit-namespace-package`) and D100 (`undocumented-public-module`) (73.10%)
    - Probably an artifact of the file structure of `test/fixtures`.
- TD003 (`missing-todo-link`) and FIX002 (`line-contains-todo`) (71.80%)
    - One set of violations contains the other.
- F509 (`percent-format-unsupported-format-character`) and PLE1300 (`bad-string-format-character` (66.70%)
    - This reproduces the finding in #11403
- TD001 (`invalid-todo-tag`) and FIX001 (`line-contains-fixme`) (64.70%)
    - I believe the set of possible violations of the first contains the set of possible violations for the second
- ANN201 (`missing-return-type-undocumented-public-function`) and D103 (`undocumented-public-function`) (63.90%)
    - Artifact of the test cases 
- PTH101 (`os-chmod`) and S103 (`bad-file-permissions`) (63.60%)
    - Artifact of the test cases  
- TD003 (`missing-todo-link`) and TD002 (`missing-todo-author`) (60.30%)
    - Artifact of the test cases
- G004 (`logging-f-string`) and TRY401 (`verbose-log-message`) (53.30%)
    - Artifact of the test cases  
- FIX002 (`line-contains-todo`) and TD002 (`missing-todo-author`) (50.70%)
    - One set of violations contains the other. 

The number of false positives for this test is either a feature or a bug depending on whether or not it's helpful to spot places where additional test cases should be added to the test fixture.

One can play with the threshold a bit, depending on how many candidate pairs you want to consider. There are

- 10 pairs with >50% match
- 13 pairs with >40% match
- 30 pairs with >30% match
- 56 pairs with >20% match
- 135 pairs with >10% match
- 300 pairs with >5% match
- 2045 pairs with >0% match
- 217470 pairs total

## Next?

So perhaps having an automated check that checks a threshold of similarity would be helpful? If CI is too unwieldy for this, one could also manually run a script for this before releases/periodically.

The json and notebook used are [at this repo](https://github.com/dylwil3/duplicate-ruff-rules) if you'd like to play around yourself.

Let me know if you'd like me to go further with this (eg continue experimenting, add something to the `scripts` folder, etc.)

An example of an additional experiment might be to augment the set of files used to generate each rule's "profile" by including one or all of the codebases in the ecosystem checks. This has the advantage of comparing rule check behavior in a more natural setting, but the disadvantage that rarely tripped rules may get buried.

---

_Comment by @MichaReiser on 2024-08-19 13:45_

Sorry for the late reply. Overall, this does seem useful, but it seems too noisy for a CI job. It requires a manual review of the flagged rules. 

The time I would find such a check the most useful is when there's a PR for a new rule, similar to the ecosystem check. But I must admit it's unclear to me how to design the check so that it is approachable, understandable, and has a good false-positive/false-negative ratio.

---

_Referenced in [astral-sh/ruff#22328](../../astral-sh/ruff/issues/22328.md) on 2026-01-01 15:07_

---

_Comment by @ntBre on 2026-01-01 15:23_

This is very cool! (and keeps popping up in my default rules searches 😄)

I wonder if we could do something like the `ecosystem-analyzer` job for ty, where it only runs when we put a special label onto a PR? I agree with Micha that it would be most helpful to run this on new rule PRs.

I don't have any great suggestion for presenting the data, but reporting a short candidate list of overlapping rules for manual review would be a decent interface in my opinion.

The extent of the range overlap might be another signal that's slightly stronger than the starting and ending lines. The rules in #22328 have exactly overlapping ranges, for example. Maybe we could extract something from the docs too. Those two rules both link to the same page in the Python docs.

Just a few ideas!

---

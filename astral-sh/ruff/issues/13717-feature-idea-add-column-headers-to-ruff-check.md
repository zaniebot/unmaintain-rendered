```yaml
number: 13717
title: "[Feature Idea] Add column headers to `ruff check --statistics`, to avoid ambiguity"
type: issue
state: closed
author: NFeruchBCG
labels:
  - cli
  - help wanted
assignees: []
created_at: 2024-10-11T16:25:52Z
updated_at: 2025-01-02T12:42:09Z
url: https://github.com/astral-sh/ruff/issues/13717
synced_at: 2026-01-10T11:09:55Z
```

# [Feature Idea] Add column headers to `ruff check --statistics`, to avoid ambiguity

---

_Issue opened by @NFeruchBCG on 2024-10-11 16:25_

This is a question that can be easily answered, but ideally wouldn't have to be asked if there were column headers.

Here is an example output to running `ruff check --statistics`

```
$ ruff check --statistics
317     F541    [*] f-string-missing-placeholders
241     F401    [ ] unused-import
112     F841    [*] unused-variable
 72     F811    [ ] redefined-while-unused
 65     F821    [ ] undefined-name
 36     E401    [*] multiple-imports-on-one-line
 23     E712    [*] true-false-comparison
 19     E711    [*] none-comparison
 19     E722    [ ] bare-except
 18     E721    [ ] type-comparison
 10     F405    [ ] undefined-local-with-import-star-usage
  6     E701    [ ] multiple-statements-on-one-line-colon
  4     E402    [ ] module-import-not-at-top-of-file
  4     F403    [ ] undefined-local-with-import-star
  3     E713    [*] not-in-test
  3     F522    [*] string-dot-format-extra-named-arguments
  2     E731    [*] lambda-assignment
  2     F601    [ ] multi-value-repeated-key-literal
  2     F632    [*] is-literal
  2     F901    [*] raise-not-implemented
  1     E741    [ ] ambiguous-variable-name
```

For anyone new to using ruff, it's not immediately apparent what each column means.

Sure, it's reasonable to assume that the first column is the # of occurrences and that the second column is the rule, but what `[*]` means is unclear. I think having an explicit header row to denote what each column is would make for a better experience :D

---

_Label `cli` added by @MichaReiser on 2024-10-13 13:32_

---

_Label `help wanted` added by @MichaReiser on 2024-10-13 13:32_

---

_Comment by @dhruvmanila on 2024-10-14 10:34_

Just to be clear, `[*]` means that the violation is fixable.

---

_Comment by @sbrugman on 2024-10-16 10:15_

The issue here might be that when the meaning of `[*]` is unclear to the user, there is currently lacking documentation. The pattern “[\*]” in the docs will search for any bracketed (* as wildcard). The CLI help does not seem to mention fixable for —statistics.

A primary step to closing this issue would be to document the meaning of `[*]` better in the docs/CLI help.

Currently, the help message is:
```text
--statistics	Show counts for every rule with at least one violation
```

With the mention that violations that are fixable are indicated:
```text
--statistics	Show counts for every rule with at least one violation. `[*]` indicates that the violation is fixable with the `--fix` option.
```

This would also appear in the docs as such. Happy to add the PR.

A slight downside would be that this is only relevant for the text format, and not for JSON (but imo still the best way to go given the alternatives).

Note that currently we mimic Flake8's `--statistics` format and adding a header is a breaking change for users depending on that (although they probably should use the JSON format). Adding “Fixable” as header would make the format less concise, and choosing “Fix” would not be as clear. Adding a line similar to `ruff check` would still be an option: `[*] indicates the violation is fixable with the `--fix` option.`


![Header fixable](https://github.com/user-attachments/assets/dbc11170-271b-4b79-8a47-6bb07dd22036)
![Header fix](https://github.com/user-attachments/assets/8c0fbf1a-d6ea-445a-ab4a-1d92916c78ea)



---

_Comment by @MichaReiser on 2024-10-16 10:25_

I would prefer a message at the end of the output similar to the `ruff check` output over extending the help message (it makes it very long)

---

_Comment by @InSyncWithFoo on 2025-01-02 12:29_

This seems to have been resolved by #13774.

---

_Closed by @dhruvmanila on 2025-01-02 12:42_

---

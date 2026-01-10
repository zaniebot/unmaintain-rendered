```yaml
number: 8175
title: Provide a grouping rule for ruff formatter conflicting rules
type: issue
state: open
author: gaborbernat
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-10-24T17:17:30Z
updated_at: 2025-06-22T08:17:30Z
url: https://github.com/astral-sh/ruff/issues/8175
synced_at: 2026-01-10T11:09:50Z
```

# Provide a grouping rule for ruff formatter conflicting rules

---

_Issue opened by @gaborbernat on 2023-10-24 17:17_

Ruff warns these rules conflict with the formatter, would be nice if we could disable these in a single line then need to disable one by one:

```toml
  "COM812", # conflicts with formatter
  "COM819", # conflicts with formatter
  "E501", # conflicts with formatter
  "ISC001", # conflicts with formatter
  "Q000", # conflicts with formatter
  "Q001", # conflicts with formatter
  "Q002", # conflicts with formatter
  "Q003", # conflicts with formatter
  "W191", # conflicts with formatter
```

perhaps we could have rule FORM1 that would be an alias to all the rules conflicting with ruff formatter?

---

_Label `formatter` added by @zanieb on 2023-10-24 17:34_

---

_Comment by @Avasam on 2023-10-24 21:04_

My use case: I currently include `"ALL"` rules and deactivate the few groups and rules I don't want or conflict with type-checkers or formatters.
I don't really care about superfluous or unused rules because Ruff is so darn fast.

This ends up being less maintenance for me to stay up to date with new rules. And it's easier to document with comments what we *don't* want or agree with.

But having a way to configure "ALL but rules obsoleted by formatter" would be nice to simplify configs (and automatically stay up to date with formatter updates!).

I don't know if `501` should be included here though. Sometimes formatter can't go under max line length and it requires a manual fix (like cutting a string, moving a comment, etc)

---

_Comment by @MichaReiser on 2023-10-25 08:10_

#8196 removes the warnings for all but 2 rules, except if you changed the configuration for some of the potentially conflicting rules. 

---

_Comment by @MichaReiser on 2023-10-27 02:20_

The latest release shipped a few improvements that should drastically reduce the warnings. Please take a look and comment on this issue if you think that  a dedicated grouping for conflicting rules would still be valuable and we'll reopen the issue.

---

_Closed by @MichaReiser on 2023-10-27 02:20_

---

_Comment by @gaborbernat on 2023-10-27 02:32_

As long as the number is greater than one, I think it would still be helpful. So I'm asking to reopen this. 

---

_Reopened by @MichaReiser on 2023-10-27 02:42_

---

_Label `wish` added by @MichaReiser on 2023-10-27 02:43_

---

_Comment by @Tedpac on 2025-06-22 03:09_

Is there any progress on this?

I'm honestly a little confused, because on the one hand, there are conflicts between the linter and the formatter, but on the other hand, there are conflicts between the rules themselves.

Currently, [the documentation](https://docs.astral.sh/ruff/linter/#rule-selection) states that:

> Ruff will automatically disable any conflicting rules when `ALL` is enabled.

I suppose this resolves the conflicts between the rules, but there will still be conflicts between the linter and the formatter, so this issue remains relevant. Am I correct?

If the above is correct, there is another issue that could be relevant when using `ALL`, and that is that I personally don't like Ruff automatically disabling conflicting rules (as stated in the documentation I quoted), because if there are two conflicting rules, it would be great if the user could see the warning and then disable one of the two rules, keeping the one they prefer. If this makes sense to anyone, I can open an issue for this.

---

_Comment by @MichaReiser on 2025-06-22 08:17_

>  because if there are two conflicting rules, it would be great if the user could see the warning and then disable one of the two rules,

Ruff disables one of the rules and shows a warning. The user can then decide to decide which one of the two they want to disable.

---

```yaml
number: 16113
title: "[`refurb`] Manual timezone monkeypatching (`FURB162`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: um
created_at: 2025-02-12T05:36:15Z
updated_at: 2025-02-18T13:46:56Z
url: https://github.com/astral-sh/ruff/pull/16113
synced_at: 2026-01-12T15:55:53Z
```

# [`refurb`] Manual timezone monkeypatching (`FURB162`)

---

_@InSyncWithFoo_

## Summary

Part of #1348.

[`FURB162`](https://github.com/dosisod/refurb/blob/7649900948c8a65296ae6efc9d8ced0bc1a54a7f/refurb/checks/datetime/simplify_fromisoformat.py) reports `datetime.fromisoformat()` calls where the lone argument is of one of the following forms:

* `date.replace("Z", <zero_offset>)`
* `date[:-1] + <zero_offset>`
* `date.strip("Z") + <zero_offset>`/`date.rstrip("Z") + <zero_offset>`

...and where `<zero_offset>` is a string literal whose content is one of `00:00`, `0000` or `00` prefixed with `+` or `-`.

Before Python 3.11, this was necessary, as `.fromisoformat()` wasn't able to handle strings with `Z` as the affix. Now it can.

The logic was taken from that of the upstream rule.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-12 05:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-02-12 09:31_

---

_Label `preview` added by @MichaReiser on 2025-02-12 09:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:22 on 2025-02-17 10:06_

It might be worth to include this sentence as well: *(barring only those that support fractional hours and minutes)*. It otherwise is unclear what "most" means.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:51 on 2025-02-17 10:09_

I think we have to mark the fix as always unsafe, considering that `fromisoformat` isn't correct for all timezones and we can't know if the date uses one of those time zones.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:63 on 2025-02-17 10:12_

```suggestion
        r#"Unnecessary timezone monkeypatching"#.to_string()
```

I'm not sure if monkeypatching is the right term here. I did find one reference that uses monkeypatching in the context of time zones ([source](https://goral.net.pl/changelog/monkeypatching-timezones/)) but the more common use is in the context of patching methods/functions etc. 

How about *Unnecessary timezone replacement with zero offset*?

---

_@MichaReiser approved on 2025-02-17 11:25_

Nice, thank you

---

_@MichaReiser reviewed on 2025-02-17 11:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1113 on 2025-02-17 11:25_

```suggestion
        (Refurb, "162") => (RuleGroup::Preview, rules::refurb::rules::FromisoformatReplaceZ),
```

---

_@InSyncWithFoo reviewed on 2025-02-17 14:13_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:51 on 2025-02-17 14:13_

What timezones are you referring to? [The documentation](https://docs.python.org/3/library/datetime.html#datetime.date.fromisoformat) mentions no such limitation.

---

_@MichaReiser reviewed on 2025-02-17 14:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:51 on 2025-02-17 14:20_

I'm referring to the *most* in the rule's documentation:

>  On Python 3.11 and later, `datetime.fromisoformat()` can handle most [ISO 8601][iso-8601] formats, including ones affixed with `Z`

---

_Closed by @InSyncWithFoo on 2025-02-17 14:22_

---

_Reopened by @InSyncWithFoo on 2025-02-17 14:23_

---

_@InSyncWithFoo reviewed on 2025-02-17 14:27_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:51 on 2025-02-17 14:27_

By "most" I mean "most formats", not "most timezones":

> Return a [`date`](https://docs.python.org/3/library/datetime.html#datetime.date) corresponding to a `date_string` given in any valid ISO 8601 format, with the following exceptions:
> 
> * Reduced precision dates are not currently supported (`YYYY-MM`, `YYYY`).
> * Extended date representations are not currently supported (`±YYYYYY-MM-DD`).
> * Ordinal dates are not currently supported (`YYYY-OOO`).

Regardless, marking the fix as unsafe is necessary, since there are edge cases like this:

```python
d = 'Z2025-01-01T00:00:00Z'

datetime.fromisoformat(d.strip('Z') + '+00:00')  # fine
datetime.fromisoformat(d)                        # error
```

---

_@MichaReiser reviewed on 2025-02-18 08:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:53 on 2025-02-18 08:27_

Can we be more specific about how it could change program behavior? Ideally, the explanation would be detailed enough for users to decide if it's okay to mark the fix as safe in their project because they know this situation can't apply to their project.

---

_@MichaReiser approved on 2025-02-18 08:27_

---

_@InSyncWithFoo reviewed on 2025-02-18 08:36_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:53 on 2025-02-18 08:36_

I'm pretty sure I added a snippet right below to demonstrate this point. Is it unclear?

---

_@MichaReiser reviewed on 2025-02-18 08:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:53 on 2025-02-18 08:57_

Oh I missed that, sorry. 

I'm still not entirely sure what the *barring only* part means. To me, it suggests that there are some cases where this operation isn't "safe". Do you have more context on what it means specifically?

---

_@InSyncWithFoo reviewed on 2025-02-18 09:11_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/fromisoformat_replace_z.rs`:53 on 2025-02-18 09:11_

There's already a link to [`fromisoformat`'s documentation](https://docs.python.org/3/library/datetime.html#datetime.date.fromisoformat) in the "References" section. I added another one.

---

_Merged by @MichaReiser on 2025-02-18 13:35_

---

_Closed by @MichaReiser on 2025-02-18 13:35_

---

_Branch deleted on 2025-02-18 13:46_

---

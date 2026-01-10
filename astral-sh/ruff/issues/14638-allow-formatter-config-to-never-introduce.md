---
number: 14638
title: Allow (formatter?) config to never introduce implicitly concatenated strings or just accept that a check run will complain about it
type: issue
state: open
author: jankatins
labels:
  - configuration
  - formatter
  - needs-decision
assignees: []
created_at: 2024-11-27T16:59:01Z
updated_at: 2025-02-17T08:23:14Z
url: https://github.com/astral-sh/ruff/issues/14638
synced_at: 2026-01-10T01:22:55Z
---

# Allow (formatter?) config to never introduce implicitly concatenated strings or just accept that a check run will complain about it

---

_Issue opened by @jankatins on 2024-11-27 16:59_

I think implicitly concat'ed strings are pure evil since a time where I had to debug an accidentally added comma in a place where `+` would have been proper, which made a single string a tuple of two strings. I would like ruff to allow one to configure that "no explicit string concat, and not fix it such a way, ever" and not complain about such a config, but currently it does complain and IMO it complains for the wrong reasons and with the wrong solution: 

I don't want it to stop complaining about implicitly concat'ed strings, but stop the formatter from introducing them by using a '+' between multiline strings. Or if the formatter needs to introduce implicitly concat'ed strings (because + would be a black violation), just let `ruff check` complain (when it happens), so I'm forced to fix it by hand (which I would be fine with). But having the scary warning in all runs makes me and especially unwary users scared into disabling that rule. :-(

Inspired by https://github.com/astral-sh/ruff/issues/13031#issuecomment-2302402183, I would have expected that

```toml
[tool.ruff.lint]
extend-select = ["ISC"]
ignore=["ISC003"] # Don't complain about explicitly concat'ed strings
unfixable = ["ISC001"] # don't fix one line implicitly concat'ed strings, but just complain and make the user fix it

[tool.ruff.lint.flake8-implicit-str-concat]
allow-multiline = false
```

would work, but ruff still complains, that this is incompatible with the formatter:

```
 The following rule may cause conflicts when used with the formatter: `ISC001`. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `select` or `extend-select` configuration, or adding it to the `ignore` configuration.
```

---

_Comment by @MichaReiser on 2024-11-27 17:32_

I think what you're asking for will be addressed with the upcoming Ruff 2025 style guide (#13371). The relevant changes are:

* The formatter will merge implicitly concatenated strings that fit on a single line into a single string literal (there are a few edge cases where that's not possible but they are very rare)
* The formatter will never collapse an implicit concatenated string https://github.com/astral-sh/ruff/pull/13928

These two changes will make the formatter compatible with ISC001 (also see https://github.com/astral-sh/ruff/issues/8272). 

---

_Label `question` added by @MichaReiser on 2024-11-27 17:32_

---

_Comment by @MichaReiser on 2024-11-27 17:38_

> would work, but ruff still complains, that this is incompatible with the formatter:

This might be a bug but I'm not sure if it's still worth fixing considering that the new style guide will be released shortly. But feel free to submit a PR that suppresses the warning if that setting is set.

---

_Renamed from "Allow (formatter?) config to never introduce implicit concatted strings or just accept that a check run will complain about it" to "Allow (formatter?) config to never introduce implicitly concatenated strings or just accept that a check run will complain about it" by @AlexWaygood on 2024-11-27 21:45_

---

_Comment by @jankatins on 2024-11-28 00:36_

> The formatter will merge implicitly concatenated strings that fit on a single line into a single string literal (there are a few edge cases where that's not possible but they are very rare)

This is actually something I don't want: up to now, all such cases of mine were bad coding from my side and in a lot of cases I had to pick a comma instead of a  plus. Just merging the strings will mean that `["A", "B" "C"]` will silently end up as `["A", "BC"]` which usually is a bug in my case.

---

_Comment by @MichaReiser on 2024-11-28 06:50_

I see. So the problem here is that you introduce implicit concatenated strings and you want the formatter to leave them unchanged? 

I don't think we should introduce an option for this because it is in direct conflict with consistent formatting. However, I do see how the formatter joining the strings can be "annoying" in an editor context. IMO, the benefit of the formatter joining the strings is that it becomes very apparent that you forgot a `,` and hitting Ctrl + Z in the editor should undo the formatter change.





> but stop the formatter from introducing them by using a '+' between multiline strings. Or if the formatter needs to introduce implicitly concat'ed strings (because + would be a black violation), 

The formatter never introduces implicit concatenated strings. It only formats implicit concatenated strings that were already present in the source. 


---

_Label `formatter` added by @MichaReiser on 2025-02-17 08:23_

---

_Label `question` removed by @MichaReiser on 2025-02-17 08:23_

---

_Label `configuration` added by @MichaReiser on 2025-02-17 08:23_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-17 08:23_

---

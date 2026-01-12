```yaml
number: 15710
title: "Request: Document which linter rules affect the formatter"
type: issue
state: closed
author: Phrogz
labels:
  - question
assignees: []
created_at: 2025-01-24T06:29:58Z
updated_at: 2025-01-27T00:29:30Z
url: https://github.com/astral-sh/ruff/issues/15710
synced_at: 2026-01-12T15:54:54Z
```

# Request: Document which linter rules affect the formatter

---

_@Phrogz_

### Description

I discovered tonight that the [rules for linting](https://docs.astral.sh/ruff/rules/) are different than the [rules available during formatting](https://docs.astral.sh/ruff/settings/#format). (Makes me sad, because I wanted to tweak a few whitespace-normalization rules that get applied during format-on-save.)

I was particularly confused by this, however, because at least one linter rule DOES affect the format-on-save functionality:

```toml
[tool.ruff.lint]
ignore = ["F401"]
```

Ignoring that _linter_ rule causes the format-on-save to no longer remove unused imports every time I save a file. However, this is not mentioned anywhere in the formatting options.

Are there other linter rules that affect the formatting?

---

_Label `question` added by @MichaReiser on 2025-01-24 07:22_

---

_Comment by @MichaReiser on 2025-01-24 07:25_

I'm sorry for the confusion for what runs as part of the formatter and what is part of the linter. 

Organizing imports (including sorting with isort) isn't part of the formatter (`ruff format`). They're part of the linter and run as part of `ruff check` (ruff check only runs the linter for now). 

Generally speaking, All configurations below `ruff.lint` are specific to the linter and do not change the behavior of `ruff format.` 

It does make sense that ignoring `F401` disables the removal of unused imports because that's what `F401` does. 

---

_Comment by @dylwil3 on 2025-01-24 09:23_

Could you say how you are invoking `ruff` for "format on save"? Is it possible you're doing something more like "apply lint fixes and format on save"? 

---

_Comment by @Phrogz on 2025-01-24 22:28_

> Could you say how you are invoking `ruff` for "format on save"? Is it possible you're doing something more like "apply lint fixes and format on save"?

Ah, bingo. You've got it. I'd only seen these lines in the (created by coworker) VS Code workspace previously:

```json
"editor.defaultFormatter": "charliermarsh.ruff",
"editor.formatOnSave": true,
```

…and did not notice these ones, as well:

```json
"editor.codeActionsOnSave": {
	"source.fixAll": "explicit",
	"source.organizeImports": "explicit"
},
```

It's the `"source.fixAll": "explicit"` along with the linter using that rule to flag the unused imports that's removing the unused imports.



---

_Closed by @Phrogz on 2025-01-24 22:28_

---

_Comment by @Phrogz on 2025-01-25 00:03_

I've closed this request, as I understand it does not make sense. However, I hope the maintainers will consider the interplay of a "linter problem that has a quick fix that can be auto-applied on save" and the formatter. If every deviation from the desired formatting was a linter error, that had a quick fix, then the formatter would not be needed at all…and you'd get the full granularity of linting rules for controlling formatting.

---

_Comment by @dylwil3 on 2025-01-25 02:17_

> If every deviation from the desired formatting was a linter error, that had a quick fix, then the formatter would not be needed at all…and you'd get the full granularity of linting rules for controlling formatting.

I can't tell from the way you've worded this whether you'd consider it good or bad for formatting to have the same granularity as linting rules. Could you clarify?

---

_Comment by @Phrogz on 2025-01-27 00:28_

I'll answer in the selfish specific first, and then generally from a project standpoint:

My current team desires a few specific deviations from the standard Ruff formatting. Specifically, we want to be able to line up values across multiple lines, in similar method calls, data structures, and commands. For example:

```python
foo( "xxx", "yyy", "zzz" )
foo( "x",   "y",   "z"   )

xxx = 123 # foo
y   = 4   # bar

z = [
    { "foo": "bar",    "jim": "jam"   },
    { "foo": "bazoom", "jim": "jammy" },
]
```

If there were 1-4 rules/configurations we could tweak to change the formatting to NOT normalize whitespace in these situations, we would be happy.

If you ended up wanting to offer a solution to this (or similar), you might add additional customizations to the formatter. Or…you might unify the highly-customized linter and the formatter into a single code path.

I'm a total outsider without any of your collective knowledge and experience building Ruff. If you told me that there are some fundamental incompatibilities between lint fixes and formatting, such that the linter could never become a formatter, I would believe you. I'm just smelling some strong overlap between linting, lint-fixing, and formatting.

---

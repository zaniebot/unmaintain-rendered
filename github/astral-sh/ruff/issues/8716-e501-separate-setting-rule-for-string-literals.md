---
number: 8716
title: "E501: Separate setting/rule for string literals"
type: issue
state: closed
author: ThiefMaster
labels:
  - configuration
  - needs-info
assignees: []
created_at: 2023-11-16T10:26:08Z
updated_at: 2024-07-14T20:11:43Z
url: https://github.com/astral-sh/ruff/issues/8716
synced_at: 2026-01-07T13:12:15-06:00
---

# E501: Separate setting/rule for string literals

---

_Issue opened by @ThiefMaster on 2023-11-16 10:26_

It would be nice if `E501` had  either a separate rule for string literals or if there was a `max-literal-line-length` setting to set a (higher/unlimited) length for string literals.

My usecase is database migrations which contain (triple-quoted multiline) string literals with raw SQL, like [this one](https://github.com/indico/indico/blob/9b6dc5e04b1827203320f4ce88eed10389813f92/indico/migrations/versions/20170905_1521_640584a3987e_add_event_roles_to_acls.py#L44). Often these are more readable when you write a statement in one line, rather than wrapping them. For example, when changing many constraints becomes very unreadable if there's not one query per line.

However, at the same time I do NOT want to excessively long lines of Python code in those files. So disabling `E501` for these files requires me to manually check all the code lines.

Some thoughts on the various options to handle this:

- New rule: Maybe a bit invasive since it'd mean `E501` would no longer check string literals, and someone not enabling the new rule would suddenly lose functionality. But also most flexible to enable/disable on a per-file basis.
- New string literal line length setting: Straightforward, the same rule would handle everything. But it could not easily be done only for certain files via `per-file-ignores` (enabling both rules globally, and disabling the new one for files matching a certain pattern)

---

_Renamed from "E501 string literals" to "E501: Separate setting/rule for string literals" by @ThiefMaster on 2023-11-16 10:26_

---

_Comment by @zanieb on 2023-11-16 16:10_

I guess my recommendation here is to use a code formatter to enforce line length and turn off this rule.

Happy to hear from more people that want this setting though.


---

_Label `configuration` added by @zanieb on 2023-11-16 16:10_

---

_Label `needs-decision` added by @zanieb on 2023-11-16 16:10_

---

_Comment by @charliermarsh on 2023-11-16 16:13_

Alternatively, I'd probably suggest just adding a `# noqa: E501` to the end of those multiline strings.

---

_Label `needs-decision` removed by @MichaReiser on 2023-11-27 06:50_

---

_Label `question` added by @MichaReiser on 2023-11-27 06:50_

---

_Label `needs-decision` added by @MichaReiser on 2023-11-27 06:50_

---

_Label `waiting-on-author` added by @MichaReiser on 2023-11-27 06:51_

---

_Label `question` removed by @MichaReiser on 2023-11-27 06:51_

---

_Label `needs-decision` removed by @MichaReiser on 2023-11-27 06:51_

---

_Comment by @MichaReiser on 2023-11-27 06:52_

@ThiefMaster would any of the above suggestions work for you? You can also disable `E501` for the entire file or all files in the migration directory.

---

_Comment by @charliermarsh on 2024-01-28 19:00_

I'm gonna close for now given inactivity. (The second point could be seen as a use-case for #7696.)

---

_Closed by @charliermarsh on 2024-01-28 19:00_

---

_Comment by @mdengler on 2024-07-14 20:10_

> I guess my recommendation here is to use a code formatter to enforce line length and turn off this rule.
> 
> Happy to hear from more people that want this setting though.

I'd like this setting, since I'd like not to have to use a "ignore W505,E501 in linter and use formatter to avoid E501-violations-except-in-literals" approach.  ignore-in-linter-and-enforce-using-formatter works for sufficiently-opinionated projects where the formatter does what the project wants; but non-opinionated purists may reasonably bristle at a linter rule that overrides explicit well-behaved coder intentions like "I want exactly this string literal" and I think a setting like the OP's would be very nice.

There are a number of theoretical and practical use-cases for such a setting:

1. string literals where python- and side-by-side-diff-considerations bend to higher-priority readability ones, like the OP's SQL-string example; and
2. string/comments for which people may `grep` (or equivalent); for example, if the user sees an error `multi-word command that is long failed; please consider long-solution text instead` in a log / warning message, does `grep "multi-word command"` that finds nothing because `ruff format` split "multi-word" and "command" over multiple lines in the source file because the string literal exceeded the `line-length`, the user is frustrated and the coder's nice verbose message is somewhat wasted

---

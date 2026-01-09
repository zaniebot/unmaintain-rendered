---
number: 6611
title: Warn about incorrect formatter suppression comments
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-16T08:42:43Z
updated_at: 2024-02-28T19:21:08Z
url: https://github.com/astral-sh/ruff/issues/6611
synced_at: 2026-01-07T13:12:15-06:00
---

# Warn about incorrect formatter suppression comments

---

_Issue opened by @MichaReiser on 2023-08-16 08:42_

Ruff's formatter only supports suppression comments in the following  positions (https://github.com/astral-sh/ruff/discussions/6338):

* `fmt: off..fmt: on`: Leading or trailing statement own line comments (but e.g. not between decorators, or inside of expressions)
* `fmt: skip`: As a trailing comment of
	* simple statements
	* decorators
	* Clause headers (everything that ends with a `:` and is followed by an indent)

We should warn if one of these comments is used in an incorrect place and, ideally, provide a code fix that moves the comment to the right position so that it suppresses the formatting of the attributed node. 

We could also warn about `fmt: off` comments that miss a corresponding `fmt: on` comment. Omitting the `fmt: on` is valid according to the range suppression definition but can be caused by refactors where you accidentally delete the `fmt: on` comment (or even a ruff fix)

## Implementation

That's where things get interesting. We can either implement this as part of the formatter OR the linter. The formatter seems a good fit because it is inherently a formatter problem. The formatter has extensive comments handling infrastructure and can track the "handled" suppression comments while formatting, ensuring that what the formatter does and the code warning about correct usages are perfectly in sync. 

However, the formatter lacks the infrastructure to report diagnostics (other than a formatting diff), provide and apply code fixes, and a mechanism to opt in/opt out to these additional checks (e.g. some may prefer to only write the  `fmt: on` when absolutely necessary). From that standpoint, the linter would be a better fit and is what I prefer. 

Us having the benefit of picking the best tool to solve problems like this instead of implementing it in the only tool that we have available (e.g. in the formatter) is a benefit of a unified toolchain. However this raises a couple of questions

* Should these check run as part of `ruff format` or `ruff check/lint` or both? 
* Should the incorrect `fmt: off` lint be enabled automatically when using the formatter (not an optional rule)
* How and where should these rules be configured
* Should these rules run even if Ruff's linter is disabled?

Our `isort` implementation probably falls into the same category. We implemented it based on our linter infrastructure, even though it isn't really linting in that sense (it falls in between linting and formatting). The way Rome solved this is that it had a dedicated organize imports action in the editor. This has the advantage that a granular configuration on whether imports, code fixes, or formatting should run on save. 


**Internal only**: [More design notes](https://www.notion.so/astral-sh/Configuration-c291a149e50f438083221a65468a736f?pvs=4#53f40fda72a14e148c104c0ce7d180a1)


---

_Label `formatter` added by @MichaReiser on 2023-08-16 08:42_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-16 08:42_

---

_Comment by @MichaReiser on 2023-08-16 16:04_

CC: @zanieb who's working on the CLI design

---

_Referenced in [astral-sh/ruff#6845](../../astral-sh/ruff/issues/6845.md) on 2023-08-24 09:12_

---

_Referenced in [astral-sh/ruff#7232](../../astral-sh/ruff/issues/7232.md) on 2023-09-08 07:42_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-15 10:06_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-15 10:06_

---

_Referenced in [pypa/pip#12478](../../pypa/pip/pulls/12478.md) on 2024-01-15 14:30_

---

_Comment by @pfmoore on 2024-01-15 16:07_

Very much looking at this from a user point of view, not an implementer point of view, I would strongly request that ruff reports any `# fmt: off` directives that would be recognised by other formatters such as black, but which would be ignored by ruff. Otherwise, there's a risk when transitioning to a new formatter that code which was manually formatted will suddenly get auto-formatted without warning.

In practice, I would be even happier if formatters could agree on a common behaviour for when formatting directives are recognised and what they mean, so that there would be no need for a warning like this anyway. But maybe things need to mature a little before that makes sense.

---

_Assigned to @snowsignal by @snowsignal on 2024-01-29 16:46_

---

_Referenced in [astral-sh/ruff#9899](../../astral-sh/ruff/pulls/9899.md) on 2024-02-08 19:25_

---

_Closed by @snowsignal on 2024-02-28 19:21_

---

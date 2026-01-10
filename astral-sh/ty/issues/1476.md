```yaml
number: 1476
title: Implementing junit style reports
type: issue
state: open
author: cetanu
labels:
  - cli
assignees: []
created_at: 2025-11-04T02:53:59Z
updated_at: 2025-12-22T11:18:12Z
url: https://github.com/astral-sh/ty/issues/1476
synced_at: 2026-01-10T01:56:40Z
```

# Implementing junit style reports

---

_Issue opened by @cetanu on 2025-11-04 02:53_

### Question

Hello friends,

I'd like to contribute some code to this project, which as the title alludes to, is JUnit style XML reports!
This format is commonly used by CI servers to produce a formatted report that is easy for users to read.

I thought I would firstly ask if this is something you'd like in your project.
I anticipate some portion of people moving across from mypy to ty will want this feature (me included)

The second part of my question, is for guidance as to how and where I would start on this because I assume there may be a preference from the maintainers.

From a brief glance, it seems that I may have to write this code in rust, and add it to the ruff codebase, in the `ty` crate, possibly by modifying the Printer? If there is a better crate for this, or a new crate would be required, I am happy to do it in whatever way is desired.

### Version

_No response_

---

_Label `question` added by @cetanu on 2025-11-04 02:54_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-04 04:31_

---

_Label `question` removed by @MichaReiser on 2025-11-04 13:23_

---

_Label `cli` added by @MichaReiser on 2025-11-04 13:23_

---

_Label `diagnostics` removed by @MichaReiser on 2025-11-04 13:23_

---

_Comment by @MichaReiser on 2025-11-04 13:28_

Thanks for offering your help and creating an issue first. 

We already have a [JUnit reporter](https://github.com/astral-sh/ruff/blob/997dc2e7ccfc378d60e1b98c3fd244eb6b37ba70/crates/ruff_db/src/diagnostic/render/junit.rs#L22) for Ruff and ty shares that infrastructure with Ruff. That means, adding the JUnit output format should mostly require funneling the CLI option through to where we decide to render the diagnostics and reviewing and generalizing any Ruff-specific logic within the JUnit reporter. 

Another thing worth checking is that the reporter emits all necessary information (e.g. sub diagnostics and multiple spans). It's not something we necessarily have to do in a first version but diagnostics might be hard to understand when omitting some of the details.

CC: @ntBre 

---

_Comment by @ntBre on 2025-11-04 14:29_

This looks like a pretty similar PR wiring up our GitLab output format: astral-sh/ruff#20155, in case it helps.

But Micha is right, `ruff` is hard-coded several places in the current version (see the [GitHub renderer](https://github.com/astral-sh/ruff/blob/fefbec4232ff7de4bddb776788a86c091b3069bd/crates/ruff_db/src/diagnostic/render/github.rs#L3) for another case where we handled that), and it won't render any sub-diagnostics or additional spans.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:21_

---

_Comment by @cetanu on 2025-11-28 12:46_

Took a look and the first few steps are pretty easy, I think I understand what you mean about the sub-diagnostics, and I think I would like to at least attempt to have those in the first implementation, but I'll see how far I get

---

_Comment by @cetanu on 2025-12-22 02:51_

Progress report:

Spent yesterday working on this instead of playing video games all day 😄 

### Added sub-diagnostics to the XML report ✅ 

I wasn't previously familiar with what these were, but now I see that they are the info/help/error messages that follow the primary failure/lint problem.

I was able to get them into the report pretty easily

### Improved the XML formatting somewhat ✅ 

Basically just made the failure message indented. It seems to be parsed the same way, and is more human readable.

### Trying to get the code snippets rendered ⚠️ 

I found some part of the codebase which deals with annotating code snippets and have tried to get that working.

Current problem: the line numbers in the gutter are wrong - they seem to start at the line of the lint problem, rather than their actual lines.

### Thoughts about generalizing the renderer even more ⚠️ 

I noticed that there are things like `org.ruff.{code}` and package names which point to ruff as well.

I added a note, inside the code that adds the sub diagnostics to the output, stating that the report/class/package names could be switched to `Ty` within that section, but not sure how appropriate it is to do so.

For me, I thought the alternative would be to get the Ty cli to pass down a small piece of config to the renderer, and have the default set to ruff.



---

_Comment by @MichaReiser on 2025-12-22 11:18_

I replied on the pull request

---

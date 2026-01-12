```yaml
number: 612
title: pyflakes F632 Autofix
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: pyflakes-f632-autofix
created_at: 2022-11-05T21:42:30Z
updated_at: 2022-12-27T05:42:49Z
url: https://github.com/astral-sh/ruff/pull/612
synced_at: 2026-01-12T15:55:05Z
```

# pyflakes F632 Autofix

---

_@squiddy_

I haven't done much with Rust yet, so any feedback is appreciated.

---

_Comment by @charliermarsh on 2022-11-05 22:06_

Awesome! Thank you for this. Will review tonight or tomorrow AM.

---

_Comment by @squiddy on 2022-11-05 22:18_

Thank you.

I've since found `SourceGenerator` and it might be better to use that instead of formatting the replacement code manually. I'll give it a try. 

---

_Comment by @charliermarsh on 2022-11-05 22:42_

Yeah using SourceCodeGenerator would be preferable. Even better would be to try and implement the fix with LibCST since then we can preserve more of the original formatting. But it’s a little more involved and SourceCodeGenerator is good too. If you’re interested in the LibCST approach, you could look at how the flake8-comprehensions fixes are implemented.

---

_Comment by @squiddy on 2022-11-05 22:46_

Agreed, by the looks of it it is more involved, but I've dabbled with LibCST before so I'll give it a try.

---

_Converted to draft by @squiddy on 2022-11-05 22:49_

---

_Comment by @charliermarsh on 2022-11-05 22:52_

The strategy isn’t super well-documented but the basic idea is to extract the source code for the expression or statement, parse it with LibCST, modify the tree, then generate source code from the modified tree (and structure the fix as a replacement of the old source code with the new).

---

_Marked ready for review by @squiddy on 2022-11-06 09:20_

---

_Comment by @squiddy on 2022-11-06 11:38_

What's not clear to me currently is where to put code. I think in a different issue you mentioned you prefer more complex code into plugins. Are those supposed to be just an entrypoint and delegating to implementation in checks/fixes? Some trivial plugins just inline the logic.

---

_Comment by @charliermarsh on 2022-11-06 11:45_

What you have here looks good / consistent with respect to code organization. “Checks” are typically small in size with no autofix. “Plugins” are typically more complex and/or benefit from direct access to the Checker to orchestrate autofixing. Some plugins call through to logic in “checks” but often I find that overkill.

---

_Comment by @squiddy on 2022-11-06 11:47_

Thanks for the clarification.

Amended the last commit to update the README to indicate this rule can be fixed automatically.

---

_@charliermarsh reviewed on 2022-11-06 11:47_

---

_Review comment by @charliermarsh on `src/pyflakes/plugins/invalid_literal_comparisons.rs`:39 on 2022-11-06 11:47_

Can this be accessed via checker.locator, rather than as an independent argument?

---

_Comment by @charliermarsh on 2022-11-06 11:48_

Looks great! Am especially impressed if you haven’t done much Rust yet :)

---

_@squiddy reviewed on 2022-11-06 11:53_

---

_Review comment by @squiddy on `src/pyflakes/plugins/invalid_literal_comparisons.rs`:39 on 2022-11-06 11:53_

Oh, good catch. Took inspiration from some other plugin here, but checker.locator is nicer. Done.

---

_Merged by @charliermarsh on 2022-11-06 11:57_

---

_Closed by @charliermarsh on 2022-11-06 11:57_

---

_Comment by @charliermarsh on 2022-11-06 11:58_

Thank you for this! Pumped to have you as a contributor.

---

_Branch deleted on 2022-12-27 05:42_

---

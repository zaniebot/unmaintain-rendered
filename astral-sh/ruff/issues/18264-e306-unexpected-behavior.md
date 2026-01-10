```yaml
number: 18264
title: "E306: unexpected behavior"
type: issue
state: closed
author: mikelei8291
labels:
  - question
assignees: []
created_at: 2025-05-22T19:56:51Z
updated_at: 2025-05-24T01:55:19Z
url: https://github.com/astral-sh/ruff/issues/18264
synced_at: 2026-01-10T11:09:58Z
```

# E306: unexpected behavior

---

_Issue opened by @mikelei8291 on 2025-05-22 19:56_

### Summary

In the [example](https://docs.astral.sh/ruff/rules/blank-lines-before-nested-definition/#example) of E306, there's no blank line required before the first nested function, but only between two nested functions. However, if there were lines of code before the first nested function, it will be reported as a style error.

![Image](https://github.com/user-attachments/assets/0c4144d3-aa35-439f-b7b0-aaf399ff4544)

### Version

ruff 0.11.10

---

_Comment by @maxmynter on 2025-05-23 10:57_

[Flake8](https://www.flake8rules.com/rules/E306.html) recommends inner methods to be surrounded by one blank line. 

[PEP8](https://peps.python.org/pep-0008/#blank-lines) only mentions the inner methods of a class, not nested functions, but states "surrounded by a single blank line". 

E306 is modeled after Pycodestyle which tests against PEP8. 

So I guess the unexpected behaviour is rather that the error is not reported in the second example and we should update the  E306. We should also clear up the wording in the docs to include nested functions.

While we're at it, we should update the broken link on E306 to the typing styleguide. I believe [this](https://typing.python.org/en/latest/guides/writing_stubs.html#blank-lines) is the correct one. 

Happy to provide a fix if someone from Astral agrees with this outline.

---

_Comment by @MichaReiser on 2025-05-23 13:12_

PEP8 says

> Extra blank lines may be used (sparingly) to separate groups of related functions. Blank lines may be omitted between a bunch of related one-liners (e.g. a set of dummy implementations).

So E306 is correct here to not report an error. This is also consistent with Ruff's formatter (and changing it would make the rule incompatible with the formatter).

The reason it requires a blank line after a non-function statement is because it's otherwise very easy to "miss" that there are any statements before the function.

Because of that, I don't think there's anything we should change here (other than the maybe broken docs)

---

_Label `question` added by @MichaReiser on 2025-05-23 13:13_

---

_Comment by @mikelei8291 on 2025-05-23 18:37_

> The reason it requires a blank line after a non-function statement is because it's otherwise very easy to "miss" that there are any statements before the function.

Good point, thanks for the explanation.

However, does it mean that the "best practice" shown on the [flake8 rules page of E306](https://www.flake8rules.com/rules/E306.html) is not correct?

---

_Comment by @MichaReiser on 2025-05-23 19:08_

> However, does it mean that the "best practice" shown on the [flake8 rules page of E306](https://www.flake8rules.com/rules/E306.html) is not correct?

It probably depends on who you ask :) 

---

_Comment by @mikelei8291 on 2025-05-24 01:55_

I'm closing this due to the fact that there is nothing to fix for Ruff. Thanks again for the responses. I think it's a good idea to open a new issue/pr for the broken doc link.

---

_Closed by @mikelei8291 on 2025-05-24 01:55_

---

```yaml
number: 4034
title: Clarify section on installing into arbitrary Python environments
type: pull_request
state: merged
author: dpoznik
labels: []
assignees: []
merged: true
base: zb/docs-discovery
head: zb/docs-discovery-dpoznik
created_at: 2024-06-05T01:16:46Z
updated_at: 2024-06-06T14:44:19Z
url: https://github.com/astral-sh/uv/pull/4034
synced_at: 2026-01-12T16:06:00Z
```

# Clarify section on installing into arbitrary Python environments

---

_@dpoznik_

## Summary

This meta-PR includes some suggestions for the docs on installing into arbitrary Python environments.

It targets the branch in #4031 and is a response to https://github.com/astral-sh/uv/issues/3951#issuecomment-2148595865.

Specifically, it:
- Moves mention of `--python=$(which python)` out of the `--system` paragraph, as the two are no longer equivalent
- Adds additional notes on how, currently, to install for the current interpreter. This section should be updated once #4009 is implemented :)

---

_Review comment by @dpoznik on `README.md`:188 on 2024-06-05 01:18_

Moved `--python=$(which python3)` to previous paragraph, since no longer equivalent to `--system`.

---

_Review comment by @dpoznik on `README.md`:188 on 2024-06-05 01:19_

Added `python -m uv` suggestion gleaned from https://github.com/astral-sh/uv/releases/tag/0.2.0.


---

_Review comment by @dpoznik on `README.md`:202 on 2024-06-05 01:21_

I wasn't sure how to interpret this:
> When the `--system` flag is provided, uv will ignore any interpreters that are not in [PEP 405 compliant](https://peps.python.org/pep-0405/#specification) virtual environments. Similarly, if the `--system` flag is not provided, uv will ignore any interpreters that are in not virtual environments.

This paragraph is a guess, but it may not be correct.

---

_@dpoznik reviewed on 2024-06-05 01:22_

---

_Marked ready for review by @dpoznik on 2024-06-05 01:23_

---

_Assigned to @zanieb by @zanieb on 2024-06-05 12:17_

---

_@zanieb reviewed on 2024-06-05 12:18_

---

_Review comment by @zanieb on `README.md`:188 on 2024-06-05 12:18_

It's only saying it's an "approximate" shorthand. I think it's rare that your `python` is a shim which automatically activates environments on invocation? Just responding to your comment though I might be okay with where it is now.

---

_@zanieb reviewed on 2024-06-05 12:18_

---

_Review comment by @zanieb on `README.md`:188 on 2024-06-05 12:18_

I'm hesitant to suggest this since it's slower than using `uv` directly (you need to pay the cost of Python interpreter startup). Might be worth saying something about which interpreter is used in this case regardless though.

---

_@zanieb reviewed on 2024-06-05 12:21_

---

_Review comment by @zanieb on `README.md`:202 on 2024-06-05 12:21_

Ah I find your paragraph confusing, but I inverted the clauses in mine (it's incorrect).

---

_Comment by @zanieb on 2024-06-05 12:22_

Thanks for doing this! I'll give it a closer read through later today

---

_Closed by @zanieb on 2024-06-05 12:22_

---

_Reopened by @zanieb on 2024-06-05 12:22_

---

_Comment by @zanieb on 2024-06-05 12:22_

Sorry, wrong button :)

---

_Review comment by @dpoznik on `README.md`:188 on 2024-06-05 17:32_

> It's only saying it's an "approximate" shorthand.

Sure, I get that. I guess what was confusing for me was than in uv<0.2, `--system` seemed more or less equivalent to `--python=$(which python3)`, whereas in uv>=0.2, this was no longer the case, at least for my use-case. Moreover, it's not currently clear in the text on `main` under which conditions the equivalence holds.

> I think it's rare that your `python` is a shim which automatically activates environments on invocation?

Hmm, as a pyenv-virtualenv user, this is always the case for me, but I'm not sure how prevalent this is, writ large.

I guess the main suggestion here is to provide clarity as to what `--system` does: perhaps including how it may be similar to `--python=$(which python3)` and how it differs.

---

_Review comment by @dpoznik on `README.md`:188 on 2024-06-05 17:34_

Ah, gotcha. Sorry, I didn't appreciate that. Thanks for the heads-up. I'll amend...

---

_Review comment by @dpoznik on `README.md`:202 on 2024-06-05 17:36_

OK, since I didn't understand your version, I figured it must have included a misstatement :)
My text was an attempt to rephrase yours under the assumption it was correct as written, which means mine is also wrong. I'll revert and let you patch your version.


---

_@dpoznik reviewed on 2024-06-05 17:36_

---

_@dpoznik reviewed on 2024-06-05 17:48_

---

_Review comment by @dpoznik on `README.md`:188 on 2024-06-05 17:48_

Ah, sorry. I didn't see your force-push. Will amend again...

---

_Merged by @zanieb on 2024-06-06 13:01_

---

_Closed by @zanieb on 2024-06-06 13:01_

---

_Branch deleted on 2024-06-06 14:44_

---

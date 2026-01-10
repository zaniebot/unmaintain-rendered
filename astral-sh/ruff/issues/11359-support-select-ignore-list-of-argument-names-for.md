---
number: 11359
title: "Support select/ignore list of argument names for `ARG` rules"
type: issue
state: open
author: mal
labels:
  - configuration
assignees: []
created_at: 2024-05-10T08:57:57Z
updated_at: 2024-05-20T00:56:03Z
url: https://github.com/astral-sh/ruff/issues/11359
synced_at: 2026-01-10T01:22:51Z
---

# Support select/ignore list of argument names for `ARG` rules

---

_Issue opened by @mal on 2024-05-10 08:57_

Keywords: `ARG`, `ARG001`, `unused arguments`

In cases where I have wanted to turn the ARG rules on it's typically only been certain arguments I care about policing (especially where meta-programming is involved), so it would be nice to be able to select/ignore certain argument names on which for ARG to report.

This is achievable with minor local modifications to the original flake8 plug-in, but so far as I am aware there's no low-cast way to do the same in `ruff`.

---

_Comment by @charliermarsh on 2024-05-10 13:23_

You could use [`dummy-variable-rgx`](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx) which I believe will ignore `ARG` rules for variables that match the pattern. But outside of that we'd need a dedicated setting. What are some examples of the names you wanted to ignore here, just to help w/ motivating use-cases?

---

_Label `configuration` added by @charliermarsh on 2024-05-10 13:24_

---

_Comment by @mal on 2024-05-11 11:29_

In my case we have a dispatcher which inspects a function's signature to determine whether it should construct and send a context object. The construction of that object while not awfully slow, also isn't free. Sometimes we're able to refactor a handler function to remove the need for that context object, in which case the signature _should_ be updated such that it never gets built or sent,

Using the ARG rules to check if this consistently named argument is ever unused is very appealing. At the same time, the code base is littered with various overridden methods (but being only py3.10, we don't yet have access to `@typing.override`), and monkey-patched functions which don't necessarily make use of all the args they must accept (and aren't suitable for that annotation afaiaa), so turning it on without being able to restrict it down the arg names we care about ends up filling the report with issues we can't easily resolve.

Unfortunately `dummy-variable-rgx`, being exclusionary and without support for lookaround (i.e. can't do `^(?!abc$)`), means (at least so far as I am aware) that it's not possible to use it to only check certain names. Perhaps a more lightweight solution than a select/ignore system would be something like `check-variable-rgx` which would serve as the inverse to dummy?

I understand this is very edge-case-y and so I appreciate your taking the time to dig a little deeper. 🙂 

---

_Comment by @liiight on 2024-05-19 14:19_

I got to this issue after encountering it as well. While I wasn't aware of @charliermarsh suggestion to use [dummy-variable-rgx](https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx) which i'll definitely try, I recommend taking a look at the source of the ARG rule as they use some configuration in that flake8 project: https://github.com/nhoad/flake8-unused-arguments?tab=readme-ov-file#flake8-unused-arguments

Perhaps some of that configuration could be ported for Ruff as well?

Thanks in advance by the way, Ruff is amazing!

---

_Comment by @charliermarsh on 2024-05-20 00:56_

Thanks! We do support `ignore-variadic-names`, and then we do most of those things by default (we ignore overloads, overrides, abstract methods, dunders, and stubs). @liiight are you looking to ignore a specific name / pattern here? Just wondering if adding a setting to allow certain unused names or patterns would solve your problem.

---

_Referenced in [wafer-space/platform.wafer.space#43](../../wafer-space/platform.wafer.space/pulls/43.md) on 2025-11-27 01:57_

---

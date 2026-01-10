---
number: 8553
title: Indicate successful check
type: issue
state: closed
author: bessman
labels:
  - good first issue
  - cli
assignees: []
created_at: 2023-11-08T10:13:51Z
updated_at: 2024-04-12T20:24:41Z
url: https://github.com/astral-sh/ruff/issues/8553
synced_at: 2026-01-10T01:22:48Z
---

# Indicate successful check

---

_Issue opened by @bessman on 2023-11-08 10:13_

Two of the five testimonials in the README mention intentionally introducing errors to be sure ruff was even running. I admit I did the same when I first tried ruff. It's cool that ruff is so fast people aren't sure it ran at all, but perhaps it is also a UX problem?

When black finds nothing to complain about, it says:

> All done! ✨ 🍰 ✨
> \<n\> files left unchanged.

pylint:

> Your code has been rated at 10.00/10

mypy:

> Success: no issues found in \<n\> source files

Perhaps ruff could do something similar if no errors are found? For example:

> $ ruff check .
> Found 0 errors in \<n\> files :tada:

On the other hand, several linters are silent if they find nothing (pycodestyle, flake8, isort, etc.).

---

_Comment by @zanieb on 2023-11-08 19:27_

I'm into a message if there are no errors and indicating the number of number of source files sounds nice. I'd prefer not to include an emoji.

---

_Label `cli` added by @zanieb on 2023-11-08 19:27_

---

_Comment by @doolio on 2023-11-09 09:01_

> On the other hand, several linters are silent if they find nothing (pycodestyle, flake8, isort, etc.).

I presume those that prefer that approach could still achieve it with the  `--quiet` or `--silent` options.

---

_Label `good first issue` added by @zanieb on 2023-11-09 14:57_

---

_Referenced in [astral-sh/ruff#8631](../../astral-sh/ruff/pulls/8631.md) on 2023-11-12 18:00_

---

_Referenced in [astral-sh/ruff#9503](../../astral-sh/ruff/pulls/9503.md) on 2024-01-13 07:14_

---

_Comment by @vladNed on 2024-02-25 17:21_

is this issue still available ? I would like to give it a go and implement a nice message output for successful checks. 

I got inspired from `black`, as it always shows the cake when everything was ok


---

_Comment by @zanieb on 2024-02-25 23:49_

Well it's not done but there are a couple existing pull requests

@hughhan1 opened a pull request but noted that @vinhocent had a better pull request. @konstin reviewed the latter one. I'm not sure what the status is. I'd love to have this done soon though!



---

_Comment by @vladNed on 2024-02-26 09:21_

I kinda have an idea to also add the checked files to the diagnostics in order to display to the user how many files where checked after a successful run.

Will make a PR and maybe you guys take a look.

---

_Comment by @konstin on 2024-02-26 09:27_

@zanieb Could you review #8631? I can solve the merge conflicts and merge after.

---

_Referenced in [astral-sh/ruff#10363](../../astral-sh/ruff/issues/10363.md) on 2024-03-12 16:57_

---

_Referenced in [astral-sh/ruff#10424](../../astral-sh/ruff/issues/10424.md) on 2024-03-15 16:29_

---

_Comment by @alexeagle on 2024-03-27 13:28_

I think that users who are trying Ruff for the first time, can't believe the speed and want to see that Ruff is actually doing something are well-advised to introduce a lint violation and see that it's reported, as a way of learning the tool.

#8631 introduced output on success. This has been discussed with other linters:

- https://github.com/golangci/golangci-lint/issues/121 - always be silent following https://www.linfo.org/rule_of_silence.html
- https://github.com/eslint/eslint/issues/3495#issuecomment-133820141 - eslint is incredibly widely adopted and the person commenting here is one of the most prolific and respected developers in the Node.js ecosystem.
- From #8853 requesting, the author observes
"On the other hand, several linters are silent if they find nothing (pycodestyle, flake8, isort, etc.)."

There are tools which interpret non-empty output to mean that a developers attention is required, and empty output to mean the linter has nothing to report. For example in CI it might report your one-line "success" message as spam boxes in the results UI, where no output would have created no box.

I think rather than add an option, ruff should go back to the old silent-on-success behavior.

---

_Comment by @zanieb on 2024-03-27 13:53_

Simply, I disagree. 

I understand the Unix philosophy and agree with it in many cases, but I don't really see this as a friendly default interface which is an emphasized goal in our tooling.

You should be able to suppress this with `-q` if it matters for your use-case. I could see automatic suppression when we can't find an interactive terminal being reasonable, but this could also be pretty confusing for people.

I'm willing to hear more discussion and feedback.

> There are tools which interpret non-empty output to mean that a developers attention is required, and empty output to mean the linter has nothing to report. 

This is the purpose of exit codes. Is there a reason that's insufficient? Deprecation warnings?


---

_Comment by @alexeagle on 2024-03-27 15:06_

I think exit codes don't express warnings well. Non zero exit code would terminate a build, which is an error semantic.

---

_Comment by @zanieb on 2024-03-27 15:19_

Yeah I think there's an open question about how to elevate warnings to users. Most users don't see any of the deprecation warnings we display in CI and are surprised when things break.

---

_Comment by @zanieb on 2024-03-27 15:32_

Note we write this to stdout and not stderr (which is where we emit things like warnings)

---

_Closed by @zanieb on 2024-04-12 20:24_

---

_Referenced in [aspect-build/rules_lint#228](../../aspect-build/rules_lint/issues/228.md) on 2024-05-09 07:42_

---

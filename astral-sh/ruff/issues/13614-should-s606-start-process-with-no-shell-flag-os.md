```yaml
number: 13614
title: "Should `S606` (`start-process-with-no-shell`) flag `os.exec*` functions?"
type: issue
state: open
author: larkost
labels:
  - rule
assignees: []
created_at: 2024-10-03T17:29:18Z
updated_at: 2024-10-07T12:37:09Z
url: https://github.com/astral-sh/ruff/issues/13614
synced_at: 2026-01-12T15:54:53Z
```

# Should `S606` (`start-process-with-no-shell`) flag `os.exec*` functions?

---

_@larkost_

The [S606 linter](https://docs.astral.sh/ruff/rules/start-process-with-no-shell/) is incorrectly including including `os.execl` and family alongside the `os.spawn` family (and others like `os.popen*` and `os.system`). The [original recommendation](https://docs.python.org/3/library/subprocess.html#replacing-the-os-spawn-family) that this linter is actually trying to fix notes that the behavior of opening a new process is better handled by the members of the `subprocess` module, and that the older methods are now really obsolete.

This is not true of the `os.exec*` family, as those do not open a new process, but rather replace the current process with the referenced command. So once this commend is executed the Python program stops, giving way to the new program as the same PID. There is no way of doing this with `subprocess` module, as those methods always start a new process.

All of the `os.exec*` functions should be removed from this linter.

Additionally, I would suggest changing the name of the linter. The change suggested has nothing at all to do with whether a shell is involved at all. In fact it is mildly harder to use a shell with the `os.spawn*`, as the `subprocess` members usually have a `shell=True` method. I would suggest something like "prefer-subprocess" or "spawn-is-obsolete", as those communicate accurately what the problem is.

List of keywords searched: "S606", "os.exec" (not just open)
Ruff version: v0.6.6, installed in Visual Studio Code through astral.sh plugin on MacOS
Ruff settings required:
```
[tool.ruff.lint]
select = ["S606"]
```

Code snippet:
```
import os

os.execlp("ls", "ls")
```

---

_Label `rule` added by @MichaReiser on 2024-10-07 07:45_

---

_Comment by @MichaReiser on 2024-10-07 07:45_

This sounds reasonable to me. @AlexWaygood wdyt?

---

_Comment by @AlexWaygood on 2024-10-07 10:35_

I think the issue here is mostly one of documentation. `S606` is a security-related rule, but the documentation for the rule makes it sound like the motivation for the rule is stylistic, and doesn't describe any of the security motivations for preferring to avoid these functions. (If the motivation for the rule were purely stylistic, then I agree that this could be a false positive, as there's functionality that `os.exec*` provides that the `subprocess` module cannot provide.)

This rule was originally provided as `B606` by the security-focussed linter bandit; Ruff's `S606` rule is a reimplementation of bandit's rule. Bandit's documentation [states](https://bandit.readthedocs.io/en/latest/plugins/b606_start_process_with_no_shell.html):

> Python possesses many mechanisms to invoke an external executable. However, doing so may present a security issue if appropriate care is not taken to sanitize any user provided or variable input.
> 
> This plugin test is part of a family of tests built to check for process spawning and warn appropriately. Specifically, this test looks for the spawning of a subprocess in a way that doesn’t use a shell. Although this is generally safe, it maybe useful for penetration testing workflows to track where external system calls are used. As such a LOW severity message is generated.

This makes it much clearer than our documentation that the primary motivation for this rule is security-related rather than simply style.

Bandit also allows you to configure exactly which `os.exec*` and `os.spawn*` functions are flagged by this rule, however, if you've decided that you want to treat one of these rules as "always safe" from a security perspective. We could possibly consider adding a similar configuration option.

> Additionally, I would suggest changing the name of the linter. The change suggested has nothing at all to do with whether a shell is involved at all. In fact it is mildly harder to use a shell with the `os.spawn*`, as the `subprocess` members usually have a `shell=True` method. I would suggest something like "prefer-subprocess" or "spawn-is-obsolete", as those communicate accurately what the problem is.

This may be true, but we also have the `S602` rule (`subprocess-popen-with-shell-equals-true`, adapted from [bandit's `B602` rule](https://bandit.readthedocs.io/en/latest/plugins/b602_subprocess_popen_with_shell_equals_true.html)) to flag possibly insecure uses of the `subprocess` module that would lead to a new shell being spawned.

In summary, I would not change the rule here. Instead, I would:
- Rewrite the docs (I'll take this on)
- Consider making it configurable

---

_Comment by @AlexWaygood on 2024-10-07 10:52_

> Additionally, I would suggest changing the name of the linter. The change suggested has nothing at all to do with whether a shell is involved at all. In fact it is mildly harder to use a shell with the `os.spawn*`, as the `subprocess` members usually have a `shell=True` method. I would suggest something like "prefer-subprocess" or "spawn-is-obsolete", as those communicate accurately what the problem is.

I think specifically the reason why this rule has `shell` in the title is because there is another rule which flags `os` functions that spawn new processes and _do_ use a shell to spawn those new processes ([`S605`: `start-process-with-a-shell`](https://docs.astral.sh/ruff/rules/start-process-with-a-shell/)). `with-no-shell` in the title of this rule distinguishes it from the title of the other rule.

Both processes that are spawned using a shell and those that don't use a shell are security risks (which is why both are reported), but the first category are a much bigger security risk. This is why it's split into two rules: one rule for the `os` functions that spawn subprocesses using shells, and one rule for the `os` functions that spawn subprocesses without shells. If you decided that the security risk of the second category is tolerable but the security risk of the first category is not, you could plausibly enable the first rule while disabling the second.

---

_Renamed from "Linter S606 incorrectly including os.exec*" to "Should `S606` (`start-process-with-no-shell`) include `os.exec*` functions?" by @AlexWaygood on 2024-10-07 10:54_

---

_Renamed from "Should `S606` (`start-process-with-no-shell`) include `os.exec*` functions?" to "Should `S606` (`start-process-with-no-shell`) flag `os.exec*` functions?" by @AlexWaygood on 2024-10-07 10:54_

---

_Comment by @AlexWaygood on 2024-10-07 12:37_

https://github.com/astral-sh/ruff/pull/13658 reworked the docs

---

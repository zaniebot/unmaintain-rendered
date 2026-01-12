```yaml
number: 943
title: "fix(flake8_boolean_trap): add whitelist for dict methods"
type: pull_request
state: merged
author: pwoolvett
labels: []
assignees: []
merged: true
base: main
head: fix/893
created_at: 2022-11-28T16:16:14Z
updated_at: 2022-11-28T22:31:35Z
url: https://github.com/astral-sh/ruff/pull/943
synced_at: 2026-01-12T15:55:05Z
```

# fix(flake8_boolean_trap): add whitelist for dict methods

---

_@pwoolvett_

This commit enables the skipping of FBT003 based on two conditions:

* function name must be explicitly whitelisted (for now, common dict methods). In the future, more could be added based on user requests.
* argument in function call must be one of the first two. The idea is to skip only things like `{}.get(False, "default")` (for boolean keys) and `{}.get(key, True)` (for boolean defaults). Other method signature with a name in the whitelist can still potentially generate FBT003.

fixes #893

---

_Comment by @pwoolvett on 2022-11-28 16:16_

https://github.com/charliermarsh/ruff/issues/893
@obi-jerome, 

---

_Comment by @andersk on 2022-11-28 18:44_

Upstream doesn’t raise FBT003 for method calls at all:

https://github.com/pwoolvett/flake8_boolean_trap/blob/7783a47196411de1ca0d0670e9acf1be88829764/src/flake8_boolean_trap.py#L189

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:30 on 2022-11-28 20:50_

Should it not be _exactly_ the second argument, of two? Why "one of the first two"?

---

_@charliermarsh reviewed on 2022-11-28 20:50_

---

_@pwoolvett reviewed on 2022-11-28 20:54_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:30 on 2022-11-28 20:54_

booleans can be used as keys, too

---

_@charliermarsh reviewed on 2022-11-28 20:55_

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:30 on 2022-11-28 20:55_

Oh right!

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:30 on 2022-11-28 20:56_

I guess, as an exception, the first argument of `fromkeys` shouldn't be boolean given that it's always a tuple. That's fine though.

---

_@charliermarsh reviewed on 2022-11-28 20:56_

---

_@andersk reviewed on 2022-11-28 21:09_

---

_Review comment by @andersk on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-28 21:09_

Consider the more inclusive term “allowlist”.

If we’re going with this method name heuristic, some other candidates that come to mind are `set.add`, `list.append`, `set.discard`, `list.count`, `list.index`, `list.insert`, `list.remove`. There could be a lot of these, even before considering methods of custom classes. I think we should maybe start with upstream’s approach of ignoring method calls entirely?

---

_Merged by @charliermarsh on 2022-11-28 21:17_

---

_Closed by @charliermarsh on 2022-11-28 21:17_

---

_@charliermarsh reviewed on 2022-11-28 21:17_

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-28 21:17_

Yeah, at least it's an independent check code so in theory can be opted out?

---

_@andersk reviewed on 2022-11-28 21:28_

---

_Review comment by @andersk on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-28 21:28_

It’s not independent from the code used for non-method function calls.

```console
$ cat test.py
def f(x):
    pass


f(True)
[].append(True)

$ flake8 test.py
test.py:5:1: FBT003 do not use boolean positional args. Hint: in `f(..)`, refactor positional arg #0 to include its argument name

$ ruff --select=FBT test.py
Found 2 error(s).
test.py:5:3: FBT003 Boolean positional value in function call
test.py:6:11: FBT003 Boolean positional value in function call


---

_@charliermarsh reviewed on 2022-11-28 21:35_

---

_Review comment by @charliermarsh on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-28 21:35_

Oh sorry, I misunderstood here. What do you think, @pwoolvett?


---

_@pwoolvett reviewed on 2022-11-28 22:31_

---

_Review comment by @pwoolvett on `src/flake8_boolean_trap/plugins.rs`:8 on 2022-11-28 22:31_

My gut tells me many people will have fbt003 disabled. For it to be useful, we should have an explicit allowlist. Its not a pain to maintain, and prs are easy to merge: add test case with false positive and name to allowlist. list will grow and after a couple of iterations it should be stable. Dev will have to pay the price of potential false negatives, but its still better than no fbt003.

* Custom classes should be addressed by the dev when a bt is raised on codebase.
* For cases such as when a library emits a bt, eventually an option in the toml could be used to extend the allow list. But do note the main issue here is the signature for those stdlib methods explicitly disallows named arguments with the `/` token. Other cases are expected to be improved by the dev when possible by using named args.
* I have nothing against splitting func (fbt003)/methods (fbt004?), keeping comments above in mind.

note: whatever is decided here will be implemented upstream eventually to keep them in sync.

If you want to debate further, maybe discussions would be a better place, but i think theyre not ensbled...

---

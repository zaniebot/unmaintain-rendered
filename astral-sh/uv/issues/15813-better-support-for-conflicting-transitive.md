```yaml
number: 15813
title: Better support for conflicting transitive dependencies
type: issue
state: closed
author: Ousret
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-09-12T14:12:49Z
updated_at: 2025-09-18T13:08:15Z
url: https://github.com/astral-sh/uv/issues/15813
synced_at: 2026-01-10T03:23:54Z
```

# Better support for conflicting transitive dependencies

---

_Issue opened by @Ousret on 2025-09-12 14:12_

### Summary

Consider the following pyproject.toml

```toml
[project]
name = "test-overshadow"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "urllib3",
    "urllib3-future",
    "pycryptodome",
    "PyCrypto",
]
```

Running `uv sync` will result in complete random results. Sometime it will be usable, sometime not.
Python packaging allow forks of existing project and competing namespaces can happen.

The solution would be to acquire a lock (either mutex or fs lock) on the site-packages root extract path.

While the given example is fairly naive, you will understand that often enough, the said dependencies can be transitives, letting no choices to the end-users.
I am not asking for a determined order, but rather, one namespace, one package in it, not a hybrid combination of them. End-users must have an escape hatch somewhere, like specifying "I prefer X on X namespace". As of right now an important aspect of packaging is completely flaky using uv.

To reproduce:

- uv run main.py
- rm -fR ./.venv
- (repeat until error, curiously don't happen enough time, I expected it to crash more often)

```python
import Crypto.Util
import Crypto.Hash
import Crypto.Protocol

import urllib3
import urllib3.util
```

```
Resolved 8 packages in 0.58ms
░░░░░░░░░░░░░░░░░░░░ [0/7] Installing wheels...                                                                                                                                                                                                                                  warning: The module `urllib3` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `urllib3-future` (v2.13.908) or `urllib3` (v2.5.0).
█████████████████░░░ [6/7] pycrypto==2.6.1                                                                                                                                                                                                                                       warning: The module `Crypto` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `pycryptodome` (v3.23.0) or `pycrypto` (v2.6.1).
Installed 7 packages in 10ms
 + h11==0.16.0
 + jh2==5.0.9
 + pycrypto==2.6.1
 + pycryptodome==3.23.0
 + qh3==1.5.4
 + urllib3==2.5.0
 + urllib3-future==2.13.908
```

> which causes an install race condition and can result in a broken module.

I know about this warning, but it seems to be more of a bug. There is a significant number of packages that do exactly that, the example shown are not isolated cases.

Regards,

### Platform

Fedora 41

### Version

uv 0.8.8

### Python version

Python 3.12.9

### Related

- https://github.com/astral-sh/uv/issues/15357
- https://github.com/astral-sh/uv/issues/15239
- https://github.com/astral-sh/uv/pull/15764 (fs locking on file, maybe consider locking top level directory instead)

---

_Label `bug` added by @Ousret on 2025-09-12 14:12_

---

_Comment by @konstin on 2025-09-12 14:58_

The escape hatch we provide is using an override with e.g. `urllib3; python_version < "0"`. This works effectively as dependency elimination. Non-hack support for this is tracked in https://github.com/astral-sh/uv/issues/4422. An even better, but even more difficult solution would be package renaming in the way that Cargo allows it.

---

_Comment by @Ousret on 2025-09-12 15:04_

Unfortunately the escape hatch does not work when the conflict is in transitive dependencies.
People migrating toward uv from pip often expect it to work without a broken namespace.

Here is an example:

```toml
[project]
name = "test-overshadow"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "niquests",
    "requests",
    "urllib3; python_version < '0'",
]
```

But I understand this is a subject uv maintainers are taking seriously.

> An even better, but even more difficult solution would be package renaming in the way that Cargo allows it.

that would be really interesting. for sure.

we should just keep in mind that people will be caught off guard with the "default" behavior. no one expect hybrid package by concurrency accident. 

---

_Comment by @konstin on 2025-09-12 15:09_

I don't mean adding a dependency, I meant adding an [override](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides) with [`tool.uv.override-dependencies`](https://docs.astral.sh/uv/reference/settings/#override-dependencies), which does apply transitively.

---

_Comment by @Ousret on 2025-09-12 15:12_

Interesting. Meantime, if someone comes with that issue we have to propose:

```toml
[project]
name = "test-overshadow"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "niquests",
    "requests",
]

[tool.uv]
override-dependencies = ["urllib3; python_version < '0'"]
```

OK. So looking forward to see improvements.

---

_Label `bug` removed by @konstin on 2025-09-12 15:13_

---

_Label `enhancement` added by @konstin on 2025-09-12 15:14_

---

_Label `needs-design` added by @konstin on 2025-09-12 15:14_

---

_Renamed from "Missing lock on parallelized concurrent packages" to "Better support for conflicting transitive dependencies" by @konstin on 2025-09-12 15:14_

---

_Comment by @notatallshaw on 2025-09-12 15:19_

FWIW, pip does not guarantee the behavior you're relying on, there is a step after resolution that determines the order to install dependencies, but it can change between pip versions (and in fact did between 25.1 and 25.2), there are no guarantees about the behavior of that order, and we may install in parallel in future versions of pip.

---

_Comment by @Ousret on 2025-09-12 15:21_

> FWIW, pip does not guarantee the behavior you're relying on

I don't expect any particular order. I don't remember writing that anywhere. Quoting myself: "I am not asking for a determined order, but rather, one namespace, one package in it, not a hybrid combination of them."

> and we may install in parallel in future versions of pip.

great, one subject to handle with that in mind then. unless a future PEP come with "namespace can't be reused in fork and pypi warehouse will be updated to reject[...]", we have a challenge to solve.

---

_Comment by @notatallshaw on 2025-09-12 15:54_

> I don't expect any particular order. I don't remember writing that anywhere. Quoting myself: "I am not asking for a determined order, but rather, one namespace, one package in it, not a hybrid combination of them."

I guess I'm not fully understanding the cause of the issue then and why it's happening with uv and not pip, is it because files from the the different packages are being written concurrently to the same directories? 

---

_Comment by @Ousret on 2025-09-14 04:53_

> is it because files from the the different packages are being written concurrently to the same directories

I am afraid that is correct. My understanding so far is that uv lock a single file/module at a time when the whole package namespace should be.

---

_Comment by @Ousret on 2025-09-17 08:37_

Already have some downstream users complaining about the default behavior of uv.

while investigating, I found that uv expose an environment variable https://docs.astral.sh/uv/reference/environment/#uv_concurrent_installs

that allows to disable concurrent unzipping, this would preferably be the default (and set concurrent unzip in preview feature/flag) meanwhile a thorough analysis is made. I suspect that users will gradually show up at my door because of this. We have no possible way to instruct uv in a typical package metadata about this.

Is there anything that can be done in a short delay? The issue at hand is reported a handful of times. As I said before, the order does not matter, the usability of one package (in given namespace) do.

regards,

---

_Comment by @konstin on 2025-09-17 10:33_

I don't think that problem is solvable as long as `urllib3-future` uses the `urllib3` module name that's already used by the widely used `urllib3` package. Python (CPython, not just packaging) does not have isolation between dependencies the way that npm or Cargo have, and there's no mechanism to declare renamings or similar replacement operations on the package, i.e. we're lacking support for this workflow on an ecosystem level.

---

_Comment by @Ousret on 2025-09-17 12:32_

I am truly lost. I speak of something and you something else entirely.

> I don't think that problem is solvable as long as urllib3-future uses the urllib3 module name that's already used by the widely used urllib3 package.

Do you deny there is thousands of projects out there that do exactly what we do? faust-cchardet, cchardet, pycrypto, pycryptodome, ...

>  Python (CPython, not just packaging) does not have isolation between dependencies the way that npm or Cargo have, and there's no mechanism to declare renamings or similar replacement operations on the package,

I just do not care about that. We already took precautions about replacing urllib3 the right way. You are not hearing what I am saying (reporting) from the start.
You are experimenting multi threaded zip extraction WITHOUT caring WHERE you write concurrently when the fact that Python packaging explicitly allows that documented behavior.

There is three solutions:

- Locking per site-package (instead of already done per file)
- Disabling multi threaded extraction by default
- Explicitly stating that UV do not support package that concurrently share a namespace (not only package but also plugins or alternative package (like -full / -chrome / -xyz)

To be truthful, four:

- We think the Python packaging standards are perfectly implemented here. At best it is a feature request.

Finally, if there is the slightest chance for us to solve the problem we're trying to solve (that ugly way) we'll surely change... But (almost) three years and we did not come to a solution.

Regards,

---

_Comment by @zanieb on 2025-09-17 13:05_

I don't understand why you think locking the directory is a solution? The install order will still be arbitrary and you could end up with either of the packages? Why do you think a deterministic ordering is not required?

> You are experimenting multi threaded zip extraction WITHOUT caring WHERE you write concurrently when the fact that Python packaging explicitly allows that documented behavior.

Please revise your tone in subsequent comments, or we'll just lock the thread. We want to help you and understand your use-case, but your comment does not meet our standards for communicating with our team.

> Explicitly stating that UV do not support package that concurrently share a namespace (not only package but also plugins or alternative package (like -full / -chrome / -xyz)

We do not support packages that intentionally overwrite modules in other packages right now. We can document this.

---

_Comment by @notatallshaw on 2025-09-17 13:13_

@Ousret as an outside of uv / astral I reread this thread and I'm still quite confused. I think the frustration comes from a lack of communicating what your actual impact is. 

You've written steps to reproduce, but not included an error or given a concrete example on how it stops your use case from working. Like what do you actually see and what do you expect to see after the install?

So we're all coming at this with our understanding of the problem and it's impacts, but if there's something you know that we don't you haven't communicated it. 

---

_Comment by @Ousret on 2025-09-17 13:42_

Ok, let's start over.

> I don't understand why you think locking the directory is a solution? The install order will still be arbitrary and you could end up with either of the packages? Why do you think a deterministic ordering is not required?

The issue I am filling right now is that "uv" is unintentionally creating hybrid namespace.

The problem is that importing the shared namespace result in random and catastrophic errors.
Locking the top level directory will automatically prevent the "hybrid" mixed packages from being created. It will be one OR the other.

- package X (with an Y counterpart)
  - file_a.py (possibly from Y fork)
  - file_b.py (possibly from X original)
  - file_c.py (possibly from Y fork)
  - ... (and so on)

 it's the lottery. 

> Why do you think a deterministic ordering is not required?

because packaging actual documentation from Python does not prefer any order and let the end users find a way to achieve a particular order. and it's perfectly fine to me. pip does not give a particular order.

> Please revise your tone in subsequent comments, or we'll just lock the thread. We want to help you and understand your use-case, but your comment does not meet our standards for communicating with our team.

did not try to be rude, was just trying to expose a situation and constantly getting pushed on another subject.

> We do not support packages that intentionally overwrite modules in other packages right now. We can document this.

This is a good starting point.

---

> I think the frustration comes from a lack of communicating what your actual impact is.

People are getting random and impossible to diagnose error coming from various transitive dependencies. Relative import are failing randomly most of the time.

> You've written steps to reproduce, but not included an error or given a concrete example on how it stops your use case from working. Like what do you actually see and what do you expect to see after the install?

Just being able to import the dependency, whether it's one or the other.

---

In summary, most people out there:

- package X
  - sub package Y
  - sub package Z
     - urllib3
   - sub package T
     - urllib3-future

I want to install package X, but the leafs dependencies are both urllib3 and urllib3-future.
"sub package Z" will fail in catastrophic and random ways, thus making "package X" unusable by accident.

`import urllib3` will fail in random ways when it should never fail.

Fork are allowed as long as we're good neighbors, meaning that a fork is a fork, must be compatible with predecessor.
Whether the namespace contain urllib3 or it's counterpart, it should work. If someone want the extra features in urllib3-future, it have to either (i) force order (ii) use the alternative namespace.

My thinking is as follow:

- Python allows package publish that reuse a namespace
- It's documented, and widely used. Not "isolated" cases.
- uv ignore that fact when unzipping multiple package whl to possible shared namespace.

so that's why I think that locking the directory is the easiest way to achieve a compromise here.

Hopefully this will clear things out.
Regards,

---

_Comment by @zanieb on 2025-09-17 14:01_

Thanks for the details.

I think where we're disconnecting is that we see picking an arbitrary package as a broken user experience too, e.g., if package T requires `urllib3-future` and we install `urllib3`, we have created an environment that does not satisfy the contract that package T has requested.

We'd rather invest in broader changes like better support for packages that are intended to replace another (e.g., see #4422). That's why we keep bringing up different solutions.

>  Python does not prefer any order and let the end users find a way to achieve a particular order. and it's perfectly fine to me. pip does not give a particular order.

When using `pip` you are performing several imperative operations, so it's more reasonable to expect the user to "figure out" the ordering. With `uv run` and `uv sync`, there's not a similar way for the user to define the ordering. Even if we lock the directories on install, repeated `uv run` operations could give you a different environment — which is still problematic.

> People are getting random and impossible to diagnose error coming from various transitive dependencies. 

We are trying to add warnings to help users understand what's going on in situations like this, e.g., see #13437

---

_Comment by @Ousret on 2025-09-17 14:08_

> I think where we're disconnecting is that we see picking an arbitrary package as a broken user experience too, e.g., if package T requires urllib3-future and we install urllib3, we have created an environment that does not satisfy the contract that package T has requested.

No, that's not it. `package T requires urllib3-future and we install urllib3` 
It's: package T requires urllib3-future and we install something that is a mix of urllib3 and urllib3-future that end up unusable sometimes.

This is why I am very confused. The reproducer gives you exceptions, sometimes. It's unpredictable. **The order is fine, I never claimed that it is an issue, not even once.**

> We'd rather invest in broader changes like better support for packages that are intended to replace another 

I understand that, but it should be a PEP that would gain approval of the wider community. And that's like in a year minimum. Does it not?

So, at least? we're in agreement that uv, while being a Python utility, choose to intentionally break a usage that worked even if imperfect?

The order is something that is interesting to **stabilize** with a Cargo like support for us, but it's not the subject I am exposing right now.

Regards,

---

_Comment by @zanieb on 2025-09-17 14:13_

> This is why I am very confused. The reproducer gives you exceptions, sometimes. It's unpredictable. The order is fine, I never claimed that it's an issue, even once.

I'm claiming the order is an issue. If we add locking as you're suggesting, then we'd be in a place where we do "package T requires urllib3-future and we install urllib3" which is nearly as problematic as "package T requires urllib3-future and we install a mix of urllib3 and urllib3-future". In both cases, your environment is broken and non-deterministic.

> So, at least? we're in agreement that uv, while being a Python utility, choose to intentionally break a usage that worked even if imperfect?

I don't think it "works" once you're not in an imperative workflow.

---

_Comment by @Ousret on 2025-09-17 14:20_

> I'm claiming the order is an issue. If we add locking as you're suggesting, then we'd be in a place where we do "package T requires urllib3-future and we install urllib3" which is nearly as problematic as "package T requires urllib3-future and we install a mix of urllib3 and urllib3-future". In both cases, your environment is broken.

Okay, so it's basically a POV.
Mine is:

- If it import, usable, then okay. Python packaging have a flaw, it's okay. If I had a choice, believe me, I would have done otherwise.. In good faith I did as best as I could with the limited time and funding I got.

Yours is:

- Whether it is usable or not, the end results is the same to me. The other issue (aka. ordering) is as bad as the current situation. Not worth pursing right now.

Okay, I understand better now.

> In both cases, your environment is broken and non-deterministic.

Python never defined forks as broken packages. We have a situation then.

At least now, I think we understood each other.
But it is sad to come to such conclusion. 

Ask yourself what's the least worst outcome? Is it the random exception? or the non-deterministic one?
Because right now, you are both non-deterministic and crashing randomly. From my point of view, not crashing is a leap forward. The warning will be more than enough afterward.

Regards,

edit: just saw https://github.com/astral-sh/uv/pull/15888 and https://github.com/astral-sh/uv/issues/13883 so the concern apply to uv itself also.

---

_Comment by @Ousret on 2025-09-18 13:08_

Closing this in favor of other issues tracking that problem as this is not a bug according to uv maintainers but rather something to explore for the future (feature request).

---

_Closed by @Ousret on 2025-09-18 13:08_

---

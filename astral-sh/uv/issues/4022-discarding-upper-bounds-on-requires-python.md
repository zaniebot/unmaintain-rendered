```yaml
number: 4022
title: "Discarding upper-bounds on `Requires-Python`"
type: issue
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
created_at: 2024-06-04T20:46:26Z
updated_at: 2026-01-05T13:44:53Z
url: https://github.com/astral-sh/uv/issues/4022
synced_at: 2026-01-10T03:11:31Z
```

# Discarding upper-bounds on `Requires-Python`

---

_Issue opened by @charliermarsh on 2024-06-04 20:46_

See: https://github.com/astral-sh/uv/pull/4021#discussion_r1626581939

---

_Comment by @charliermarsh on 2024-06-04 20:52_

It's awkward but perhaps a pragmatic decision to treat `>=3.7` as compatible with `>=3.7,<4`.

---

_Label `preview` added by @charliermarsh on 2024-06-04 20:52_

---

_Comment by @zanieb on 2024-06-04 20:56_

Thanks for opening a tracking issue.

Here's some relevant discussion:

- https://github.com/python-poetry/poetry/issues/3747
- https://github.com/pypa/packaging.python.org/pull/850
- https://github.com/python-poetry/poetry/issues/8616

Probably more out there too.

---

_Comment by @zanieb on 2024-06-04 20:57_

> It's awkward but perhaps a pragmatic decision to treat >=3.7 as compatible with >=3.7,<4.

Seems pretty reasonable until Python 4 is like... a real thing that might happen. If they switch to CalVer <4 is going to be a problem anyway :)

---

_Comment by @charliermarsh on 2024-06-04 20:59_

Haha. But if Python 4 is a thing, then _users_ could set `<4`. I was more thinking we'd throw this out on declared metadata.


---

_Comment by @hmc-cs-mdrissi on 2024-06-04 21:47_

A broader thing you could do is ignore all upper bounds for Requires-Python. https://discuss.python.org/t/requires-python-upper-limits/12663 has much longer discussion but while standards say require python works for lower bounds, for upper bounds it's less clear.

edit: The main issue is how will backtracking behave and older versions often do not have requires-python upper bound set even though it's unlikely older version actually does work better.

---

_Comment by @charliermarsh on 2024-06-04 22:13_

Henry's comment there is interesting:

> It really should have two settings, one for Requires-Python, and one for solving.

In Poetry, the published `Requires-Python` is coupled to the `Requires-Python` used for solving.


---

_Comment by @zanieb on 2024-06-04 22:14_

cc @henryiii :)

---

_Comment by @charliermarsh on 2024-06-04 22:15_

That suggests we shouldn't use `Requires-Python` but a separate setting for this hah.

---

_Comment by @henryiii on 2024-06-04 22:19_

Requires-Python can't be used for upper bounds, it doesn't work properly. The field currently is used to back-solve, and backsolving is the wrong thing to do for an upper bounds. Libraries that have added an upper bound here (numba, numpy, etc) have had to remove it as it provides a worse experience to have the solver just permanently install a really old and broken package instead of the latest broken package. :)

https://iscinumpy.dev/post/bound-version-constraints/ talks about it some, and I've proposed some solutions in https://discuss.python.org/t/requires-python-upper-limits/12663 - though it got a bit stuck because packaging.tags should have a way to compute unions on SpecifierSets to help libraries detect an upper bound reliably, and I don't think we fully settled on a preferred solution.

Only a small handful of libraries _actually_ have a true, known upper bound; libraries like numba and numpy - but those also have binaries, so `--only-binary` works instead. Most other libraries, including every single library in the world that sets `<4`, is just wildly sticking something there.

After the next Python is released, old releases may actually have a computable upper bound, but you can't go back and modify old metadata.

---

_Comment by @charliermarsh on 2024-06-04 22:27_

What would you suggest we do? We can compute unions on specifier sets.

---

_Comment by @henryiii on 2024-06-04 22:39_

For now, ignoring <4 when making a lock file is safe. IMO ignoring upper bounds when making a lock file is safe, as you can't guarantee the upper solution is in fact compatible regardless (for example, https://pypi.org/project/rembg/#data says <3.13 but it's because a dependency states that, and poetry would force them to do that. But when the dependency updates, that fixes the package depending on it, a package should state what _they_ support!), but it's not as surprising to respect it as it is to respect <4.

If you want to have a lock file range, you can default to `requires-python`, but make sure there's a way to specify it without affecting the public metadata, please!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-04 23:37_

---

_Comment by @dimbleby on 2024-06-05 20:30_

this has come up a few times in poetry, I think https://github.com/python-poetry/poetry/issues/6884 contains the most useful discussion.  fwiw I continue to hold the view that I expressed there: that it would be better to get the rules changed - presumably by an updated PEP - than to have solvers guess that folk meant something different than what they wrote.

Incidentally re
> for example, https://pypi.org/project/rembg/#data says <3.13 but it's because a dependency states that, and poetry forces them to too.

While I think it's true that poetry's choice to default `requires-python` as capped has proved somewhat infectious, this particular project is a very curious example to choose to make that point - as rembg does not use poetry.

---

_Comment by @henryiii on 2024-06-05 20:45_

Poetry _would_ force this though, and it's just the most recent thing I'd seen with the cap.

Currently, no one knowns what Python 4 will be like, other than it will be "less disruptive" than Python 3, and that it will come after 3.33. So I think it's perfectly fine to ignore "<4" as it only purely incorrect for anyone to use it now. Certainly don't make it infectious.

This Python limit is special, though, unless you can _also_ solve for the Python version, which Poetry, PDM, etc. can't do. If you can't ever fix a solve by using this, what is the point of potentially breaking working solves?

And in general, if any solver starts going back to older versions of a package with an upper cap (which they all do), they not going to arrive a better solve with an older package just because they find one without a cap.

I feel most solvers go for mathematical correctness here when practically backsolving is only useful for lower bounds, and is at best expensive (for perfect metadata) and at worst wrong (for un-editable metadata).

Do keep in mind, though, I work on a total of 0 solvers. :)

---

_Comment by @dimbleby on 2024-06-05 20:53_

> Poetry _would_ force this though, and it's just the most recent thing I'd seen with the cap.

sure, it's not that you're wrong about poetry - but you are wrong to assume that _everything_ is poetry's fault!

I agree that most solvers go for correctness, I agree that it's plausible that if the folk writing the spec had understood the consequences of that then they might have written it differently.  But perhaps we disagree about the right resolution to that:
- you're willing to just ignore the part of the rules that you don't like
- I say: if the spec is wrong then first get the spec fixed
  - if everyone agrees that the spec is wrong then that should be easy, if not then perhaps it shouldn't be done...

Maybe it would be interesting to have a solver take the other path and see what happens.

---

_Comment by @charliermarsh on 2024-06-05 20:56_

> This Python limit is special, though, unless you can also solve for the Python version, which Poetry, PDM, etc. can't do.

What does this mean @henryiii?

---

_Comment by @henryiii on 2024-06-05 21:18_

> assume that everything is poetry's fault!

Sorry, didn't mean everything is Poetry's fault! I'll settle for "almost everything". :P

Poetry and PDM solve for a range - that's _why_ this is infectious. Say you have one dependency: numpy. But this is during the few weeks where they had `>=3.9,<3.13`. Now Poetry will force you to set your Requires-Python to `>=3.9,<3.13` (or narrower), since it can't find a lockfile that will be declared to support 3.13. However, _as a python library_, that's wrong, since your library just depends on `numpy`, and when numpy updates for 3.13 (or drops this silly cap, which they did), then your _library_ now supports Python 3.13. It's only your _lockfile_ that doesn't. So yes, you need to update your lockfile, but you shouldn't have to update your metadata and make a new release of your library, because you should't have been forced to modify your library metadata in the first place.

> if the spec is wrong then first get the spec fixed

What would you suggest fixing? I have three options, I don't think we ever picked one though. And IMO it's still pretty squarely on the solvers to fix; an official statement that the upper bound is ignored or disallowed would help, of course. If I don't wan't to put a limit on my usage of Python, a solver should not _force_ me to because my dependency does. I'm fine if it forces me to acknowledge that with `tool.something`, but not my project metadata.

> What does this mean

Just that `numpy<2` is not the same as `requires-python<3.11`. You can select `numpy<2`, but you can't pick the Python you are running on (in the mentioned tools). So a limit only serves to either cause backsolving on packages to look for a higher or missing upper cap (which is wrong for an upper limit) or to cause the solve to error, which turns a non-zero chance of failure into a guaranteed failure. The NumPy cap _might_ cause a working solve by selecting an older NumPy. If a tool _does_ solve for Python, like conda does, then it is much more like a normal package cap, and could cause a working solve by lowering Python rather than backsolving for older libraries without caps.

Conda lockfiles have a pinned version of Python. Poetry, PDM, etc. have a range of Pythons.

(TL;DR: the solution here may be affected by uv's Python installation plans ;) )

---

_Comment by @zanieb on 2024-06-05 21:46_

Regarding updating `Requires-Python` to be narrower than your dependents see https://github.com/astral-sh/uv/issues/4071 which proposes only narrowing the lockfile value and #4021 where we require users to narrow the `pyproject.toml` value. I find narrowing _just_ the lockfile relatively compelling.

I'm also quite intrigued by the idea of "solving" for Python versions but haven't fully grasped what that would look like in practice.

---

_Comment by @dimbleby on 2024-06-05 21:47_

> What would you suggest fixing?

depends what exactly you think is broken!  eg I think I have seen you assert that requires-python was never intended / should not be used for upper bounds at all (apologies if this is misrepresenting you, or you now think something different).  In that case, one might propose something like: introduce `min-python`, deprecate `requires-python`.

> If I don't wan't to put a limit on my usage of Python, a solver should not force me to because my dependency does

While related, I think this is a separate question from whether or not upper bounds and consequent backsolving were a good idea.  eg per zanieb the published range and the locked range do not have to be identical, this is something that could - at least in principle - be addressed today without changes to specs.

---

_Comment by @henryiii on 2024-06-05 22:01_

Yes, I have said both, though the former is not correct, the designers assumed it could be used for anything, it just turned out that it doesn't "just work" when you design solvers. Adding a cap (remember when it was designed nothing had it, so it was always "added") just causes backsolving to before the cap existed.

`requires-python` originally supported dropping Python 3.x versions while keeping 2.7, which is why it needed to be a specifier set. Changing it to a new field would be a monumental task compared to just disallowing an upper cap. And if there was a situation like that in the future, it could also be determintal to handicap ourselves now. I have three suggestions in the linked Python discussion, but I don't think the churn of replacing the whole thing is acceptable.


---

_Comment by @henryiii on 2024-06-05 22:03_

But anyway, I am curious to see what's chosen here, and eventually will try to restart the Python discussions.

---

_Comment by @charliermarsh on 2024-06-05 23:25_

> I find narrowing just the lockfile relatively compelling.

I think there's something to this but it doesn't quite work as-is. We include your own project in the lockfile, so that sets an upper limit on the `Requires-Python` that we could infer -- like, it can't get any wider than your own project's requirement.

> But anyway, I am curious to see what's chosen here, and eventually will try to restart the Python discussions.

My guess right now is that we'll ignore upper bounds on declared `Requires-Python` from dependencies.


---

_Comment by @hmc-cs-mdrissi on 2024-06-06 00:10_

> I'm also quite intrigued by the idea of "solving" for Python versions but haven't fully grasped what that would look like in practice.

This I take to mean treat python as another dependency and install it. uv pip install python==3.11 for example. And that user could specify,


```
python>=3.10
numpy>=1.24
...
```

and then treat requires-python same as adding a dependency constraint on python itself and be able to install/upgrade/downgrade python like any other library. That's what makes conda special. Conda can install python packages, but it can also install non-python packages and even python interpreter itself.

---

_Comment by @charliermarsh on 2024-06-06 00:11_

👍 Yeah, "solving" for Python would mean you end up solving for a specific Python version (as opposed to solving for a range, I guess).

---

_Closed by @charliermarsh on 2024-06-06 17:52_

---

_Renamed from "Consider discarding `< 4` upper-bound on `Requires-Python`" to "Discarding upper-bounds on `Requires-Python`" by @konstin on 2025-09-18 09:07_

---

_Comment by @flying-sheep on 2025-09-18 09:11_

## Why I’m not happy with this issue being closed

Hmm, how do we deal with packages that set the upper bound for a good reason? The current behavior for flat-out ignoring the upper bound is problematic, see this short excerpt of the long list of issues linked above:

- #5046
- #9020
- #10916
- #12060

All of these are **legitimate `uv` resolver bugs** that are closed and redirected here.

@charliermarsh says in https://github.com/astral-sh/uv/issues/10440#issuecomment-2581677866

> Our stance is that requires-python is intended to indicate minimum Python version compatibility.

which would be fine, if there was some other way to indicate an upper bound on the supported Python versions for the packages mentioned above.

## How to go forward?

You say that this hack was installed to work around real problems, but it causes real problems as well. Can we find a less fragile solution than to just ignore a tool that a bunch of packages use correctly?

How about assuming the upper bound is monotonically increasing instead? If we have

- mydep 0.1 (no pyproject.toml, build crashes with Python >=3.9)
- mydep 1.0 (pyproject.toml, supports Python >=3.10, <3.13)
- mydep 1.1 (pyproject.toml, supports Python >=3.11, <3.14)

then trying to find a compatible version for Python 3.11 will stop backtracking beyond `mydep==1.0`, as it “knows” that 1.0 is already not compatible.

---

_Comment by @dimbleby on 2026-01-01 14:05_

Also
- #7020
- #7040
- #7830
- #7881
- #7965
- #7990
- #8863
- #9413
- #10440
- #10664
- #11981
- #12126
- #12443
- #12482
- #17280


---

_Comment by @henryiii on 2026-01-01 14:59_

Finding every issue where somebody had an incompatible version of python does not mean that they have anything to do with this issue. Many of those packages do not have an upper bound on Python requires on their current releases, because it does not work. Packages like numba and numpy had to remove it because other locking resolvers do not ignore it. Most of the time you cannot know what your upper bound will be until after you've released your package, and medatata is immutable.

Assuming monotonic bound increasing would be much better, but then it would create very strange situations where you might need to look at every single package in order to figure out the correct metadata. It would be the only case where a previous release affects a current release even if its metadata seems to match. Deleting releases could have consequences. And would be a big performance hit. All to support metadata that any major package has to remove anyway in order to work with other solvers.

---

_Comment by @dimbleby on 2026-01-01 16:25_

I believe that every single one of the issues I linked involves uv trying to install a package whose metadata has an upper bound indicating that it cannot be installed on the available python.  Feel free to point out any that I got wrong.

(Most of them are either numba 0.53.1 or llvmlite 0.36.0 - because I found them by searching for the error message which those packages give.  I assume there are other examples out there which give different errors.)

I understand what uv is trying to do by ignoring upper bounds here.  I intend that pointing out that this is not a pure win should be helpful.

---

_Comment by @henryiii on 2026-01-01 18:46_

Those packages do not have upper bounds on their current versions. Both of them used to, but removed it because it broke solvers. 

https://pypi.org/project/llvmlite/#data

https://pypi.org/project/numba/#data

If there is no upper bound, then it doesn't matter if you ignore it or not.

The only downside I know of is a potential error message improvement. It doesn't fix solves to back solve to older python versions if your Python version is fixed. If you can change the python version of demand, which I don't think uv is doing, then it might help by picking an older Python version. If the metadata is right, which it is not. At this point, we would need a new metadata value. And I don't know how you would set it, usually you don't know until later.

Conda does do this (select a Python version based on the solve), but also lets you patch metadata and patching metadata is done _a lot_.

---

_Comment by @dimbleby on 2026-01-01 19:25_

eg numba could - and probably should - reintroduce the upper bound today.  The worst backtracking that would then happen is to the current release (0.63.1), which would fail to build with the ugly error message.

But more often the accurate metadata could be used to quickly produce a helpful error message, without having to download and try to compile the package.  (Except for uv users, on account of uv ignoring that metadata)

---

_Comment by @esc on 2026-01-01 19:44_

Hi, Numba dev here,

since Numba and llvmlite seem to be causing headaches often for `uv` users, I'm trying to see if there is anything that can be done from the Numba/llvmlite side to alleviate users issues?

Take the most recent example here:

https://github.com/numba/llvmlite/issues/1385

I don't understand why `uv` is unable to find and install the most recent versions of Numba and llvmlite? Is this because `shap` specifies something that can't be installed? Can anyone shed light on what is going on here? The user is using Python 3.13 and we clearly support that and have supported it since some time? Why does `uv` end up at llvmlite 0.36.0?

---

_Comment by @dimbleby on 2026-01-01 20:18_

uv is upgrading to `numpy` 2.4.0, and then backtracking through `numba` versions until it finds one old enough that it did not put an upper cap on its `numpy` requirement.

(the easiest way to debug any case where a solver does not give you the expected answer is to add a requirement demanding the answer that you want - and then see what gives.)

per this and many linked issues, opinions vary about the desirability of upper bounds.  Possibly numba is at fault for excluding recent numpy versions - I do not know whether new numpy is often breaking for numba or not

Otherwise the easiest thing to do would be to advise your users explicitly to require a recent version of numba / llvmlite; then there can be no question of uv - or any other tool - trying to install an old version.

---

_Comment by @henryiii on 2026-01-01 20:20_

> eg numba could - and probably should - reintroduce the upper bound today ... But more often the accurate metadata could be used to quickly produce a helpful error message

No, that's not what would happen. What would happen is as soon as 3.15 shows up, then Poetry and friends would backtrack 0.63.1 and try to build it, rather than immediately trying to build the most recent version. The old backtrack would then trigger an old version of llvmlite, then users would be confused as to why old versions are being used when there are newer ones. This would slowly get even more confusing over time.

You _can't use an upper limit on python_requires_ to do anything. You can't. Most packages don't know what their upper bound is and you can't edit it after the fact. And the small subset of packages that do (things like numba with use private parts of CPython, also numpy) still run afoul of locking resolvers that take a theoretically correct approach to resolving packages, which doesn't work if the metadata has _ever_ been non-perfect.

This issue is _only_ about upper bounds on python-requires. It is not about upper bounds on anything else. numba is breaking due to upper bounds on numpy, not because of upper bounds on Python, and adding upper bounds on Python would not help that.

I believe if you put numba first then the correct resolution happens?

---

_Comment by @esc on 2026-01-01 20:25_

@dimbleby thank you, that makes total sense!

> uv is upgrading to `numpy` 2.4.0, and then backtracking through `numba` versions until it finds one old enough that it did not put an upper cap on its `numpy` requirement.

Yes, this is Numba 0.53.1 --> llvmlite 0.36.0 and NumPy 2.4 came out a few days ago.

> per this and many linked issues, opinions vary about the desirability of upper bounds. Possibly numba is at fault for excluding recent numpy versions - I do not know whether new numpy is often breaking for numba or not

NumPy breaks Numba with a majority (all?) new releases. We choose to deliberately pin to a specific NumPy version to protect our users from numerical issues and segfaults. It causes a lot of CI headaches though when new NumPy's come out, every time. We may need to find a better approach.

> Otherwise the easiest thing to do would be to advise your users explicitly to require a recent version of numba / llvmlite; then there can be no question of uv - or any other tool - trying to install an old version.

Right so in this case, if we ask users to explicitly use a specific version of Numba and llvmlite (like 0.63.1 and 0.46.0) -- `uv` will select a Numpy < 2.4.

Thanks, I'll bear that in mind for future.


---

_Comment by @dimbleby on 2026-01-01 20:26_

Backtracking to an old numba when python 3.15 shows up is no more or less confusing than backtracking to an old numba when numpy 2.4.0 shows up.  Either way, it is something that the tool can and should explain.

My experience (when python 3.14 showed up) is: I `uv add numba` and this adds the latest version (as a lower bound, no backtracking) and tries to build a project that the metadata ought to say was not going to work.

IMO reinstating the upper bound would move us in the right direction: in the short-medium term it would introduce some tolerable backtracking, in the long run old versions become irrelevant and better metadata gives better solves

---

_Comment by @henryiii on 2026-01-01 21:12_

We have to specify what resolvers should do in the standard to change this. Currently, no package should add upper bounds because resolvers are allowed to not ignore it (only uv ignores it), breaking solves. IMO, if every resolver was required to ignore it in the solve, packages could be allowed to add upper bounds, but that would be a standards change. And I still don't think that would provide better metadata (since 99% of packages don't know the actual limit - it's much more common that the limit is caused by core dependency like numpy), and you can't ever _fix_ anything unless you can select the Python version itself.

Also, anything you standardize would be misused by packages that don't actually know their limit (90%+ percent of packages support the next version of Python, normal stuff isn't broken very often), making Python upgrades artificially harder. This isn't going to excite people to work on standardizing something.

Keep in mind, upper bounds _break locking solvers_ even on versions they are not running on, since they lock for the whole range. For example, in poetry if you have `python_requires>=3.10`, it will _always_ resolve the version of numpy/numba that doesn't have an upper bound, since you don't have an upper bound. Regardless of if 3.15 or whatever even exists yet. That's why _this was tried by numba and numpy_, two packages that really do have an upper bound, and reverted shortly afterwards. Nothing has changed other than uv ignoring upper bounds which helps a little. But Poetry, for example, still doesn't.

Also, _this never gives you a better solve_. You can't solve for an older version with a higher python-requires limit.


---

_Comment by @henryiii on 2026-01-01 21:15_

For numba, if you could ship releases supporting new numpy and Python versions sooner, that would help a lot; you can't rely on upper bounds. Not sure there's much else to do, other than maybe mention that listing numba first helps uv (if it does).

---

_Comment by @henryiii on 2026-01-01 21:25_

BYW, I talk about upper bounds in detail in https://iscinumpy.dev/post/bound-version-constraints/, and proposed several options to fix requires-python in 2021: https://discuss.python.org/t/requires-python-upper-limits/12663 - that didn't go anywhere, though. I think there might be some movement this year if packaging makes it easier to detect upper bounds in a few months (https://github.com/pypa/packaging/issues/947).

---

_Comment by @dimbleby on 2026-01-01 21:50_

I am all in favour of educating folk on when upper bounds are and are not appropriate.  To my mind numba is an example with a legitimate use case: setting and supporting upper bounds would give a better user experience for those people who try to install numba in places where it will not go.

(I see that [this post](https://discuss.python.org/t/requires-python-upper-limits/12663/58) proposes that there is indeed a reasonable way to transition from no-cap to cap, which is by having an intermediate release where the build fails.  I agree; the current numba release is in effect a suitable intermediate release).

No doubt there are lots of packages setting upper bounds that they would be better not setting; but we do not need to throw the baby out with the bath water.

Re your assertion that eg poetry "will always resolve the version of numpy/numba that doesn't have an upper bound" - this is not so for a couple of reasons
- specifically poetry allows the locking-range to be specified separately from the metadata range
- more generally new projects, and updates of existing projects, provide an ever-rising lower bound on their requirements.  
  - Sooner or later the "version... that doesn't have an upper bound" is simply excluded

However I think we are straying from what is useful in a uv issue tracker, and into broader waters.  

All that I intended to do here was to note that uv has lots of bug reports from people running into this, and being confused by it.  Perhaps clearer documentation or logging or error messages would be enough; one way or another this is not yet a solved problem.

---

_Comment by @henryiii on 2026-01-02 00:59_

> uv has lots of bug reports from people running into this

The bug reports you linked were not from "Discarding upper-bounds on Requires-Python", they were from including upper bounds on packages, which is a totally different thing, and not ignored by uv. The only ones I see for this issue is a perceived error message being faster/better. Though in practice, with other packages that have always had upper bounds, the error messages I've mostly seen are identical to if you have a typo in the package name (no distribution found for X), which isn't actually helpful.


My point was, if numpy were to add upper bounds again in 2.5, then this:

```toml
[project]
requires-python = ">=3.12"
dependencies = ["numpy"]
```

Would forever resolve to numpy 2.4 on every locking solver except uv (since uv ignores the upper bounds, it is the only one that works!). Every single user making a project would have to either learn the custom poetry locking range syntax (or equivalent in whatever they are using), or they would incorrectly add an upper bound to requires-python (which would be very tempting!), or they'd have to add a marker to the dependency (also incorrect!). And it's impossible to teach the correct method without being tool specific. You don't want the easy thing to be the wrong things! Unless something gets standardized, it's incorrect to add an upper bound to this field. This field only is used to declare you no longer support a version of Python, and that's invalid for upper bounds.

If this gets standardized, there probably needs to be a "tested upper bounds" vs. "I know it it can't work" upper bounds distinction. And I don't think it makes sense to work on it before working on editable metadata, which is IMO a prerequisite, as it's important to be able to update.

---

_Comment by @flying-sheep on 2026-01-02 12:26_

> If this gets standardized

It is: “This field specifies the Python version(s) that the distribution **is compatible with**.[…] The value must be in the format specified in [Version specifiers](https://packaging.python.org/en/latest/specifications/version-specifiers/).” – [the core-metadata standard about `Requires-Python`](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-python)

It also says which specific version specifiers features are not supported (namely environment specifiers and nothing else). Again: it does *not* say that there can’t be upper bounds. So it’s pretty clear to me that the standard says that adding an upper bound means

> "I know it it can't work" upper bounds

Sure standard also says “Installation tools **may** look at this when picking which version of a project to install.”, so a tool may ignore this field.

But it’s not ambiguous about how to interpret the field when a tool looks at it at all IMO, and I can’t find any place in the standard that supports your interpretation of “This field only is used to declare you no longer support a version of Python”

---

_Comment by @henryiii on 2026-01-02 15:00_

The problem is, this is a very complex behavior, and it needs to be clearly specified as a standard. The standard was developed and implemented for one reason: packages (led by ipython, at the time) wanted to be able to drop releasing new package versions for old versions of Python without breaking those versions (2.7, specifically, IIRC). For this usage, you need the solver to include the metadata.

For the use you are asking for, you _do not_ want the solver to include this metadata. You want it to be an error message after the solve. In other words, `>=3.9,<3.12` would look for an older version if running on 3.8, but would error if running on 3.12. Those are two different behaviors. There is nothing about this in the standard, because it was developed for one use case. Both simple solvers (like pip) and locking solvers (like poetry) backtrack on upper bounds, just like lower bounds. `uv` is the only one with reasonable behavior by ignoring upper bounds, but packages are designed based on standards, you can't assume every user will use uv, so as it stands, you can't add upper bounds.

This is very, very different from packages, since you can't change the version of Python you are running on. (uv can, technically, but it doesn't). Conda can, which is why it's like a package version there.

Let's take an example without upper bounds so uv gets included; let's pretend pybind11 3.1 has a new `>=3.8,!=3.9.0` requires-python. There was a bug causing a segfault in pybind11 in Python 3.9.0 that we fixed in Python 3.9.1, so it's not far fetched. If you are running on 3.9.0, what should you do? Most users who add something like that simply don't want to fix the bug on 3.9.0, and expect an error, not a backsolve to before the package discovered the bug. What would happen is that 3.0.x would be installed instead. However, in pybind11's case, we added a workaround for 3.9.0 that causes all 3.9.x version to leak memory in order to work around this 3.9.0 bug; we would actually _want_ the backsolve in this case (assuming we dropped the workaround in 3.1). Both are possible, and there's no way for a package author to declare "won't work" vs. "backsolve" currently.

Now let's try this with a locking solver. A technically correct solution would be to never include this hypothetical pybind11 3.1 if you include a range that includes 3.9.0 (like `>=3.8` or even `>=3.9`), since the locking solver is finding a technical solution to the problem "what package works with every Python version this lockfile could be used for". But that means, instead of allowing us to no longer ship for older versions, this field is now blocking us from delivering software to everyone using a locking solver; and `uv run` secretly uses a locking solver, so this "super easy high level" runner is also blocked from getting our updates.

Both normal and locking solvers need to have a clear requirement in the standard for what to do. And I think editable metadata is likely a must, as well. The overwhelming opinion I've seen from these discussions is requires-python can't be fixed, we need a new metadata field.

I'd recommend putting effort into the monumental task of adding editable metadata instead of focusing on this for now, or one of the other tasks the WheelNext project is working on. This is hard, and solving it will just add better error messages (at best), and could make it harder to update CPython at worst, if it's misused. I'd recommend returning to it once editable metadata is solved. 

---

_Comment by @flying-sheep on 2026-01-02 15:06_

> uv is the only one with reasonable behavior by ignoring upper bounds

All the solver failures when numba/llvmlite are in the dependency tree disagree with this assessment. Uv’s choice is a tradeoff just like pip’s.

The real issue is that PyPI has no repodata patching. Conda-forge just retroactively changes all past packages with broken metadata to have correct metadata.

---

_Comment by @henryiii on 2026-01-02 16:13_

> The real issue is that PyPI has no repodata patching

That's exactly what I was referring to by "editable metadata"!

> All the solver failures when numba/llvmlite are in the dependency tree disagree with this assessment. Uv’s choice is a tradeoff just like pip’s.

numba/llvmlite failures are due to their dependency on numpy, not due to requires-python upper bounds, _because they don't have upper bounds anymore_, they have not for year(s). So pip and uv treat them identically, as **they don't have upper bounds on requires-python**.

---

_Comment by @flying-sheep on 2026-01-02 16:47_

> That's exactly what I was referring to by "editable metadata"!

yup, fully agreed here!

> they don't have upper bounds anymore, they have not for year(s).

I must be confused because numba still has it in its setup.py, but crashes on build instead of adding it to the metadata. That seems like the worst of both worlds!

---

_Comment by @esc on 2026-01-02 19:58_

> > they don't have upper bounds anymore, they have not for year(s).
> 
> I must be confused because numba still has it in its setup.py, but crashes on build instead of adding it to the metadata. That seems like the worst of both worlds!

The upper bound is checked at runtime when executing `setup.py`, not via `requires-python` metadata. The problem with `requires-python` for Numba is that if we use it and a new, unsupported Python is released, `pip install numba` will backtrack to an ancient release, the one before we started using `requires-python` in the first place. I don't remember exactly what happens then if there is a run-time check or it tries to compile from source because there isn't an appropriate wheel... Using a run-time check is the only way we can tell our users reliably that whatever unsupported Python they are using is actually unsupported. We can not patch `pypi` metadata so this is not fixable (w/o restoring to extreme measures, like deleting historic Numba releases.)

---

_Comment by @flying-sheep on 2026-01-03 11:44_

Don't get me wrong, this stuff is complicated and I'm not blaming anyone, but there are ways this could have / can be fixed better:

- If you had instead consistently set the maximum version in metadata and told people to require a minimum numba version that's greater than the last version without maximum bound, pip would work perfectly
- pip could assume that the upper bound monotonically increases

I think we have enough data now that we know the different ways this problem surfaces and can work on long-term and short-term solutions.

Fact is that packages like numba *do* have a maximum supported version, that *should* be exposed to and respected by packaging tools, we just need to figure out how.

---

_Comment by @henryiii on 2026-01-03 19:18_

* Always specifying it would be a bit better, but that still would not fix locking solvers except for uv (since uv ignores the upper bound, it's the only one that would work!). Pip also would not "work perfectly", it would claim there is no such packages as numba, last I checked it would not give you a nice message. It also has no concept of bounds, it just knowns if something matches a SpecifierSet or it doesn't.
* Assuming monotonic increase means that suddenly how one package matches depends on every other release before it, which doesn't happen anywhere else.

We had a ton of data back four years ago too. The only thing that has changed is uv ignores the upper bounds, which makes the problem a lot better, but only for uv.

> that should be exposed to and respected by packaging tools

Yes, but taking the field that solvers use to backtrack and reusing that for errors instead might not be the right solution. We might need a new field. And it's not that big of an issue; packages can simply not put upper bounds, and add a hard error in setup.py (or tool specific, like tool.scikit-build.fail), and that works fine and produces good errors and doesn't trip up locking solvers. There are only a _few_ (large) packages that have true upper bounds like numba and numpy. Yes, it _could_ be better, but there are a lot of other packaging issues that are a lot bigger that would have more impact, like variant support, default extras, etc.

---

_Comment by @flying-sheep on 2026-01-03 20:54_

Great points! I guess my issue is mostly that if a package like numba has a version that is compatible with the current Python, but a solver like uv somehow tries to build a numba version that isn’t compatible with the current Python yet, it’ll crash without need, while expressing that in metadata would result in things working.

Granted that practically, I haven’t seen that in quite a while!

> [pip] would claim there is no such packages as numba, last I checked it would not give you a nice message

Point taken about “working perfectly”, but of course correct metadata plus it being respected *could* work perfectly, unlike the build-time crash, that always will have limits!

---

_Comment by @henryiii on 2026-01-03 21:53_

An upper cap will never result in something working, it will just result in a different error. Lower caps let you backsolve to a working version, upper caps do not, because packages do not stop supporting newer pythons, they only stop supporting older pythons.

This is different than upper caps on packages, where you might be able to lower the package. Unlike conda, you cannot change the version of Python you're running from the solver. So it never fixes anything.

(I'm not disagreeing that it could be slightly better to have the crash sooner, with metadata, just that it's not something that needs to be prioritized because it doesn't fix any solves - there's no way to fix the situation if the package does not support that new version of Python yet).

---

_Comment by @flying-sheep on 2026-01-03 22:10_

I don’t think that’s right, imagine the following scenario:

numba 0.62 doesn’t support Python 3.14. numba 0.63 does.

- most people using Python 3.14 will currently see their solver try to build numba 0.63.* first and succeed. yay!
- but if something causes the solver (maybe that’s even possible with uv) to try numba 0.62 first, it’ll crash and stop, even though it *could* have succeded, had it only tried  numba 0.63 first

Maybe this can’t be an issue in practice, because all solvers only ever *back*track and never try an older version before a newer one, but there’s no guarantee that a solver acts like that.

Of course if it’s never and will never be an issue in practice, I’m just (unintentionally) being obnoxious here and making a mountain out of a molehill. Apologies in that case, I’ll stop now!

---

_Comment by @dimbleby on 2026-01-03 22:28_

or, perhaps more likely:
- `foo` version 1 has no dependency on `bar`
- `foo` version 2 requires `bar>=3`
- `bar` does not yet support the latest python

When I add to my project a dependency on `foo>=1`: I want the solver successfully to discover that `bar` is not good for me and backtrack to an earlier version of `foo` that does not require it.

---

_Comment by @dimbleby on 2026-01-03 23:20_

You can try out the above thought experiment for real with this toy:
```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.14"
dependencies = [
    "foo-ish>=0.1.0",
]

[tool.uv.sources]
foo-ish = { index = "testpypi" }

[[tool.uv.index]]
name = "testpypi"
url = "https://test.pypi.org/simple/"
explicit = true
```
where `foo-ish` is playing the part of `foo`; and its version 0.2 depends on `backports-zstd` (playing the part of `bar`).

There is a perfectly good solution available using `foo-ish 0.1` but by ignoring the upper bound declared at `backports-zstd`, uv fails to find it.  

If we translate the test-pypi stuff into poetry-speak then we get a successful solve.

---

_Comment by @henryiii on 2026-01-04 02:31_

Why doesn't `foo-ish` have `"backports-zstd>=1.3.0; python_version<"3.14"`? Its metadata is incorrect; it's correct to fail to install it on 3.14 since it has a hard dependency on a package that has the upper cap. Without that, twenty years from now this would still back solve to 0.1, which likely will be confusing. I don't think there should be a case where a solver will never pick a newer version; this is almost exactly the problem with dozens of links above - people are putting `["numpy", "numba"]`, and uv is selecting the most recent numpy, then a really, really old numba that doesn't have a limit on numpy. Nobody is happy with the result of that solve!

I'm not sure of a realistic example where this would be useful; backports should always have limits like that. If you add something (like a hypothetical working python-limited numba), do you want users to get a really old version that happened to be the last one before you added the numba dependency? Most library authors probably would be surprised by that.

For the other point, the default solvers always solve for the most recent versions that are valid.

There is a potential use that would work _much_ better with working upper caps, and that's the "oldest supported version" solver unique to uv. But since we can't edit metadata, this would only help on a small handful of packages. For now, we have to be explicit, which isn't bad. 

For example:

https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/pyproject.toml#L87-L90

```
    'setuptools >=43; python_version<"3.9"',
    'setuptools >=45; python_version=="3.9"',
    'setuptools >=49; python_version>="3.10" and python_version<"3.12"',
    'setuptools >=66.1; python_version>="3.12"',
```

(Also, you can see that often versions from years ago still work, like 66.1 still working on 3.14, even for something as complex as setuptools; after pulling distutils out in 3.12, it's not broken yet. Upgrading to new Python versions would be _much_ harder if this was common!)

(FYI, numpy is just below, too https://github.com/scikit-build/scikit-build-core/blob/302e0fde412a4545adcc81d75d6bb49ee583e81c/pyproject.toml#L106-L110)

---

_Comment by @henryiii on 2026-01-04 02:32_

(A backport is the one case where this upper cap in python-requires kind-of works, but it's because everyone using it should also put the same upper limit on it's usage, and locking solvers like poetry will generally make that known pretty quickly, as you can see).

---

_Comment by @dimbleby on 2026-01-04 08:06_

Dismissing this example by relying on specific features of `backports-zstd` is missing the point.   It's a toy example, `backports-zstd` is just standing in as an arbitrary project-with-an-upper-bound.

> Why doesn't foo-ish have "backports-zstd>=1.3.0; python_version<"3.14"?

Sure, for this particular package that's a reasonable solution.   But earlier in this thread, when thinking more generally,  you thought it was "also incorrect!".

Of course the case against upper bounds is still very much alive!  But it must acknowledge trade-offs: the assertion should be that some situations are more important than others, that some failures are worth living with.

It must not assert such absolutes as "this never gives you a better solve" "An upper cap will never result in something working".  These claims are not true.

---

_Comment by @henryiii on 2026-01-04 23:38_

Yes, I'm still not convinced there's a practical, real-world case where the resolution is _better_, but it's not absolute claim. If I was the author of `foo-ish`, I'd rather find out sooner that I made a mistake, rather than being surprised years from now when I find many of my users are stuck forever on 0.1 because 0.2+ can't be resolved on 3.14+ or if 3.14 is included in a locking solver's range. As a library author, adding a dependency shouldn't mean I can no longer ship updates to users (like how numba's numpy cap now means uv picks up an old numba if numpy comes first!).

The special case required above is that it goes back to before the dependency was added; assuming that some previous point in the development before some dependency added and assuming that is usable is not very safe, IMO; it's really only safe if users always specify lower bounds. If the dependency was always present, this would not work.

But yes, I do see there's a hypothetical case where it could affect the solve.


---

_Comment by @henryiii on 2026-01-04 23:39_

(For a lower-bound solve, it very much would affect the solve, IMO that's more common, I've never seen the above, but the lower-bound solve is something I've worked with on a handful of packages. Though again, it would rely on accurate bounds, which is really hard without editable metadata except in special cases)

---

_Comment by @quencs on 2026-01-04 23:43_

That’s fair — lower-bound solving does materially affect the outcome, and I agree it’s a much more realistic scenario than the earlier example. I’ve also seen it surface real issues in practice.

That said, as you point out, its usefulness hinges almost entirely on the accuracy of bounds, and in the absence of editable or dynamically refined metadata, those bounds are often approximations at best. Outside of fairly constrained or curated ecosystems, it’s hard to rely on them as a robust signal.

So while lower-bound solves are valuable, I tend to see them as a complementary tool rather than a substitute for earlier, explicit validation during development or CI — especially when the cost of discovering incompatibilities late is borne primarily by users.

---

_Comment by @dimbleby on 2026-01-05 00:13_

> If I was the author of foo-ish, I'd rather find out sooner that I made a mistake

The author of foo-ish did not make a mistake (unless you are still stuck on the use of `backports-zstd` in the example).  They might just as well have introduced a dependency on `numba` or whatever, when by your own account a marker would not be appropriate.

My experience from the poetry issue tracker is that pypi and python are big enough that: if you can think of a scenario - then sooner or later it will happen.

Introducing a new dependency - or picking one up transitively - does not strike me as particularly unrealistic.  Quite possibly some of the issues linked to this one arose in just such a way.

---

_Comment by @konstin on 2026-01-05 13:44_

There's another aspect to this where `requires-python` doesn't really fit: There's two kinds of Python dependencies, the language level (grammar/builtins/stdlib) and the unstable CPython ABI (https://docs.python.org/3/c-api/stable.html#unstable-c-api).

`requires-python` is a version specifier that does not include any implementation information or even flavors such as freethreaded, so it cannot express ABI dependencies. The main cases we're discussing here aren't cases where changes in Python-the-language caused an error, but the unstable C ABI. numba specifically requires a version in a range of CPython unstable C ABIs, and it fails to install on any other implementation, such as PyPy, _even if requires-python matches_:

**Passes**
```
uv venv -p python311 -c
CMAKE_ARGS="-DLLVM_DIR=/usr/lib/llvm-20/lib/cmake/llvm" uv pip install numba==0.63.1
```

**Fails**
```
uv venv -p pypy311 -c
CMAKE_ARGS="-DLLVM_DIR=/usr/lib/llvm-20/lib/cmake/llvm" uv pip install numba==0.63.1
```

In both of the above cases, we conform to the `requires-python` upper bound.

There have also been cases where patch versions were specified in requires-python to exclude a buggy `3.x.0` release, and there has been confusion about how prerelease specifiers in requires-python should be handled. Those too are CPython-specific details, which should not be encoded in the implementation-independent Core Metadata (https://discuss.python.org/t/requires-python-and-pre-release-python-versions/62959).

---

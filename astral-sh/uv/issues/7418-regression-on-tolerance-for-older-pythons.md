```yaml
number: 7418
title: Regression on tolerance for older Pythons
type: issue
state: open
author: garthk
labels:
  - help wanted
  - uv python
assignees: []
created_at: 2024-09-16T02:05:32Z
updated_at: 2024-11-29T00:02:07Z
url: https://github.com/astral-sh/uv/issues/7418
synced_at: 2026-01-12T15:59:13Z
```

# Regression on tolerance for older Pythons

---

_@garthk_

Edit: I'm sorry; this top comment was a mess. I've revised it for clarity [below](#issuecomment-2506861641).

----

Having only just recently improved uv's tolerance for Python 3.6 in #6996 merged @charliermarsh 2024-09-04, I'm dismayed to have the rug pulled out by #7266 merged @zanieb 2024-09-11.

As I've said before: while I appreciate the Python team no longer support Python 3.6, RedHat-style Linux distributions will support Python 3.6 in their`/usr/libexec/platform-python` until [releasever 8 expires in 2029](https://access.redhat.com/support/policy/updates/errata#RHEL8_Planning_Guide). 

It strikes me as off brand for Astral to provide Python programmers with excellent tooling, except for _those_ Python programmers, over there. (They're an "edge case", if you know what I mean. Cough.) If they'd personally earned a disregard bordering on spite, maybe, but not if they're helping? #6996 for example fixes the problem underlying #3371, which prompted this defensive work in `get_operating_system_and_architecture`.

I'm not asking for uv to provide a managed Python 3.6, but would appreciate a fix starting with a unit test ensuring `--python=3.6` works if the following are all true:  

- `--python-preference` isn't `only-managed`
- python3.6 is on the PATH
- [SOURCE_DATE_EPOCH](https://reproducible-builds.org/docs/source-date-epoch/) < 1877558400


---

_Comment by @notatallshaw on 2024-09-16 14:32_

Some thoughts on having worked a lot with RHEL and legacy versions of software (and just being an outside observer here, I have no connection to Astral):

My understanding is the Python distro that RedHat supports only supports installing 3rd party libraries via RedHat, e.g. `yum install python3-requests`, if you're using pip or uv to instal you won't get RedHat support, and if you don't care about RedHat support why not upgrade your Python?

RHEL often supports things long past the unpaid support for an open source product ends, e.g. Python 2 support in RHEL ended just this year. If you would like a similar support that RHEL provides, perhaps consider emailing Astral and see if you can come to a paid service arrangenent like Red Hat does with it's customers?

---

_Comment by @zanieb on 2024-09-16 14:42_

Sorry about that. https://github.com/astral-sh/uv/pull/7266 was fixing a clear oversight, in which we applied the Python version restriction on a subset of platforms. I'm fine with not erroring if we query a Python 3.6 interpreter, I'm not sure why we need that in the first place cc @konstin. The simple fix here may just be to lower that to `<3.6` since we know the interpreter query script works with 3.6.

I don't have qualms with some form of 3.6 support if we're not spending a significant amount of time maintaining it.

---

_Comment by @konstin on 2024-09-16 14:49_

That checks catches cases where machines have ancient versions of Python installed which would subsequently error in our discovery script and break discovery. If we want to allow Python 3.6, we need to have a platform check for 3.6 to ensure the script doesn't fail on those versions when evolving the discovery scripts, regressing for users that want to skip EOL versions.

---

_Comment by @charliermarsh on 2024-09-16 14:52_

My only comment here is that if we're going to offer even "informal" support for this, we need a system test for it. Otherwise, it'll keep breaking. So that would be a welcome contribution (even if it's failing for now).


---

_Comment by @zanieb on 2024-09-16 14:53_

I believe now if the query script fails we always skip the interpreter so I don't think that's as big of a deal as it was before. It's mostly about messaging around the failure of the query script, which I think is a tracing log anyway.

---

_Label `help wanted` added by @zanieb on 2024-09-16 15:41_

---

_Label `uv python` added by @zanieb on 2024-09-16 15:41_

---

_Comment by @garthk on 2024-09-18 23:25_

> My understanding is the Python distro that RedHat supports only supports installing 3rd party libraries via `yum install`

@notatallshaw Fedora et al prefer any package shared across the system to use their packaging system. It's a plain Python distribution, though. You develop your Python package like any other: it's only after you build the wheel that anything changes.

Side note: Ruff's `flake8-tidy-imports` is a great way to make sure you don't use API surface added between the version of a package available as an RPM and the version that dropped support for your Python.

```toml
[tool.ruff.lint.flake8-tidy-imports.banned-api]
"re.Match".msg = "from typing import Match"
"re.Pattern".msg = "from typing import Pattern"
"typing_extensions.NotRequired".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.OrderedDict".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.override".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.ParamSpec".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.ReadOnly".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.Required".msg = "python3-typing-extensions-3.7.4.3-2"
"typing_extensions.Self".msg = "python3-typing-extensions-3.7.4.3-2"
```

While we're developing a Python package with all those arms tied behind our backs, it's the same as you're used to. We take our deps and dev-deps from PyPi while we're developing, re-testing against the deps' RPMs before we pack our own.

If I wanted you to work, I'd certainly expect to pay for it somehow, but I don't want your work. Kinda the opposite, really. If you're aware 3.6 matters to your users and customers, you won't break it, you won't be asked to fix it, and you won't risk tarnishing your brand in an attempt to justify why breaking it was fine actually.

---

> Sorry about that. […] I'm not sure why we need that in the first place.

Thanks, @zanieb. #3371 reports a confusing error caused by the info script breaking on Python 3.6. #3398 delivers a less confusing error message by gating the version on Linux. #6996 fixes the info script so it works on Python 3.6. #7266 applies #3398's version gate to other platforms.

I should emphasise #3398 and #7266 were good calls given the information on hand at the time. I wish I had a time machine, but only so I could tell #6996 me to look wider.

---

> I don't have qualms with some form of 3.6 support if we're not spending a significant amount of time maintaining it.

Yeah, nah, any load should be borne by those of us that need it. This week is a wreck, but perhaps next week I'll file a PR to relax the gate and add that test.

---

> If we want to allow Python 3.6, we need to have a platform check for 3.6 to ensure the script doesn't fail on those versions when evolving the discovery script.

Quite true, @konstin. I think I remember seeing them in your PRs? I'll check.

---

> If we're going to offer even "informal" support for this, we need a system test for it. 

I love "informal", @charliermarsh: it's appropriate, and gentler than my "tolerate". Very on brand.

Would the test I described in my top post count as a "system test", or is there more you'd want to see in the PR?

---

_Comment by @notatallshaw on 2024-09-18 23:36_

> Kinda the opposite, really. If you're aware 3.6 matters to your users and customers, you won't break it, you won't be asked to fix it, and you won't risk tarnishing your brand in an attempt to justify why breaking it was fine actually.

Everything has a cost associated with it, adding 3.6 to the test matrix costs real money to a CI bill, and if it breaks for 3.6 only that costs real money in developer time.

At some point you have to decide when something is no longer worth supporting, most of the Python ecosystem has dropped support for Python 3.6 and 3.7, and numpy stack has dropped support for Python 3.8 and 3.9.

Astral may be happy to support 3.6 because the calculus makes sense to them, but it is a calculus, not a free ride, so as someone who supports and maintains OSS it's a bit frustrating to hear users who provide no support throw phrases around like "tarnishing your brand". 

---

_Comment by @garthk on 2024-11-23 23:07_

> users who provide no support

#6996

I dropped in to say I was going to work on a fix for this at the PyConAU 2024 sprints, and now I have to put extra effort into maintaining motivation despite people throwing phrases around like "no support' and "free ride". Dude, could you not?

Not a bad point about CI, though. While I'm not sure it's worth my time maintaining a list of things I didn't ask for—a hostile reader will either skip it or read it only to find reasons to throw another elbow—but I'll make a note here to start with these if I do:

* I'm not asking for all projects support Python 3.6
* I'm not asking for all projects to invest their own time in fixing things when they break Python 3.6
* I'm not sure adding Python 3.6 to the CI matrix for every check and balance is a good use of compute, either

---

_Comment by @garthk on 2024-11-23 23:07_

Back to matters of #7418 specifically: @charliermarsh @konstin @zanieb anything about `get_interpreter_info.py` and `interpreter.rs` I should know before I have a shot at this tomorrow?

---

_Comment by @charliermarsh on 2024-11-24 03:10_

@garthk -- Nothing specific. It looks like we added that gating in https://github.com/astral-sh/uv/pull/3398 (due to https://github.com/astral-sh/uv/issues/3371), but then it was expanded beyond Linux in https://github.com/astral-sh/uv/pull/7266 although I'm not certain why, the PR doesn't say much.

---

_Comment by @zanieb on 2024-11-26 00:24_

I believe it was expanded in https://github.com/astral-sh/uv/pull/7266 because it was not intentionally limited to a single platform (as mentioned at https://github.com/astral-sh/uv/issues/7418#issuecomment-2353124885) and I pointed this out while doing some related work, e.g., https://github.com/astral-sh/uv/pull/7264. I also don't remember the exact details though.

I don't think it should be complicated or controversial to change.

> I'm not sure adding Python 3.6 to the CI matrix for every check and balance is a good use of compute, either

We have a lot of edge-case tests that do not run the full test suite, e.g., https://github.com/astral-sh/uv/blob/77116bef26f60ca3c9acfba9e5bd16f80bf3333c/.github/workflows/ci.yml#L1284-L1308 or https://github.com/astral-sh/uv/blob/77116bef26f60ca3c9acfba9e5bd16f80bf3333c/.github/workflows/ci.yml#L698-L749 — something like that seems like a reasonable trade-off for compute time / preventing regressions.

---

_Comment by @garthk on 2024-11-28 23:53_

G'day again! I've sought and taken feedback, and would like re-state my issue for clarity by removing anything poorly chosen or regulated, un-necessary, distracting, or all of the above. I'll leave it up there for the accountability. Sorry for the confusion and delay.

---

I improved uv's tolerance for Python 3.6 in #6996 merged @charliermarsh 2024-09-04. It came undone in #7266 merged @zanieb 2024-09-11. I'd like to fix it again.

The Python team no longer support Python 3.6, but RedHat-style Linux distributions will support Python 3.6 in their`/usr/libexec/platform-python` until [releasever 8 expires in 2029](https://access.redhat.com/support/policy/updates/errata#RHEL8_Planning_Guide). SuSE 15 also has Python 3.6 as its default python3.

#6996 fixed the problem underlying #3371, which'll help. 

I'm not asking for uv to provide a managed Python 3.6—@charliermarsh's “informal support” sounds about right—but will attempt a fix including a unit test ensuring `--python=3.6` works if:

- `--python-preference` isn't `only-managed`
- python3.6 is on the PATH
- [SOURCE_DATE_EPOCH](https://reproducible-builds.org/docs/source-date-epoch/) < 1877558400

---

_Comment by @garthk on 2024-11-28 23:59_

@zanieb that'll come in handy; thanks! I've got the easy work in `get_interpreter_info.py` done; it'll be the tests that are the tricky bit. I might open the PR early so we've got the diff as a backdrop for Q&A; please do advise otherwise if getting there a failed build at a time won't match the house style.

---

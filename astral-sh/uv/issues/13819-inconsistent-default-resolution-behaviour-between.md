```yaml
number: 13819
title: "Inconsistent default resolution behaviour between pip and uv: if-necessary-or-explicit vs if-necessary"
type: issue
state: closed
author: pelson
labels:
  - question
assignees: []
created_at: 2025-06-03T14:37:43Z
updated_at: 2025-06-11T19:54:56Z
url: https://github.com/astral-sh/uv/issues/13819
synced_at: 2026-01-12T16:01:37Z
```

# Inconsistent default resolution behaviour between pip and uv: if-necessary-or-explicit vs if-necessary

---

_@pelson_

### Summary

I appreciate very much the caveat in https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility related to pre-release compatibility. I wanted to highlight a specific difference, as I believe `uv` is being overly eager to install pre-releases.

With `pip`, if you do not enable pre-releases, it will only install them if no non-pre-release version is available:

```
$ python -m pip install "scipy>=1.1,<2a0" --force-reinstall | grep "Successfully installed"
Successfully installed scipy-1.15.3
```

With `uv`, and its default `if-necessary-or-explicit` behaviour, this constraint results in a pre-release being installed:

```
$ echo "scipy>=1.1,<2a0" | uv pip compile - | grep 'scipy=='
scipy==1.16.0rc1
```

If we change the pre-release flag to ``if-necessary`, then we see the expected behaviour:

```
$ echo "scipy>=1.1,<2a0" | uv pip compile --prerelease "if-necessary" - | grep 'scipy=='
scipy==1.15.3
```


However, this now means that the pip behaviour of using pre-releases if there is no other option is not followed:


```
$ python -m pip install "scipy>=1.16rc0" --force-reinstall  | grep "Successfully installed"
Successfully installed scipy-1.16.0rc1
```


```
$ echo "scipy>=1.16rc0" | uv pip compile --prerelease "if-necessary" - | grep 'scipy=='
  × No solution found when resolving dependencies:
  ╰─▶ Because only scipy<1.16rc0 is available and you require scipy>=1.16rc0, we can conclude that your requirements are unsatisfiable.

      hint: `scipy` was requested with a pre-release marker (e.g., scipy>=1.16rc0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

There is some history of issues here, in particular I point out https://github.com/astral-sh/uv/issues/9395#issuecomment-2576040442. I raise this to provide specific examples of the difference between pip and the `if-necessary` option, and to flag the desire to align with `pip` on the particular point of determining pre-releases.

I fully agree with the issue where it is claimed that [`if-necessary` is unexpectedly named](https://github.com/astral-sh/uv/issues/12589#issuecomment-2770222517), and would advocate for having an additional option which isn't "Allow pre-release versions if all versions of a package are pre-release", but is rather "Allow pre-release versions if the only versions available after applying the constraint are pre-releases". Doing this clearly requires an additional step that is possibly not being done today in `uv` (apply the constraint before deciding if pre-releases should be used), but this would be an essential step if one is to be consistent with `pip` in the way it resolves whether pre-releases are installed.

_At the very least_, it would be good to have a flag to enable the same behaviour as pip in this regard.


### Platform

Ubuntu

### Version

uv 0.7.9

### Python version

Python 3.11

---

_Label `bug` added by @pelson on 2025-06-03 14:37_

---

_Comment by @konstin on 2025-06-04 14:04_

Instead of following pip's model, we chose to make the choice over prerelease or not explicit by requiring a prerelease specifier in a direct dependency, i.e. listing it in a file the user controls. This is an intentional choice to reduce the complexity in prerelease selection: As a user, you decide ahead of time whether you are interested in prereleases for a package or not.

When using uv, I'd recommend using `<2` over `<2a0`. If you know that you need a more recent version of e.g. scipy, you can use `>=1.16.0rc1,<2` to select that prerelease.

It is also due to how our incremental SAT solver works (PubGrub), which cannot re-enable prereleases if weren't selected once (which would break clause learning), this is something where pip's design limits their resolver choices.

---

_Label `bug` removed by @konstin on 2025-06-04 14:04_

---

_Label `question` added by @konstin on 2025-06-04 14:04_

---

_Comment by @pelson on 2025-06-10 08:05_

Thanks for the response. Having gone back to `pip` to demonstrate that `scipy>=1.1,<2a0` meant something different to `scipy>=1.1,<2`, it turns out that the behaviour changed in `pip` at some point.

Previously, `<2` would trigger the installation of `2.0a0` if `pre` was enabled, but this is no longer the case, and your suggestion works out well at this point.

In terms of the naming of `if-necessary`, it is quite unfortunate IMO, but I think there are other issues where this is covered more in-depth (https://github.com/astral-sh/uv/issues/9395#issuecomment-2498355868). As a result, I will close this.

Thanks again @konstin.



---

_Closed by @pelson on 2025-06-10 08:05_

---

_Comment by @notatallshaw on 2025-06-11 19:45_

This is an example of where pip (whose behavior is inherited from packaging) simply wasn't following the spec.

I detailed the behavior change in the 25.0 bug fix section: https://github.com/pypa/pip/blob/25.1.1/NEWS.rst#bug-fixes-3

> The inclusion of packaging 24.2 changes how pre-release specifiers with < and > behave. Including a pre-release version with these specifiers now implies accepting pre-releases (e.g., <2.0dev can include 1.0rc1). To avoid implying pre-releases, avoid specifying them (e.g., use <2.0). The exception is !=, which never implies pre-releases.

uv and pip now both correctly this standard for user specified packages, uv fails to follow this standard for dependencies and transative dependencies for the reasons @konstin outlines. 

---

_Comment by @notatallshaw on 2025-06-11 19:54_

> this is something where pip's design limits their resolver choices.

Also FYI, pip can't use pubgrub-rs because it's the design choice is to stick to pure Python.

But what you mention here about pre-releaes isn't a design choice, it's a recommendation of the standard, and it's not incompatible with a pubgrub style algorithm.

Poetry, which uses mixology, a pubgrub style algorithm, successfully follows the standards recommendations here, where uv does not.

The major limiting factor for pip adopting a pubgrub style algorithm is maintainer time. 

---

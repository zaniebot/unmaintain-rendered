```yaml
number: 11612
title: "Using `uv tool run` to run an arbitrary version of a tool using a compatible version of Python doesn't work unless you explicitly specify the python version"
type: issue
state: open
author: taranlu-houzz
labels:
  - bug
assignees: []
created_at: 2025-02-19T01:46:24Z
updated_at: 2025-02-19T20:51:58Z
url: https://github.com/astral-sh/uv/issues/11612
synced_at: 2026-01-12T16:00:41Z
```

# Using `uv tool run` to run an arbitrary version of a tool using a compatible version of Python doesn't work unless you explicitly specify the python version

---

_@taranlu-houzz_

### Summary

I am attempting to find a proper `uv` invocation that will run an arbitrary version of a tool from an internal pypi package that does not share the same name as the package, without having to specify the specific Python version to use. I also want to make sure the command will work regardless of where I call it from (e.g. in a dir with a pyproject.toml, etc.).

So far, I have tried various iterations of this:
```
uv tool run --python-preference only-managed --index 'http://<internal pypi repo>:8080' --prerelease allow --from internal.namespace.package@latest tool --version
```
This does not work, but if I explicitly supply a compatible Python version with `--python` it does. It feels like since I am asking for a specific version of the package, and I am saying that I want `uv` to only use its managed versions of Python, it should be able to figure out the version that it needs.

### Platform

macOS (Darwin 23.6.0 arm64)

### Version

uv 0.6.1 (c91ee82a8 2025-02-17)

### Python version

Python 3.13.0 (the tool uses 3.11)

---

_Label `bug` added by @taranlu-houzz on 2025-02-19 01:46_

---

_Comment by @charliermarsh on 2025-02-19 01:47_

Can you expand on what doesn't work? E.g., can you include some of the error messages you're seeing?

---

_Label `bug` removed by @charliermarsh on 2025-02-19 01:47_

---

_Label `needs-mre` added by @charliermarsh on 2025-02-19 01:47_

---

_Comment by @taranlu-houzz on 2025-02-19 01:59_

Here is what I get without the `--python` flag:

<details>
<summary>Output</summary>

```
❯ uv tool run --python-preference only-managed --index 'http://<internal pypi repo>:8080' --prerelease allow --from internal.namespace.package@latest tool --version
  × No solution found when resolving tool dependencies:
  ╰─▶ Because bpy==4.0.0 has no wheels with a matching Python ABI tag (e.g., `cp313`) and
      internal.namespace.package<=1.0.0a54 depends on bpy==4.0.0, we can conclude that
      internal.namespace.package<=1.0.0a54 cannot be used.
      And because only the following versions of internal.namespace.package are available:
          internal.namespace.package==1.0.0a52
          internal.namespace.package==1.0.0a53
          internal.namespace.package==1.0.0a54
          internal.namespace.package==1.0.0a55
          internal.namespace.package==1.0.0a56
          internal.namespace.package==1.0.0a57
          internal.namespace.package==1.0.0a58
          internal.namespace.package==1.0.0a59
          internal.namespace.package==1.0.0a60
          internal.namespace.package==1.0.0a61
          internal.namespace.package==1.0.0a62
          internal.namespace.package==1.0.0a63
          internal.namespace.package==1.0.0a64
          internal.namespace.package==1.0.0a65
          internal.namespace.package==1.0.0a66
          internal.namespace.package==1.0.0a67
          internal.namespace.package==1.0.0a68
          internal.namespace.package==1.0.0a69
          internal.namespace.package==1.0.0a70
          internal.namespace.package==1.0.0a71
          internal.namespace.package==1.0.0a72
          internal.namespace.package==1.0.0a73
          internal.namespace.package==1.0.0a74
      we can conclude that internal.namespace.package<1.0.0a55 cannot be used. (1)

      Because bpy==4.2.0 has no wheels with a matching Python ABI tag (e.g., `cp313`) and
      internal.namespace.package>=1.0.0a55,<=1.0.0a67 depends on bpy==4.2.0, we can conclude that
      internal.namespace.package>=1.0.0a55,<=1.0.0a67 cannot be used.
      And because we know from (1) that internal.namespace.package<1.0.0a55 cannot be used, we can conclude
      that internal.namespace.package<1.0.0a68 cannot be used. (2)

      Because bpy==4.3.0 has no wheels with a matching Python ABI tag (e.g., `cp313`) and
      internal.namespace.package>=1.0.0a68 depends on bpy==4.3.0, we can conclude that
      internal.namespace.package>=1.0.0a68 cannot be used.
      And because we know from (2) that internal.namespace.package<1.0.0a68 cannot be used, we can conclude
      that all versions of internal.namespace.package cannot be used.
      And because you require internal.namespace.package, we can conclude that your requirements are
      unsatisfiable.

      hint: `internal.namespace.package` was found on http://<internal pypi repo>:8080/, but
      not at the requested version (all of:
          internal.namespace.package<1.0.0a52
          internal.namespace.package>1.0.0a52,<1.0.0a53
          internal.namespace.package>1.0.0a53,<1.0.0a54
          internal.namespace.package>1.0.0a54,<1.0.0a55
          internal.namespace.package>1.0.0a55,<1.0.0a56
          internal.namespace.package>1.0.0a56,<1.0.0a57
          internal.namespace.package>1.0.0a57,<1.0.0a58
          internal.namespace.package>1.0.0a58,<1.0.0a59
          internal.namespace.package>1.0.0a59,<1.0.0a60
          internal.namespace.package>1.0.0a60,<1.0.0a61
          internal.namespace.package>1.0.0a61,<1.0.0a62
          internal.namespace.package>1.0.0a62,<1.0.0a63
          internal.namespace.package>1.0.0a63,<1.0.0a64
          internal.namespace.package>1.0.0a64,<1.0.0a65
          internal.namespace.package>1.0.0a65,<1.0.0a66
          internal.namespace.package>1.0.0a66,<1.0.0a67
          internal.namespace.package>1.0.0a67,<1.0.0a68
          internal.namespace.package>1.0.0a68,<1.0.0a69
          internal.namespace.package>1.0.0a69,<1.0.0a70
          internal.namespace.package>1.0.0a70,<1.0.0a71
          internal.namespace.package>1.0.0a71,<1.0.0a72
          internal.namespace.package>1.0.0a72,<1.0.0a73
          internal.namespace.package>1.0.0a73,<1.0.0a74
          internal.namespace.package>1.0.0a74
      ). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By default,
      uv will only consider versions that are published on the first index that contains a given package, to avoid
      dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy unsafe-best-match`
      to consider all versions from all indexes, regardless of the order in which they were defined.

      hint: `bpy` was found on http://<internal pypi repo>:8080/, but not at the requested version
      (bpy==4.0.0). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple).
      By default, uv will only consider versions that are published on the first index that contains a given
      package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were
      defined.

      hint: You require CPython 3.13 (`cp313`), but we only found wheels for `bpy` (v4.0.0) with the following
      Python ABI tag: `cp310`

```

</details>

And this is with the `--python` flag:

<details>
<summary>Output</summary>

```
❯ uv tool run --python 3.11 --python-preference only-managed --index 'http://<internal pypi repo>:8080'
--prerelease allow --from internal.namespace.package@latest tool --version
Installed 27 packages in 96ms
internal.namespace.package: 1.0.0a74 (90aeed988a9725b854637c193fce941955d37d86)
```

</details>

It also does seem to download the package each time when I would think it should cache it?

---

_Comment by @charliermarsh on 2025-02-19 02:03_

My guess is this is because you don't have a `requires-python` set on the package? I believe that we discern the correct Python version if you set a `requires-python`.

---

_Comment by @taranlu-houzz on 2025-02-19 02:09_

Hmm, I don't think so. The package does have a `requires-python`.
```toml
readme = "README.md"
requires-python = ">=3.11, <3.12"
version = "1.0.0a74"
```

---

_Comment by @charliermarsh on 2025-02-19 02:11_

We might ignore the upper-bound -- I'd have to check. Regardless, I feel like we should be able to do better here based on the wheel tag errors above.

---

_Label `needs-mre` removed by @charliermarsh on 2025-02-19 02:12_

---

_Label `bug` added by @charliermarsh on 2025-02-19 02:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-19 03:00_

---

_Comment by @my1e5 on 2025-02-19 10:56_

@charliermarsh - there are some other open issues that might be related to this:

1. https://github.com/astral-sh/uv/issues/8229
2. https://github.com/astral-sh/uv/issues/8206

There was a PR you made recently that I believe was intended to address some of this

* https://github.com/astral-sh/uv/pull/10401

But I just had another test of the MRE I posted in https://github.com/astral-sh/uv/issues/8206 and it seems like the issue is not fully solved. I made a new MRE github repo you can test - https://github.com/my1e5/uv-tool-python-version. I've also included the verbose logs of when I run `uv tool install`. I've set `requires-python` to `==3.11.*` but uv uses `3.13` during the install. Hopefully this is of some help. Thanks.

EDIT:
On closer reading of PR https://github.com/astral-sh/uv/pull/10401, it seems that this is the intended behaviour currently - try to install using the latest(?) Python version on your system (in my case I do have 3.13 installed), and only if that fails do a 'refinement' of the selected Python version? I guess my thinking was that if I put `requires-python = "==3.11.*"` then uv will surely only use 3.11 to run/install the tool. I can see the argument that if resolving/installing with a more recent Python works then just go with that, but it can feel counterintuitive if `requires-python` explicitly specifies a particular version and that isn't respected automatically. This can lead to confusing behaviour if one of your projects appears to install with your intended `requires-python` version (because one of dependencies will only resolve with 3.11 say), but your other project with the same `requires-python` installs with a more recent python version (because it happens to have dependencies that all work with 3.13 say). Of course, being completely explicit and telling your users to use `-p 3.11` in the install/run command will prevent this from being a problem. I only pose this as a potential pitfall new users can find themselves in. I appreciate it is not necessarily simple to solve! ☺️


---

_Comment by @zanieb on 2025-02-19 15:03_

It seems feasible this is a case of "we ignore upper bounds on Python during dependency resolution" so the inferred range we use when we search for a Python interpreter doesn't include it?

edit: Looking at the pull request, it seems like we even explicitly exclude the upper bound in the logic

---

_Comment by @charliermarsh on 2025-02-19 15:11_

The “refine the Python version” logic looks right to me: we find the lower bound, then use a compatible minor. So we’d look for 3.11.

---

_Comment by @zanieb on 2025-02-19 15:29_

I reproduced and this is "working as intended" since we ignore Python upper bounds this doesn't fail in the first place and we don't use the refinement logic.

I think the solution here would be to respect Python upper bounds on the first-party package, like the target tool in this case. Starting a discussion on that in https://github.com/astral-sh/uv/issues/11624

---

_Comment by @charliermarsh on 2025-02-19 17:28_

What about expanding the refinement logic to understand these kinds of failures?

---

_Comment by @charliermarsh on 2025-02-19 17:29_

Like, if they had `requires-python = ">=3.11, <3.12"`, but _did_ publish Python 3.13 wheels, then it's plausible we would _want_ to succeed with Python 3.13.

---

_Comment by @zanieb on 2025-02-19 17:56_

There isn't a failure though, so we're not in the refinement logic? edit: Note I am specifically referring to https://github.com/astral-sh/uv/issues/11612#issuecomment-2668289295 (I missed the failure mode in the toggle).

>  if they had requires-python = ">=3.11, <3.12", but did publish Python 3.13 wheels

This seems really weird — I don't understand how that would happen. But agree it's plausible that should succeed.

---

_Comment by @zanieb on 2025-02-19 17:59_

Expanding the refinement logic to handle this missing wheel case makes sense. I'm a bit confused though because the error says only `cp310` is available but the `requires-python` range is `>=3.11`? — maybe that's just a bug with our error message hint since 3.11 succeeds.

---

_Comment by @charliermarsh on 2025-02-19 20:18_

@taranlu-houzz -- What wheels _are_ available, exactly, for `bpy==4.0.0`?

---

_Comment by @taranlu-houzz on 2025-02-19 20:21_

@charliermarsh What would be the fastest way to check that? I don't recall which versions use which version of `bpy` off the top of my head unfortunately.

---

_Comment by @charliermarsh on 2025-02-19 20:34_

If you look at `http://<internal pypi repo>:8080/bpy`, the HTML should include a list of available wheels for the package.

---

_Comment by @taranlu-houzz on 2025-02-19 20:51_

Hmm, that seems to redirect to the main `pypi.org` (https://pypi.org/simple/bpy/). This is what I see:
```
Links for bpy
bpy-2.82-cp37-cp37m-macosx_10_11_x86_64.whl
bpy-2.82-cp37-cp37m-win_amd64.whl
bpy-2.82.1-cp37-cp37m-win_amd64.whl
bpy-2.82.1.tar.gz
bpy-2.91a0-cp37-cp37m-macosx_10_13_x86_64.whl
bpy-2.91a0-cp37-cp37m-manylinux2014_x86_64.whl
bpy-3.4.0-cp310-cp310-macosx_10_13_x86_64.whl
bpy-3.4.0-cp310-cp310-macosx_11_0_arm64.whl
bpy-3.4.0-cp310-cp310-manylinux_2_17_x86_64.whl
bpy-3.4.0-cp310-cp310-win_amd64.whl
bpy-3.5.0-cp310-cp310-macosx_10_15_x86_64.whl
bpy-3.5.0-cp310-cp310-macosx_11_0_arm64.whl
bpy-3.5.0-cp310-cp310-manylinux_2_28_x86_64.whl
bpy-3.5.0-cp310-cp310-win_amd64.whl
bpy-3.6.0-cp310-cp310-macosx_10_15_x86_64.whl
bpy-3.6.0-cp310-cp310-macosx_11_0_arm64.whl
bpy-3.6.0-cp310-cp310-manylinux_2_28_x86_64.whl
bpy-3.6.0-cp310-cp310-win_amd64.whl
bpy-4.0.0-cp310-cp310-macosx_10_15_x86_64.whl
bpy-4.0.0-cp310-cp310-macosx_11_0_arm64.whl
bpy-4.0.0-cp310-cp310-manylinux_2_28_x86_64.whl
bpy-4.0.0-cp310-cp310-win_amd64.whl
bpy-4.1.0-cp311-cp311-macosx_11_0_arm64.whl
bpy-4.1.0-cp311-cp311-macosx_11_0_x86_64.whl
bpy-4.1.0-cp311-cp311-manylinux_2_28_x86_64.whl
bpy-4.1.0-cp311-cp311-win_amd64.whl
bpy-4.2.0-cp311-cp311-macosx_11_0_arm64.whl
bpy-4.2.0-cp311-cp311-macosx_11_0_x86_64.whl
bpy-4.2.0-cp311-cp311-manylinux_2_28_x86_64.whl
bpy-4.2.0-cp311-cp311-win_amd64.whl
bpy-4.3.0-cp311-cp311-macosx_11_0_arm64.whl
bpy-4.3.0-cp311-cp311-macosx_11_0_x86_64.whl
bpy-4.3.0-cp311-cp311-manylinux_2_28_x86_64.whl
bpy-4.3.0-cp311-cp311-win_amd64.whl
```
I guess that is because the local pypi repo is setup to fall back to the regular pypi.

---

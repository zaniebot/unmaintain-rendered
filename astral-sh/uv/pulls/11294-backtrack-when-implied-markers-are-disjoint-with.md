```yaml
number: 11294
title: Backtrack when implied markers are disjoint with target platform
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/skip-implied
created_at: 2025-02-06T19:59:37Z
updated_at: 2025-05-07T20:33:31Z
url: https://github.com/astral-sh/uv/pull/11294
synced_at: 2026-01-10T11:10:35Z
```

# Backtrack when implied markers are disjoint with target platform

---

_Pull request opened by @charliermarsh on 2025-02-06 19:59_

## Summary

I'm a little surprised that we weren't doing this already, but if we look at a wheel-only distribution, and it doesn't have _any_ wheels for the current fork environment, we shouldn't accept it.

For example, given:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.13.0"
dependencies = ["PyQt5-Qt5"]

[tool.uv]
environments = ["sys_platform == 'win32'"]
```

We should be backtracking to `5.15.2`, which is the last version with Windows support.


---

_Comment by @charliermarsh on 2025-02-06 20:01_

Ugh, I guess this change is somewhat problematic since it looks like the `transformers` resolver now fails due to `jaxlib` lacking distributions for ARM Linux:

```
  × No solution found when resolving dependencies for split (python_full_version >= '3.12' and platform_machine == 'aarch64' and sys_platform == 'linux'):
  ╰─▶ Because only the following versions of jaxlib are available:
          jaxlib==0.4.6
          jaxlib==0.4.7
          jaxlib==0.4.9
          jaxlib==0.4.10
          jaxlib==0.4.11
          jaxlib==0.4.12
          jaxlib>=0.4.13
      and jaxlib<=0.4.13 has no wheels with a matching platform tag, we can conclude that jaxlib<=0.4.13 cannot be used.
      And because transformers[all] depends on jaxlib<=0.4.13 and your project requires transformers[all], we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @paveldikov on 2025-02-28 19:36_

Just saw this and thought it related to #11841? :-)

> Ugh, I guess this change is somewhat problematic since it looks like the `transformers` resolver now fails due to `jaxlib` lacking distributions for ARM Linux

And of course missing a source distribution (same as with `PyQt5` and friends). So, really, the `jaxlib` is not truly portable, therefore test failure is valid?

---

_Comment by @charliermarsh on 2025-02-28 19:39_

@paveldikov -- Yeah exactly. It's actually correct to fail. I'm just worried about passing that on to users as a default since in practice they may not care. It's tricky...

---

_Comment by @notatallshaw on 2025-02-28 19:53_

What about in the error telling the user how to restrict to known good platforms when a required library has no source distribution? Does depend on users reading error messages 🙃.

---

_Comment by @paveldikov on 2025-02-28 19:53_

Some options for handling this semi-gracefully:
* Allow user to exclude don't-care platforms from consideration (#11463?). Then a friendly error message can prompt the user to do so in case of partial unsatisfiability.
* Fallback to emitting almost-universal lockfiles/constraints files, that are actively self-aware of their own unsatisfiability for given environments.


---

_Closed by @charliermarsh on 2025-05-04 17:02_

---

_Comment by @paveldikov on 2025-05-07 13:22_

Was this closed purely because of the `transformers` library's pin to a really old version of `jaxlib`? I think it's a shame because this would really unblock a number of issues that we commonly face in our environment. Certainly `PyQt5` is one such problem-child package that currently requires kid-glove treatment :-(

To avoid the robbing-Peter-to-pay-Paul effect, perhaps this could have been gated behind a setting? Something along the lines of `universal-require-compatible-dist`, or something more succinctly worded?

---

_Comment by @konstin on 2025-05-07 13:24_

Have you seen the required-environments feature we shipped (https://docs.astral.sh/uv/concepts/resolution/#required-environments)? If it doesn't work for you, could you describe what it's missing?

---

_Comment by @paveldikov on 2025-05-07 14:29_

That would almost work apart from two caveats:
* `required-environments` applies as a post-hoc filter, and therefore applies too late if one of the platform wheels is being blocked by a security scanner. See #11463
* `PyQt5` legitimately supports Windows + Linux (and I do want my lockfiles to support both). It just happens that the latest several versions -- for some inscrutable reason :-( -- do not ship Windows wheels OR sdists. Therefore the latest versions (which the current algorithm will happily pick) result in lockfiles that are not usable on Windows i.e. are not actually universal.

---

_Comment by @konstin on 2025-05-07 18:37_

I could get pyqt5 to work with `required-environment` with the following `pyproject.toml`. I see the problem with security scanners, uv currently expects access all files in an index (or at least the `data-dist-info-metadata` file, which doesn't require having access to the actual wheel).

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "pyqt5",
]

[tool.uv]
required-environments = [
  "sys_platform == 'win32'",
  "sys_platform == 'linux'"
]
```

This setup makes the lockfile dispatch between Windows and Linux:

```toml
[[package]]
name = "pyqt5-qt5"
version = "5.15.2"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "sys_platform == 'win32'",
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/1c/7e/ce7c66a541a105fa98b41d6405fe84940564695e29fc7dccf6d9e8c5f898/PyQt5_Qt5-5.15.2-py3-none-win32.whl", hash = "sha256:9cc7a768b1921f4b982ebc00a318ccb38578e44e45316c7a4a850e953e1dd327", size = 43447358, upload-time = "2021-03-10T14:01:17.827Z" },
    { url = "https://files.pythonhosted.org/packages/37/97/5d3b222b924fa2ed4c2488925155cd0b03fd5d09ee1cfcf7c553c11c9f66/PyQt5_Qt5-5.15.2-py3-none-win_amd64.whl", hash = "sha256:750b78e4dba6bdf1607febedc08738e318ea09e9b10aea9ff0d73073f11f6962", size = 50075158, upload-time = "2021-03-10T14:05:20.868Z" },
]

[[package]]
name = "pyqt5-qt5"
version = "5.15.16"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "sys_platform != 'win32'",
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/2f/97/ef77663fb5d61b65f2e93edc135cc0b86724c1fc610c10a6867ae0c0baff/PyQt5_Qt5-5.15.16-1-py3-none-manylinux2014_x86_64.whl", hash = "sha256:2cfa8f50dd29618ef98f29355f83d8a5f3e41003be22128e9b5d94d214b6b468", size = 61080044, upload-time = "2025-02-24T22:30:07.132Z" },
    { url = "https://files.pythonhosted.org/packages/ff/89/65a4b9c6bc89012182ff46bf8e503a8aa5e5570df7cb628211c93012beca/PyQt5_Qt5-5.15.16-py3-none-macosx_10_13_x86_64.whl", hash = "sha256:18b6fec012de60921fcb131cf2a21368171dc29050d43e4b81a64be407a36105", size = 39957063, upload-time = "2024-12-18T15:42:29.63Z" },
    { url = "https://files.pythonhosted.org/packages/0a/60/b6db285c87666b02aa11d0946d7bb381d8bdcc815cc5aa61fa91272b321b/PyQt5_Qt5-5.15.16-py3-none-macosx_11_0_arm64.whl", hash = "sha256:e1a0e7ae35a7615c74a293705204579650930486a89af23082462f429dae504a", size = 37122434, upload-time = "2024-12-18T15:42:53.006Z" },
    { url = "https://files.pythonhosted.org/packages/dc/1a/c4f861c4d7a9844a207b8bc3aa9dd84c51f823784d405469cde83d736cf1/PyQt5_Qt5-5.15.16-py3-none-manylinux2014_x86_64.whl", hash = "sha256:5ee1754a6460849cba76c0f0c490c0ccc3b514abc780b141cf772db22b76b54b", size = 59854452, upload-time = "2024-12-18T15:43:30.262Z" },
]
```

---

_Comment by @paveldikov on 2025-05-07 20:33_

> I could get pyqt5 to work with required-environment with the following pyproject.toml.

I can confirm that this works with `uv.lock`, but not for `uv pip compile` (either txt or PEP 751 output format)


> uv currently expects access all files in an index (or at least the data-dist-info-metadata file, which doesn't require having access to the actual wheel).

To add insult to injury, certain PyPI-like repo backends do not support range queries, so a full wheel download it must be. (besides, even if range requests were to be supported, I wouldn't be surprised if the gating logic were so coarse-grained so as to make it 403 anyway)


---

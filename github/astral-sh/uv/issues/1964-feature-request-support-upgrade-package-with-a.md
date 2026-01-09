---
number: 1964
title: "[Feature Request] Support `--upgrade-package` with a specific version"
type: issue
state: closed
author: hongqn
labels:
  - compatibility
  - cli
assignees: []
created_at: 2024-02-25T12:25:38Z
updated_at: 2024-07-10T03:09:14Z
url: https://github.com/astral-sh/uv/issues/1964
synced_at: 2026-01-07T13:12:16-06:00
---

# [Feature Request] Support `--upgrade-package` with a specific version

---

_Issue opened by @hongqn on 2024-02-25 12:25_

`pip-compile --upgrade-package` allows you to upgrade a specific package to a specific version instead of the latest version. For example, you can use the following command to update the `django` package to the latest version and `requests` to version `2.0.0`:

```
pip-compile --upgrade-package django --upgrade-package requests==2.0.0
```

Currently, when using `uv pip compile --upgrade-package requests==2.0.0`, it produces the following error:

```
❯ uv --version
uv 0.1.10
❯ uv pip compile --upgrade-package requests==2.0.0
error: invalid value 'requests==2.0.0' for '--upgrade-package <UPGRADE_PACKAGE>': Not a valid package or extra name: "requests==2.0.0". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.

For more information, try '--help'.
```

Additionally, `pip-compile` also supports the following form:

```
pip-compile --upgrade --upgrade-package 'requests<3.0'
```

It would be great if `uv` supports this feature as well.

---

_Label `compatibility` added by @zanieb on 2024-02-25 16:00_

---

_Label `cli` added by @zanieb on 2024-02-25 16:00_

---

_Comment by @zanieb on 2024-02-25 16:02_

Seems reasonable, thanks for the clear request :)

---

_Comment by @charliermarsh on 2024-02-25 16:27_

I don’t think I fully understand what the semantics of this are, especially for the variant that’s a bound rather than a specific version. What happens if the resolution can’t succeed using the given range / version? Is it ignored? (I’m hesitant to support the variant that takes a bound, it just adds a lot of complexity. Should that not be a constraint provided via a constraint file?)

---

_Comment by @zanieb on 2024-02-25 16:29_

I was also wondering if constraints could / should be used for this purpose. It does seem weird to provide arbitrary bounds on a single invocation.

---

_Comment by @hongqn on 2024-02-29 07:04_

In my scenario, this need often arises due to security reasons: the dependency package has discovered security vulnerabilities and needs to be upgraded, but we hope to stay as close as possible to the version already deployed in production in order to avoid introducing faults.

---

_Comment by @charliermarsh on 2024-02-29 16:40_

But, should / could that not be provided as a constraint file?

---

_Comment by @Redoubts on 2024-02-29 21:07_

should it? for me I tend to do something like 
`pip-compile --output-file out.txt in.txt -P package~=X.Y`

Where I treat the existing out.txt as the current state of the venv. When I run a command like this, I'm kinda asking a question:
* what package if any fits this constraint, given my current venv?

sometimes the answer is yes, with no issues (and write a new out.txt)
sometimes the answer is no, unpin other packages (and fail to write a new out.txt)
and maybe it's "irredeemable constraints" (and fail to write a new out.txt)

Usually my in.txt doesn't usually have version pins (that's what out.txt is for), unless I know for certain new versions would break something. Are you suggesting alternative to using bounded `-P` args is to temporarily add bounds to in.txt?

---

_Comment by @ewianda on 2024-05-13 12:09_

Any update on this one?

---

_Comment by @charliermarsh on 2024-05-13 14:18_

No updates right now.

---

_Comment by @mariokostelac on 2024-05-29 12:34_

What is the expected workflow for upgrading a package?

I'm in the process of migrating pip-tools to uv workflows. Previously, I'd use `pip-compile requirements.in --upgrade-package boto3==x -o requirements.txt`, which would take requirements.txt (as constriants) into account and make sure that boto3 is upgraded. If it can't be, it would upgrade necessary transient packages to make boto3 upgradeable to the version I had.

Right now, I use `uv pip compile -c requirements.txt requirements.in -P boto3 -o requirements.txt`, but it does nothing (there is newer version). If I remove boto3 manually from requirements.txt, it also does nothing (probably because of some transitive deps).

---

_Referenced in [astral-sh/uv#3990](../../astral-sh/uv/issues/3990.md) on 2024-06-03 16:43_

---

_Comment by @charliermarsh on 2024-06-03 20:23_

Conceptually, is this the same as `--upgrade-package boto3` with `boto3==x` as a constraint? I'm trying to understand the semantics of `--upgrade-package boto3==x`.

---

_Comment by @mark-thm on 2024-06-03 20:48_

Not quite, -U allows all requirements in the output to be updated but -P specifically operates with the new constraint _and_ updates only the minimal set of requirements in the output to satisfy that additional constraint. If the new constraint can’t be satisfied with existing input constraints, pip compile errors. 

---

_Comment by @charliermarsh on 2024-06-03 20:49_

Thanks! Though I'm wondering specifically about the use of `-P` with a version specifier.

---

_Comment by @mark-thm on 2024-06-03 23:16_

Yep, so expectation of -P foo==1.2.3 is:
- apply an additional constraint of “foo==1.2.3”
- allow only output requirements necessary to meet this constraint to upgrade
- still meet all input constraints 

---

_Referenced in [astral-sh/uv#2512](../../astral-sh/uv/issues/2512.md) on 2024-07-03 19:29_

---

_Comment by @avilaton on 2024-07-03 19:31_

Hi, just driving by to mention that this shows up when we try to have dependabot run updates using `uv` instead of `pip-compile` so a potential big impact from having it work exactly the same as `pip-compile` 
https://github.com/astral-sh/uv/issues/1964

---

_Comment by @avilaton on 2024-07-03 19:34_

Dependabot will try to do exactly this
```
uv pip compile --build-isolation --output-file=requirements.txt \
    --no-emit-index-url \
    --no-annotate \
    --no-header \
    -P attrs==18.0.1 pyproject.toml
```
and `uv` complains with
```
error: invalid value 'attrs==13.0.1' for '--upgrade-package <UPGRADE_PACKAGE>': Not a valid package or extra name: "attrs==13.0.1". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

---

_Comment by @charliermarsh on 2024-07-03 20:19_

That’s helpful to know, thanks. We should probably support it then.

---

_Comment by @avilaton on 2024-07-09 12:30_

It is my first time dearing to even look at a rust codebase but I would like to help. If you have a sample PR in which similar work has occurred, a few pointers or any guidance you think would help me find a way to send a PR for this, I'll take a shot at it. The carbon footprint of dependabot using pip-tools makes this work any effort.

---

_Comment by @charliermarsh on 2024-07-09 21:23_

My initial thinking is something like: change `Upgrade` in `package_options.rs` to `Packages(FxHashSet<Requirement>)` rather than `Packages(FxHashSet<PackageName>)`. Then, at some point, iterate over those requirements and add them to constraints, then proceed as usual.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-10 02:33_

---

_Referenced in [astral-sh/uv#4952](../../astral-sh/uv/pulls/4952.md) on 2024-07-10 02:44_

---

_Closed by @charliermarsh on 2024-07-10 03:09_

---

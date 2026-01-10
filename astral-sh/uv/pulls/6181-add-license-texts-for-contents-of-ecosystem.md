```yaml
number: 6181
title: Add license texts for contents of ecosystem/
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: ecosystem-licenses
created_at: 2024-08-18T13:23:14Z
updated_at: 2024-09-22T04:41:33Z
url: https://github.com/astral-sh/uv/pull/6181
synced_at: 2026-01-10T12:53:32Z
```

# Add license texts for contents of ecosystem/

---

_Pull request opened by @musicinmybrain on 2024-08-18 13:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
While the contents of `ecosystem/` are “merely” `pyproject.toml` files and one trivial Python script, they are still covered by the licenses of the projects from which they are copied. Not only is maintaining license/copyright statements good practice, but it’s generally specifically required by the particular licenses involved here.

Even though these files are for integration testing only – and therefore do not contribute to the license of the compiled `uv` executable – they are nevertheless part of the source archive, so distributors and integrators need to consider their license status. For example, I maintain the `uv` package in Fedora Linux, and I need to consider these licenses because the files would be redistributed in the source RPMs.

## Test Plan

<!-- How was it tested? -->

N/A – validated by examination of the diff.

---

_Comment by @musicinmybrain on 2024-08-18 13:31_

I am not sure what to do about `prettier` complaining about `ecosystem/home-assistant-core/LICENSE.md`. The license is copied from `home-assistant-core` upstream, so it should not be modified or reformatted. Maybe I should rename it to `ecosystem/home-assistant-core/LICENSE`, dropping the somewhat dubious `.md` extension? That seems safe enough.

---

_Comment by @zanieb on 2024-08-18 13:37_

There's an ignore file at: https://github.com/astral-sh/uv/blob/main/.prettierignore

---

_Comment by @zanieb on 2024-08-18 13:37_

Thanks for taking the time to do this. I'll have @BurntSushi review since he added the tests :)

---

_Assigned to @BurntSushi by @zanieb on 2024-08-18 13:37_

---

_Label `internal` added by @zanieb on 2024-08-18 13:37_

---

_Comment by @musicinmybrain on 2024-08-18 13:41_

> There's an ignore file at: https://github.com/astral-sh/uv/blob/main/.prettierignore

Good suggestion – thanks! I added `ecosystem/home-assistant-core/LICENSE.md` to `.prettierignore`.

---

_Comment by @musicinmybrain on 2024-08-18 13:46_

(I submitted the `pretix` license – the `AGPL-3.0-only` portion of which includes a custom exception – [for review by Fedora Legal](https://gitlab.com/fedora/legal/fedora-license-data/-/issues/559). I’ll follow up in a separate issue if it’s found to be problematic.)

---

_Comment by @BurntSushi on 2024-08-19 12:01_

> For example, I maintain the uv package in Fedora Linux, and I need to consider these licenses because the files would be redistributed in the source RPMs.

Can you why you're including them in the source RPMs? Why not exclude them?

---

_Comment by @musicinmybrain on 2024-08-19 12:53_

> Can you why you're including them in the source RPMs? Why not exclude them?

The source RPM includes the `.spec` file that controls how the package is built, the original upstream source archive, and any patches or ancillary files. The original source archive is, e.g., https://github.com/astral-sh/uv/archive/0.2.37/uv-0.2.37.tar.gz (plus three more archives corresponding to the `async_zip`, `pubgrub`, and `reqwest_middleware` forks referenced by git hash in `Cargo.toml`), and so anything that’s in the git repository is in the source RPM. Note that we do run all of the tests that don’t require network access.

If I wanted to avoid distributing the `ecosystem/` directory, I would need to use a script to download the source archive from GitHub, remove `ecosystem/`, and then repack the source archive. Then, I would need to make sure I skipped running the tests that rely on those files; fortunately, those seem to be contained to `crates/uv/tests/ecosystem.rs`. All of this is perfectly feasible, and I maintain other packages in Fedora where this approach is necessary, but it seems to be a bit of a heavyweight workaround for missing license texts. Alternatively, I *might* be able to switch to using the PyPI sdist, if it isn’t missing anything else I need.

While I doubt anyone is going to get upset about copying a `pyproject.toml` file in practice, in principle, maintaining the correct license texts for files copied from other projects and redistributed via GitHub would seem to be something that should matter to the `uv` project too, regardless of what downstream distribution packagers are doing.

---

_Comment by @BurntSushi on 2024-08-19 13:07_

I think any tests involving things in the `ecosystem` directory are going to involve some kind of network I/O. At least, all extant tests do.

> While I doubt anyone is going to get upset about copying a pyproject.toml file in practice, in principle, maintaining the correct license texts for files copied from other projects and redistributed via GitHub would seem to be something that should matter to the uv project too, regardless of what downstream distribution packagers are doing.

I'm not a lawyer, so I'm not going to try to get into legal stuff here, but my mind is more headed in the direction of whether it's worth putting these tests in a different repository. Like, you really should not need to be getting AGPL license exemptions from lawyers in order to package `uv`. I don't want that to be a hurdle that folks feel like they need to cross, and if putting these tests in this repository provokes that hurdle, it might be something to rethink.

I suppose this is orthogonal to putting the license files in the repo. Since the legal aspect of this doesn't really change if we put them in another repo.

---

_Comment by @BurntSushi on 2024-08-19 13:09_

@charliermarsh I think we should merge this. If problems arise from it, then we can consider moving the tests to a different repository, although I'd really rather not do that. What do you think?

---

_Comment by @musicinmybrain on 2024-08-19 13:16_

I think your analysis in https://github.com/astral-sh/uv/pull/6181#issuecomment-2296538198 is very reasonable.

---

_Comment by @musicinmybrain on 2024-08-23 15:51_

> If I wanted to avoid distributing the `ecosystem/` directory, I would need to use a script to download the source archive from GitHub, remove `ecosystem/`, and then repack the source archive. Then, I would need to make sure I skipped running the tests that rely on those files; fortunately, those seem to be contained to `crates/uv/tests/ecosystem.rs`. All of this is perfectly feasible, and I maintain other packages in Fedora where this approach is necessary, but it seems to be a bit of a heavyweight workaround for missing license texts. […]

This – combined with applying this PR as a patch – is the approach I’m using to move forward for now, although I hope it won’t be permanent – either because the `pretix` license is found to be acceptable for Fedora, or because the contents of `ecosystem/` end up being moved out of this repository.

---

_@zanieb approved on 2024-08-23 16:28_

---

_Merged by @zanieb on 2024-08-23 16:28_

---

_Closed by @zanieb on 2024-08-23 16:28_

---

_Comment by @musicinmybrain on 2024-09-22 04:41_

> (I submitted the `pretix` license – the `AGPL-3.0-only` portion of which includes a custom exception – [for review by Fedora Legal](https://gitlab.com/fedora/legal/fedora-license-data/-/issues/559). I’ll follow up in a separate issue if it’s found to be problematic.)

Just following up to say that the replacement of `pretix` with `saleor` in https://github.com/astral-sh/uv/pull/7567 due to https://github.com/astral-sh/uv/issues/7566 is really helpful, as now everything in `ecosystem/` has standard licenses.

---

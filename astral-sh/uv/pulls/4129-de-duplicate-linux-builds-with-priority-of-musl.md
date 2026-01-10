```yaml
number: 4129
title: De-duplicate Linux builds with priority of MUSL targets
type: pull_request
state: closed
author: T-256
labels: []
assignees: []
base: main
head: linux-musl
created_at: 2024-06-07T12:26:59Z
updated_at: 2024-06-19T14:51:43Z
url: https://github.com/astral-sh/uv/pull/4129
synced_at: 2026-01-10T13:54:02Z
```

# De-duplicate Linux builds with priority of MUSL targets

---

_Pull request opened by @T-256 on 2024-06-07 12:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Ref https://github.com/astral-sh/uv/issues/4122#issuecomment-2154280414 musllinux targets are superset of gnu targets.

Should we document it that why we use musl builds?

## Test Plan
https://github.com/T-256/uv/actions/runs/9416773652/job/25940621883

---

_Comment by @ofek on 2024-06-07 16:03_

I think it would be nice to document!

---

_@henryiii reviewed on 2024-06-08 05:30_

---

_Review comment by @henryiii on `README.md`:509 on 2024-06-08 05:30_

```suggestion
The ABI used for Linux platform is MUSL with statically linked C-Runtime, except for _PPC64_ and _s390x_ archs which are only available with dynamically linked GNU ABI.
```

---

_Comment by @henryiii on 2024-06-08 05:31_

I think the wheels also need to be dual tagged. You can tell maturin to do this. `--compatibility 2_17` IIRC.

---

_Comment by @T-256 on 2024-06-08 11:36_

Thanks @henryiii for your review.

> I think the wheels also need to be dual tagged. You can tell maturin to do this. `--compatibility 2_17` IIRC.

Can you open a new PR for that? I think it's out of scope of this PR.

---

_Comment by @charliermarsh on 2024-06-08 16:12_

I’m undecided on whether I want to do this. But we should either (1) do this and omit all the non-musl release targets (this would now be considered a breaking change), (2) do this but copy the musl to the non-musl targets (sort of strange IMO), or (3) re-add the GNU dynamically linked build (but keep the dual-tagged wheel).


---

_Comment by @charliermarsh on 2024-06-08 16:19_

Are there examples of other similar tools that only ship against musl?

---

_Comment by @ofek on 2024-06-08 16:57_

ripgrep does that but only for x86_64 https://github.com/BurntSushi/ripgrep/releases/tag/14.1.0

---

_Comment by @BurntSushi on 2024-06-08 17:31_

For ripgrep, I don't really have a consistent philosophy here. I'm pretty reactive and add things that people request (or submit), and will generally remove artifacts if they become too burdensome to maintain. With that said, if I were to clean things up, I'd probably bias toward using musl for everything where I could, and not offer anything that is dynamically linked to glibc. I don't think I've ever offered a glibc release artifact for Linux x86-64, and I don't think I've ever heard anyone complain about it. Using musl is just more convenient and doesn't really confer any downsides for ripgrep specifically. (Its allocator tends to be slower than glibc, but I mitigate that by using a different allocator. I think we do the same for `uv`.)

---

_Comment by @charliermarsh on 2024-06-08 20:08_

Ok, I think for now, my preference is:

- Keep these builds.
- Re-add the dynamically-linked GNU variant to the release pipeline.
- ...but dual-tag our wheels, and upload the dual-tagged wheels.


---

_Comment by @T-256 on 2024-06-08 20:37_

Also see https://www.tweag.io/blog/2023-08-10-rust-static-link-with-mimalloc/

> (Its allocator tends to be slower than glibc, but I mitigate that by using a different allocator. I think we do the same for `uv`.)

Correct, according to blog post with mimalloc it'd be even faster than linux-gnu target. We currently use jemalloc.

---

_Comment by @charliermarsh on 2024-06-08 20:49_

Yeah I'm ultimately a bit torn but it doesn't really cost us much to continue shipping the dynamically-linked versions, so I have a slight preference towards doing so.

---

_Comment by @T-256 on 2024-06-12 01:01_

According to https://github.com/astral-sh/uv/pull/4254, this is now breaking change, 
@charliermarsh feel free to close it since you agreed continuation with both static and dynamic libc releases in the pipeline.


---

_Closed by @zanieb on 2024-06-19 14:51_

---

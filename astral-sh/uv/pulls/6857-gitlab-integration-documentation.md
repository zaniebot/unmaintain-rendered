```yaml
number: 6857
title: GitLab Integration documentation
type: pull_request
state: merged
author: jvacek
labels:
  - documentation
assignees: []
merged: true
base: main
head: dockertag-docs
created_at: 2024-08-30T09:53:57Z
updated_at: 2024-10-01T18:34:50Z
url: https://github.com/astral-sh/uv/pull/6857
synced_at: 2026-01-12T16:07:33Z
```

# GitLab Integration documentation

---

_@jvacek_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Documentation for GitLab integration, reliant on the new tags introduced in https://github.com/astral-sh/uv/pull/6053

## Test Plan

<!-- How was it tested? -->


---

_Label `documentation` added by @zanieb on 2024-08-30 12:18_

---

_Assigned to @zanieb by @zanieb on 2024-08-30 12:18_

---

_Comment by @zanieb on 2024-08-30 12:18_

Thanks! I'll be on vacation until Tuesday but will review this then. Exciting to see it.

---

_Marked ready for review by @jvacek on 2024-09-04 11:53_

---

_Comment by @jvacek on 2024-09-04 11:56_

Compared to the GitHub guide, I've removed stuff that I believe is irrelevant to a GitLab deployment. Features like Matrices are well documented in GitLab docs, and using `uv` inside of a container should be pretty evident from the first example.

I did leave in the caching, as that seemed like an obvious "default" thing to have implemented.

I also had to add the md file to the ignore of prettier, as it kept trying to re-format my yaml codeblocks, which would not work if someone wanted to copy-paste them.

---

_Comment by @paatre on 2024-09-04 20:55_

Great to see this being documented.

That said, I tried your documented example in my GitLab CI configuration:

```yaml
variables:
  UV_VERSION: 0.4
  PYTHON_VERSION: 3.12
  BASE_LAYER: alpine

stages:
  - foo

bar:
  stage: foo
  image: 
    name: ghcr.io/astral-sh/uv:$UV_VERSION-python$PYTHON_VERSION-$BASE_LAYER
  script: 
    - run main.py
```

But the job still fails on the `sh` not being recognized by the container:

```
Using docker image sha256:f92e1aa098053d32da4d6ff0a0bc60ca74454153fac4b72ac501f95370e3f4a4 for ghcr.io/astral-sh/uv:0.4-python3.12-alpine with digest ghcr.io/astral-sh/uv@sha256:71a178e0a86e3cd35f55e22a854ec862ceecdb36e9d6664c90e6e9df7f714965 ...
error: unrecognized subcommand 'sh'
Usage: uv [OPTIONS] <COMMAND>
For more information, try '--help'.
```

Is there something I'm doing wrong or what?

---

_Comment by @zanieb on 2024-09-04 20:59_

The container entrypoint is set to `uv` xref #7030 

Looks like it's trying to run `uv sh`.

---

_Comment by @paatre on 2024-09-04 21:12_

> The container entrypoint is set to `uv` xref #7030
> 
> Looks like it's trying to run `uv sh`.

Yeah, that was it. I continued to read the docs and there's two occations regarding the shell:

1. Image requirements: https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#image-requirements
2. Explanation of how the runner starts with Docker containers: https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#override-the-entrypoint-of-an-image.

It's pretty obvious now that `uv` is the default `ENTRYPOINT` for the container and `sh` is the default `CMD` by GitLab. So the `script` is given to `sh` and it's being passed to `uv`, and that leads to problems.

I just need to override the entrypoint as you've also described in #7030. I kind of understand both sides of the coin:
- I want to have consistent experience on all images
- I want to use images easily without extra container configurations on CI

So yeah, which one is the main driver here is a good question.

---

_Comment by @paatre on 2024-09-04 21:33_

Lol, why didn't I think of this: https://github.com/astral-sh/uv/issues/7030#issuecomment-2329443719

That would have been an even better solution, at least in my very simple use case. The downside with using `CMD` with the default `uv` entrypoint is that the `script` doesn't work if you need or want it in the pipeline.

---

_Comment by @samypr100 on 2024-09-05 00:00_

@paatre 0.4.6 (once released) will have the behavior you were looking for

---

_Review comment by @davidfritzsche on `docs/guides/integration/gitlab.md`:1 on 2024-09-05 21:03_

```suggestion
# Using uv in GitLab CI/CD
```

---

_@davidfritzsche reviewed on 2024-09-05 21:03_

---

_@jvacek reviewed on 2024-09-10 09:11_

---

_Review comment by @jvacek on `docs/guides/integration/gitlab.md`:1 on 2024-09-10 09:11_

😅 thanks!

---

_Comment by @jvacek on 2024-09-13 09:43_

Re-tested with 0.4.9, did I miss any to-dos?

---

_@BaconPancakes reviewed on 2024-09-16 20:53_

---

_Review comment by @BaconPancakes on `docs/guides/integration/gitlab.md`:33 on 2024-09-16 20:53_

are you missing a "cache" key here?

---

_Review comment by @jvacek on `docs/guides/integration/gitlab.md`:33 on 2024-09-23 12:17_

Fixed, thanks!

---

_@jvacek reviewed on 2024-09-23 12:17_

---

_@zanieb approved on 2024-10-01 18:25_

Thanks! Sorry about the delay here.

---

_Merged by @zanieb on 2024-10-01 18:34_

---

_Closed by @zanieb on 2024-10-01 18:34_

---

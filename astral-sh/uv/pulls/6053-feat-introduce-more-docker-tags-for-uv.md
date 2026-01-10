```yaml
number: 6053
title: "feat: introduce more docker tags for uv"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: more-docker-images
created_at: 2024-08-13T05:29:52Z
updated_at: 2024-09-04T22:20:55Z
url: https://github.com/astral-sh/uv/pull/6053
synced_at: 2026-01-10T12:53:32Z
```

# feat: introduce more docker tags for uv

---

_Pull request opened by @samypr100 on 2024-08-13 05:29_

## Summary

Closes https://github.com/astral-sh/uv/issues/5610

This PR introduces additional images with the uv/uvx binaries from scratch for both amd64/arm64 and make the mapping easy to configure by generating the Dockerfile on the fly. This approach focuses on minimizing CI time by taking advantage of dedicating a worker per mapping (20-30s~ per job).

This PR also fixes `org.opencontainers.image.version` for all tags (including the one from `scratch`) to contain the right release version instead of branch name `main` (default when no tag patterns are specified).

For example, on release `x.y.z`, this will publish the following image tags with format `ghcr.io/astral-sh/uv:{tag}` with manifests for both amd64/arm64. This also include `x.y` tags for each respective additional tag.

* From **scratch**: `latest`, `x.y.z`, `x.y` (currently being published)
* From **alpine:3.20**: `alpine`, `alpine3.20`, `x.y.z-alpine`, `x.y.z-alpine3.20`
* From **debian:bookworm-slim**: `debian-slim`, `bookworm-slim`, `x.y.z-debian-slim`, `x.y.z-bookworm-slim`
* From **buildpack-deps:bookworm**: `debian`, `bookworm`, `x.y.z-debian`, `x.y.z-bookworm`
* From **python:3.12-alpine**: `python3.12-alpine`, `x.y.z-python3.12-alpine`
* From **python:3.11-alpine**: `python3.11-alpine`, `x.y.z-python3.11-alpine`
* From **python:3.10-alpine**: `python3.10-alpine`, `x.y.z-python3.10-alpine`
* From **python:3.9-alpine**: `python3.9-alpine`, `x.y.z-python3.9-alpine`
* From **python:3.8-alpine**: `python3.8-alpine`, `x.y.z-python3.8-alpine`
* From **python:3.12-bookworm**: `python3.12-bookworm`, `x.y.z-python3.12-bookworm`
* From **python:3.11-bookworm**: `python3.11-bookworm`, `x.y.z-python3.11-bookworm`
* From **python:3.10-bookworm**: `python3.10-bookworm`, `x.y.z-python3.10-bookworm`
* From **python:3.9-bookworm**: `python3.9-bookworm`, `x.y.z-python3.9-bookworm`
* From **python:3.8-bookworm**: `python3.8-bookworm`, `x.y.z-python3.8-bookworm`
* From **python:3.12-slim-bookworm**: `python3.12-bookworm-slim`, `x.y.z-python3.12-bookworm-slim`
* From **python:3.11-slim-bookworm**: `python3.11-bookworm-slim`, `x.y.z-python3.11-bookworm-slim`
* From **python:3.10-slim-bookworm**: `python3.10-bookworm-slim`, `x.y.z-python3.10-bookworm-slim`
* From **python:3.9-slim-bookworm**: `python3.9-bookworm-slim`, `x.y.z-python3.9-bookworm-slim`
* From **python:3.8-slim-bookworm**: `python3.8-bookworm-slim`, `x.y.z-python3.8-bookworm-slim`


---

_Comment by @samypr100 on 2024-08-13 12:23_

Below is what [https://github.com/astral-sh/uv/pkgs](https://github.com/astral-sh/uv/pkgs/container/uv/versions?filters%5Bversion_type%5D=tagged) would end up looking like with the additional tags (just for 0.2.35).


<img width="560" alt="uv-tags" src="https://github.com/user-attachments/assets/2765eed0-6783-48a0-bb20-b31e8918de5c">



---

_Marked ready for review by @samypr100 on 2024-08-13 12:24_

---

_Comment by @zanieb on 2024-08-13 13:47_

Epic! Thank you! I'll review this one.

---

_Assigned to @zanieb by @zanieb on 2024-08-13 13:47_

---

_@zanieb approved on 2024-08-13 16:53_

This looks good to me. I might play with the job names so they're more readable? but otherwise it seems great. I like the approach you took.

---

_Comment by @zanieb on 2024-08-13 17:06_

I would like to hear a bit more about what motivating adding quite so many tags, I was imagining a less :)

---

_Label `release` added by @zanieb on 2024-08-13 17:09_

---

_Comment by @samypr100 on 2024-08-13 20:46_

> This looks good to me. I might play with the job names so they're more readable? but otherwise it seems great. I like the approach you took.

Happy to rename the jobs, lmk

---

_Comment by @samypr100 on 2024-08-13 20:48_

> I would like to hear a bit more about what motivating adding quite so many tags, I was imagining a less :)

I primarily mimicked supported patterns from docker hub official images for [python](https://hub.docker.com/_/python) and general use (non-bundled python ones) and the tags they support. I considered the feedback / use cases from @konstin and yours on https://github.com/astral-sh/uv/issues/5610#issuecomment-2260503147 as well. I thought this was a good starting point.

---

_Comment by @zanieb on 2024-08-16 01:35_

Cool, thanks for explaining. Just waiting for consensus from more of the team here.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-16 01:35_

---

_Comment by @jvacek on 2024-08-28 09:14_

@samypr100 do you have these pushed anywhere by any chance? Would love to test them out, I could write some docs on how to implem these for GitLab

---

_Comment by @samypr100 on 2024-08-28 13:03_

> @samypr100 do you have these pushed anywhere by any chance? Would love to test them out, I could write some docs on how to implem these for GitLab

I've published the ones I've built in https://github.com/samypr100/uv/pkgs/container/uv, but do not use these for anything official as I might delete them eventually.

That being said, it would very helpful if you can confirm they resolve your problem in Gitlab.

---

_Comment by @zanieb on 2024-08-28 13:40_

@samypr100 does this need a rebase after the other changes?

I might merge this for the release today. Nobody else on the team is expressing opinions...

---

_Comment by @samypr100 on 2024-08-28 13:52_

> @samypr100 does this need a rebase after the other changes?
> 
> I might merge this for the release today. Nobody else on the team is expressing opinions...

@zanieb It does not need a rebase, unless we also want to support tags without `patch` version (e.g. 0.3) on the additional extra images, That can be done as a follow up though.

---

_Comment by @samypr100 on 2024-08-29 02:36_

> > @samypr100 does this need a rebase after the other changes?
> > I might merge this for the release today. Nobody else on the team is expressing opinions...
> 
> @zanieb It does not need a rebase, unless we also want to support tags without `patch` version (e.g. 0.3) on the additional extra images, That can be done as a follow up though.

@zanieb The latest version now also handles `major.minor` tags as well. I've also added the details of the new tags to the new docs section introduced in #6768.

---

_Comment by @jvacek on 2024-08-29 08:51_

@samypr100 Will test, thanks!

Also, any chance you could introduce the same changes to ruff?

---

_Comment by @samypr100 on 2024-08-29 17:46_

> @samypr100 Will test, thanks!
> 
> Also, any chance you could introduce the same changes to ruff?

I can take a look at supporting after this is merged, although I don't think the python-based images make sense over there so it will likely be just distroless, alpine, and debian.

---

_Comment by @jvacek on 2024-08-30 09:25_

I've got success on GitLab, amazing! Thanks @samypr100 

Here's my config:
```
.uv_compile:
  stage: analysis
  image:
    name: ghcr.io/samypr100/uv:0.4-python3.12-alpine
  script: >
    cd $CODE_LOCATION

    if [ -z "$FILES" ]; then
      FILES=$(find . -iname requirements*.in -maxdepth 1)
    fi

    for file in $FILES; do
      output_file=${file%.in}.txt
      uv pip compile $file --extra-index-url $EXTRA_INDEX_URL --no-strip-extras --output-file $output_file
    done

    git diff HEAD --exit-code
```

Side note: I have to call to `git diff` to complete my use-case for checking if the requirements are compiled,  https://github.com/astral-sh/uv/issues/3021 is relevant here in my opinion.

---

_@zanieb approved on 2024-09-03 13:43_

---

_Merged by @zanieb on 2024-09-03 13:44_

---

_Closed by @zanieb on 2024-09-03 13:44_

---

_Branch deleted on 2024-09-03 14:23_

---

_Comment by @samypr100 on 2024-09-04 01:52_

It worked 🥳, starting with 0.4.4 new extra docker tags have been published.

---

_Comment by @zanieb on 2024-09-04 02:48_

Haha oops I meant to do that release so I could watch it.. glad it worked! Thank you!

---

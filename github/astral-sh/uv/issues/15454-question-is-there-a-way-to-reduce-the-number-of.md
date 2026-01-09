---
number: 15454
title: "Question: Is there a way to reduce the number of http requests uv makes if everything is already in cache?"
type: issue
state: open
author: henryborchers
labels:
  - question
assignees: []
created_at: 2025-08-22T14:23:59Z
updated_at: 2025-09-10T14:22:44Z
url: https://github.com/astral-sh/uv/issues/15454
synced_at: 2026-01-07T13:12:19-06:00
---

# Question: Is there a way to reduce the number of http requests uv makes if everything is already in cache?

---

_Issue opened by @henryborchers on 2025-08-22 14:23_

### Question

I'm sorry if there is a better place to ask this question. 

My questions:

Is there any way to reduce the number of HTTP requests that uv makes (GET and HEAD) when you already have a resolved dependency solution and they are already in the cache? 

I know about the "--offline" flag but that will only work if the packages are already downloaded in the cache. What I really want is for uv to check offline first and only if it can't do it offline, go online and download everything. Is there anything that does that? 

Alternatively, is there anything I can set in my pyproject.toml or with environment variable to let uv know that the dependency resolution is already good and reduce the number of calls to the PyPI server to verify the solution? Just install the solution with the packages already in the cache?


Here is why I'm interested:

I'm running a lot of tests on various environments for my Python packages on my CI. Across multiple OS, Linux, MacOS, Windows. Multiple arches x86_64, ARM64. And all supported versions of Python, 3.9-3.13.  That's a lot of permutations. Uv is fast enough to make this sane. Great job!

However, Nexus now limits the number of http requests for the free tier to 200,000 per day. Which sounds like a lot but I'm hitting that almost every single day when I run my nightly CI tests.

I use Docker containers for testing everything on CI except for MacOS. When I used Docker as a test environment, I mount Docker volumes to cache $UV_CACHE_DIR across runs. This works great for reducing the number of GET requests my tests have to make to my Nexus PyPi proxy and internal package index. However, even if these test environments have the packages in the local cache, UV still makes a lot of requests to my Nexus instance. I assume to make sure that it has the latest version of the packages and that the solution is still valid.

I declare my dependencies loosely in my pyproject.toml files in the project.dependencies section and I create a frozen requirements-dev.txt file (created with uv pip compile with the "--universal" and the "--python=lowest version of python supported) flag) with version numbers pinned which I both install into a virtual environment in CI and use as constraint when I'm running tests in tox.  

I've tried using UV_CONSTRAINT to my requirements-dev.txt file to limit the versions to ones already in the cache but it seems like UV still wants to update the package metadata and sends a bunch of HEAD requests. Are there any tricks to reduce this? 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @henryborchers on 2025-08-22 14:24_

---

_Comment by @notatallshaw on 2025-08-22 14:34_

I think uv could offer at least two user customizations here if their maintainers are interested:

* Disable prefetching metadata
* Allow the user to specify that the local HTTP cache should not be expired by max-age

I have also had use cases where I would like uv to make less network requests, due to having unreliable network environment, so the less network requests the less chance of an error.

My current solution has been to do all my uv steps in the docker build step, and make sure those steps are cached by docker itself and therefore uv rarely runs in my CI.

---

_Comment by @charliermarsh on 2025-08-22 14:37_

The second is, in some sense, already supported, because you can define your own HTTP headers: https://docs.astral.sh/uv/concepts/indexes/#customizing-cache-control-headers. So you could tell uv to cache the Simple API responses from your registry for much longer.

---

_Comment by @charliermarsh on 2025-08-22 14:38_

But adding some way to "allow stale" could also be helpful / complementary.

---

_Comment by @notatallshaw on 2025-08-22 14:41_

> The second is, in some sense, already supported, because you can define your own HTTP headers: https://docs.astral.sh/uv/concepts/indexes/#customizing-cache-control-headers.

I missed that feature was added, very cool!

I definitely think OP should check the impact of setting`files = "max-age=365000000, immutable"` if their internal repo files are immutable (which in my experience is the default setup for internal artifact repositories).

---

_Comment by @zanieb on 2025-08-22 16:23_

This is similar to https://github.com/astral-sh/uv/issues/10380

---

_Comment by @henryborchers on 2025-08-25 16:34_

When I set cache-control with a custom max-age, where is the cache age information stored?

---

_Comment by @henryborchers on 2025-08-25 16:44_

Is the age of api cache determined by the creation date of the .rkyv file?

---

_Comment by @charliermarsh on 2025-08-25 17:16_

> When I set cache-control with a custom max-age, where is the cache age information stored?

IIRC it's sorted in the `.http` file that we store alongside the response.

---

_Comment by @henryborchers on 2025-08-25 18:00_

I tried running my project in a docker container with:
* Mount a docker volume to hold all in '/tmp'. 
* Set UV_CACHE_DIR to '/tmp/uvcache' in my docker images
* Set /etc/uv/uv.toml to have cache-control = { api = "max-age=6000", files = "max-age=365000000, immutable" } on the image. 
* For good measure, set UV_CONFIG_FILE to /etc/uv/uv.toml

Theoretically, this should, with each time I run the container:
* store all cache files in /tmp/uvcache.
* uv should not have to make external http requests with subsequent runs as long as it's within 100 minutes. Even if I close the container and start up a new one.

I tried this and while /tmp/uvcache gets filled with cached files, it seems to be ignored when I try it again. 

I feel like I'm missing some important concept.

---

_Comment by @henryborchers on 2025-09-10 14:22_

If I run `uv sync` with the "`--frozen`" flag, will uv try to download/update the package metadata if it's already downloaded?

---

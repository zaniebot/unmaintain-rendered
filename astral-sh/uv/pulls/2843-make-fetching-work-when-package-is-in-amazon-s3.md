```yaml
number: 2843
title: Make fetching work when package is in Amazon S3
type: pull_request
state: closed
author: torarvid
labels: []
assignees: []
base: main
head: amazon-s3-fetching
created_at: 2024-04-05T22:21:05Z
updated_at: 2024-05-08T14:54:11Z
url: https://github.com/astral-sh/uv/pull/2843
synced_at: 2026-01-10T14:37:54Z
```

# Make fetching work when package is in Amazon S3

---

_Pull request opened by @torarvid on 2024-04-05 22:21_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When a package is hosted on Amazon S3 (sometimes the case when using Gemfury as a private repository), there might be a redirect from the Gemfury link to the S3 link. The S3 link will be different for HEAD and GET requests. For this reason, we need to use the original Gemfury link when passing arguments to the range reader.

Fixes #2025 

## Test Plan

Just tested locally where I had a repro of the issue.

---

_Assigned to @zanieb by @zanieb on 2024-04-06 03:45_

---

_Comment by @zanieb on 2024-04-06 15:15_

I'll have to think about this some more, we've very explicitly used the _response_ URL over the _request_ URL in the past. I worry this could break other user's workflows without further consideration. 

cc @baszalmstra perhaps your team would find this an interesting [async_http_range_reader](https://github.com/prefix-dev/async_http_range_reader) edge case.

Do you know how `pip` handles this case? I guess they're just not doing `HEAD` requests.

---

_Comment by @charliermarsh on 2024-04-06 15:23_

`pip` does do a `HEAD` request: https://github.com/pypa/pip/blob/7c49d06ea4be4635561f16a524e3842817d1169a/src/pip/_internal/network/lazy_wheel.py#L52. But I think you need to pass `--use-feature=fast-deps` to enable range requests.

---

_Comment by @charliermarsh on 2024-04-06 15:24_

Oh, but they definitely _don't_ use the response URL from the `HEAD` when making subsequent `GET` requests. So that part might be wrong?

---

_Comment by @charliermarsh on 2024-04-06 15:25_

My read is that we should consider passing the URL explicitly to `async_http_range_reader`, and not relying on the response URL in those places. We typically need to use the response URL when (e.g.) the response returns relative paths and there was a redirect. But this is a bit different: it should be the same URL. (I might be totally misunderstanding the issue.)

---

_Comment by @baszalmstra on 2024-04-06 16:00_

Thanks for the ping. I think there is a case for both options. I guess if a server reports a different redirect url based on the http method this could be problematic.

I would be happy accept a pr in async_http_range_reader whatever you decide. 

---

_Comment by @torarvid on 2024-04-06 20:23_

@charliermarsh @zanieb Just in case you didn't see it, I wrote about what led me to this PR in the comments of #2025. To sum up: I don't think this is the one-and-only right way to fix this, it's merely a proof of concept that seemed to get me past the problem 😊

Having said that, I thought the api of that range reader was a little bit strange. To me, the part where you give it the headers of the response so that it can determine whether the server supports range requests seems perfectly natural. But it's less clear to me that the URL used to make those range requests need to be the url in the response (and not the url in the original request). I guess I'm somewhat biased since I've made this PR that hacks around the fact that this url can't be overridden 😊 

---

_Comment by @charliermarsh on 2024-04-06 20:28_

Yeah, I’m fairly confident we should change it to reuse the original URL. We just need to verify that it won’t regress a few other cases where we explicitly _need_ to use a response URL. (For example, if a registry returns relative paths, those need to be relative to the response URL in the event of a redirect. But that’s a different case than the range requests we’re doing here.)

---

_Comment by @zanieb on 2024-04-08 21:08_

Started the upstream changes at https://github.com/prefix-dev/async_http_range_reader/pull/11


---

_Comment by @charliermarsh on 2024-05-08 14:45_

Superseded by https://github.com/astral-sh/uv/pull/3460.

---

_Closed by @charliermarsh on 2024-05-08 14:54_

---

_Closed by @charliermarsh on 2024-05-08 14:54_

---

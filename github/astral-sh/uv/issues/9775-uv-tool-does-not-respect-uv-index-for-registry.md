---
number: 9775
title: "`uv tool` does not respect `UV_INDEX_*` for registry authentication when pulling dependencies"
type: issue
state: open
author: ot-goegelem
labels:
  - needs-decision
assignees: []
created_at: 2024-12-10T14:55:18Z
updated_at: 2025-08-14T15:41:42Z
url: https://github.com/astral-sh/uv/issues/9775
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv tool` does not respect `UV_INDEX_*` for registry authentication when pulling dependencies

---

_Issue opened by @ot-goegelem on 2024-12-10 14:55_

- uv version: `uv 0.5.6`
- operating system: `Debian GNU/Linux 11 (bullseye)`

I have a project where I developed a cli tool and I wanted to install my local package (without pushing it to a remote registry before) 

Because I have some private dependencies from a private gitlab registry I added a new index to the `pyproject.toml`
This is pushed to a repository I did not want to put the credentials in the URL.
Instead I set them via the environment variables:
`export UV_INDEX_PRIVATE_GITLAB_USERNAME=__token__`
`export UV_INDEX_PRIVATE_GITLAB_PASSWORD=<gitlab_token>`

_pyproject.toml_
``` toml
...
dependencies = [
	...
	"private-dep==version",
        ...
]

[tool.uv.sources]
private-dep = { index = "private-gitlab" }

[[tool.uv.index]]
name = "private-gitlab"
url = "https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple"
default = true
...
```

In order to install the local cli tool I used this command:
`uv tool run --from . <tool_name>` 


I don't want to post the full log of the command, but in the log (with verbose enabled and RUST_LOG=trace) you can see the following:
```
...
uv_client::cached_client::revalidation_request url="https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/"
            0.039609s   0ms TRACE uv_auth::middleware Handling request for https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/
            0.039635s   0ms TRACE uv_auth::middleware Request for https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/ is unauthenticated, checking cache
            0.039826s   0ms TRACE uv_auth::cache No credentials in cache for URL https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/
            0.039925s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/
            0.040096s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", gitlab.com)
            0.040292s   0ms DEBUG reqwest::connect starting new connection: https://gitlab.com/
            0.040930s   1ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("gitlab.com"), port=None
...
0.547996s TRACE h2::proto::streams::streams drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 2, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(65535), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097122), available: Window(2097122) }, in_flight_recv_data: 30, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: Some(Indices { head: 1, tail: 2 }) }, is_recv: true, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Remaining(0) }
    0.548040s TRACE h2::proto::streams::counts transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
            0.548129s 508ms TRACE uv_auth::middleware Request for https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/ failed with 401 Unauthorized, checking for credentials
            0.548163s 508ms TRACE uv_auth::cache No credentials in cache for realm https://gitlab.com
            0.548220s 508ms DEBUG uv_auth::middleware No netrc file found
          0.548266s 519ms TRACE h2::proto::streams::streams drop_stream_ref; stream=Stream { id: StreamId(1), state: State { inner: Closed(EndStream) }, is_counted: false, ref_count: 1, next_pending_send: None, is_pending_send: false, send_flow: FlowControl { window_size: Window(65535), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, send_task: None, pending_send: Deque { indices: None }, next_pending_send_capacity: None, is_pending_send_capacity: false, send_capacity_inc: false, next_open: None, is_pending_open: false, is_pending_push: false, next_pending_accept: None, is_pending_accept: false, recv_flow: FlowControl { window_size: Window(2097122), available: Window(2097122) }, in_flight_recv_data: 30, next_window_update: None, is_pending_window_update: false, reset_at: None, next_reset_expire: None, pending_recv: Deque { indices: None }, is_recv: false, recv_task: None, push_task: None, pending_push_promises: Queue { indices: None, _p: PhantomData<h2::proto::streams::stream::NextAccept> }, content_length: Remaining(0) }
          0.548305s 519ms TRACE h2::proto::streams::recv auto-release closed stream (StreamId(1)) capacity: 30
          0.548327s 519ms TRACE h2::proto::streams::recv release_connection_capacity; size=30, connection in_flight_data=30
          0.548346s 519ms TRACE h2::proto::streams::counts transition_after; stream=StreamId(1); state=State { inner: Closed(EndStream) }; is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
        0.548380s 520ms TRACE uv_client::base_client Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("gitlab.com")), port: None, path: "/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/private-dep/" }))) }
        0.548446s 520ms TRACE uv_client::base_client Cannot retry error: not an IO error (`WrappedReqwestError`)
...
```

## Scenarios
- running `uv run <tool_name>` **with environment variables** -> works 🟢 
- running `uv tool run --from . <tool_name>` **with environment variables** -> does **not** work 🔴 
- running `uv tool install --from . <tool_name>` **with environment variables** -> does **not** work 🔴 
- running `uv tool run --from . <tool_name>` **with credentials in index url in `pyproject.toml`** -> works 🟢 



---

_Comment by @charliermarsh on 2024-12-10 14:55_

`uv tool` doesn't look at your `pyproject.toml`, since it's a global command (but `pyproject.toml` is scoped to a project).

---

_Comment by @ot-goegelem on 2024-12-10 15:06_

But running `uv tool run --from . <tool_name>` with credentials in index url in `pyproject.toml` **works**

I understand that `uv tool` normally doesn't look at the `pyproject.toml`, but when I am doing `--from .` it should look at my current project and take `pyproject.toml` into account. 

---

_Comment by @zanieb on 2024-12-10 15:26_

>  understand that uv tool normally doesn't look at the pyproject.toml, but when I am doing --from . it should look at my current project and take pyproject.toml into account.

I don't know if I agree. `--from .` should probably build the package as-if for production — I don't think it should even use `sources`?

---

_Comment by @SalttownUser on 2024-12-20 18:14_

I have a similar using the `uv publish` command. I'm trying to upload a package to a private GitLab package registry and getting check error.

The index url is a GitLab group url combining packages from several projects:

```
[[tool.uv.index]]
name = "gitlab-private"
url = "https://example.org/api/v4/groups/1/packages/pypi/simple"
default = true
publish-url = "https://example.org/api/v4/projects/2/packages/pypi"
```

I did use these environment variable with credentials for url:
```
UV_INDEX_GITLAB_PRIVATE_PASSWORD
UV_INDEX_GITLAB_PRIVATE_USERNAME
```

and 

```
UV_PUBLISH_USERNAME
UV_PUBLISH_PASSWORD
```

containing the credentials for publish-url.


```
uv publish --index gitlab-private -v
DEBUG uv 0.5.11 (c4d0caaee 2024-12-19)
warning: `uv publish` is experimental and may change without warning
DEBUG Publishing with index gitlab-private
DEBUG Not a distribution filename: `.gitignore`
Publishing 6 files https://example.org/api/v4/projects/2/packages/pypi
DEBUG Using request timeout of 900s
DEBUG Checking for my_package-py3-none-any.whl in the registry
DEBUG No cache entry for: https://example.org/api/v4/projects/2/packages/pypi/simple/my_package/
DEBUG No netrc file found
error: Failed to query check URL
  Caused by: Package `my_package` was not found in the registry
```

When I add the credentials to the url with `url = "https://user:password@example.org/api/v4/groups/1/packages/pypi/simple"` everything works.

---

_Comment by @zanieb on 2025-01-07 19:23_

@SalttownUser please open a separate issue, that looks unrelated to this one.

---

_Label `needs-decision` added by @zanieb on 2025-01-07 19:23_

---

_Referenced in [astral-sh/uv#10386](../../astral-sh/uv/issues/10386.md) on 2025-01-08 09:55_

---

_Comment by @rbetts on 2025-08-14 15:41_

I was confused (as a uv novice) that `uv pip install` finds a package in a private registry that `uv tool install` does not find. This issue explains clearly why that is. However, I was unable to understand the reason from the error messages or from the index documentation and only understood after finding this issue.

Perhaps https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run could gain a section on using `uv tool install` with a private index?  Or https://docs.astral.sh/uv/concepts/indexes/#-index-url-and-extra-index-url could gain a section describing the (non-)interaction with `uv tool`?

(It also took me a bit to realize that the `[[tool.uv.index]]` syntax in pyproject has nothing to do with `uv tool`... which, in hindsight, is obvious.)

---

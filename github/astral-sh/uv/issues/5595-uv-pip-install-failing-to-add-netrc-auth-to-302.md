---
number: 5595
title: "`uv pip install` failing to add netrc auth to 302 redirected request"
type: issue
state: closed
author: RichardDRJ
labels:
  - needs-design
  - network
assignees: []
created_at: 2024-07-30T11:47:13Z
updated_at: 2025-06-16T12:46:02Z
url: https://github.com/astral-sh/uv/issues/5595
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip install` failing to add netrc auth to 302 redirected request

---

_Issue opened by @RichardDRJ on 2024-07-30 11:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


Hi, I'm trying to use `uv` with a private `simpleindex` server which will conditionally route requests for different packages to different upstream indices.

One of the upstream indices requires auth, which is defined in a `~/.netrc` file; when I try to connect directly to that index, `uv pip install` correctly picks up the defined credentials. However, when I try to connect to `simpleindex`, the server returns an HTTP 302, and then UV fails to pick up the defined credentials for the redirected host.

Standard `pip` does correctly include credentials in requests triggered by a 302.

**UV Platform**: Ubuntu 20.04.6 LTS
**UV Version**: 0.2.31

### Expected flow

1. UV makes a request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. UV makes a request to {{URL 2}} _with basic auth from `~/.netrc`_

### Actual flow

1. UV makes a request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. UV makes a request to {{URL 2}} _with no auth_ (checked with mitmproxy that the auth header is not present)

-----

The command I'm running is:

```sh
env RUST_LOG=trace UV_INDEX_URL="http://127.0.0.1:8085/" uv pip install --dry-run '{{ package in private index}}' --no-cache -vvv
```

(where `http://127.0.0.1:8085/` is the `simpleindex` URL)

<details>
<summary>Full trace output</summary>

```
    0.000030s DEBUG uv uv 0.2.31
 uv_requirements::specification::from_source source={{PACKAGE SPECIFIER}}
    0.001984s DEBUG uv_python::discovery Searching for Python interpreter in system path
    0.002083s TRACE uv_python::interpreter Querying interpreter executable at /home/ubuntu/tmp/simple-index/.venv/bin/python3
    0.060733s DEBUG uv_python::discovery Found `cpython-3.11.9-linux-x86_64-gnu` at `/home/ubuntu/tmp/simple-index/.venv/bin/python3` (active virtual environment)
    0.060776s DEBUG uv::commands::pip::install Using Python 3.11.9 environment at .venv/bin/python3
    0.061281s TRACE uv_fs Checking lock for `.venv`
    0.061312s DEBUG uv_fs Acquired lock for `.venv`
    0.061996s DEBUG uv::commands::pip::install At least one requirement is not satisfied: {{PACKAGE SPECIFIER}}
 uv_client::linehaul::linehaul 
    0.063217s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
 uv_resolver::resolver::solve 
    0.068887s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.9
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.069025s   0ms DEBUG uv_resolver::resolver Adding direct dependency: {{PACKAGE SPECIFIER}}
    0.069051s   0ms  INFO pubgrub::internal::partial_solution add_decision: root @ 0a0.dev0
   uv_resolver::resolver::choose_version package={{PACKAGE NAME}}
 uv_resolver::resolver::process_request request=Versions {{PACKAGE NAME}}
   uv_client::registry_client::simple_api package={{PACKAGE NAME}}
      0.069339s   0ms TRACE uv_client::registry_client Fetching metadata for {{PACKAGE NAME}} from http://127.0.0.1:8085/{{PACKAGE NAME}}/
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/tmp/.tmpKmpDVH/simple-v10/index/2a5a9429a02c276a/{{PACKAGE NAME}}.rkyv
 uv_resolver::resolver::process_request request=Prefetch {{PACKAGE NAME}} ==0.1.0
 uv_client::cached_client::from_path_sync path="/tmp/.tmpKmpDVH/simple-v10/index/2a5a9429a02c276a/{{PACKAGE NAME}}.rkyv"
          0.069795s   0ms TRACE uv_client::cached_client No cache entry exists for /tmp/.tmpKmpDVH/simple-v10/index/2a5a9429a02c276a/{{PACKAGE NAME}}.rkyv
        0.069824s   0ms DEBUG uv_client::cached_client No cache entry for: http://127.0.0.1:8085/{{PACKAGE NAME}}/
       uv_client::cached_client::fresh_request url="http://127.0.0.1:8085/{{PACKAGE NAME}}/"
          0.069859s   0ms TRACE uv_client::cached_client Sending fresh GET request for http://127.0.0.1:8085/{{PACKAGE NAME}}/
          0.069886s   0ms TRACE uv_auth::middleware Handling request for http://127.0.0.1:8085/{{PACKAGE NAME}}/
          0.069894s   0ms TRACE uv_auth::middleware Request for http://127.0.0.1:8085/{{PACKAGE NAME}}/ is unauthenticated, checking cache
          0.069917s   0ms TRACE uv_auth::cache No credentials in cache for URL http://127.0.0.1:8085/{{PACKAGE NAME}}/
          0.069927s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for http://127.0.0.1:8085/{{PACKAGE NAME}}/
          0.069969s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("http", 127.0.0.1:8085)
          0.070003s   0ms DEBUG reqwest::connect starting new connection: http://127.0.0.1:8085/
          0.070030s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("http"), host=Some("127.0.0.1"), port=Some(Port(8085))
          0.070046s   0ms DEBUG hyper_util::client::legacy::connect::http connecting to 127.0.0.1:8085
          0.070165s   0ms DEBUG hyper_util::client::legacy::connect::http connected to 127.0.0.1:8085
          0.070196s   0ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
          0.070240s   0ms TRACE hyper_util::client::legacy::pool checkout dropped for ("http", 127.0.0.1:8085)
          0.098371s  28ms TRACE hyper_util::client::legacy::pool put; add idle connection for ("http", 127.0.0.1:8085)
          0.098415s  28ms DEBUG hyper_util::client::legacy::pool pooling idle connection for ("http", 127.0.0.1:8085)
          0.098449s  28ms DEBUG reqwest::async_impl::client redirecting 'http://127.0.0.1:8085/{{PACKAGE NAME}}/' to 'https://{{PASSWORD PROTECTED UPSTREAM}}/{{PACKAGE NAME}}'
          0.098479s  28ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", {{PASSWORD PROTECTED UPSTREAM}})
          0.098491s  28ms DEBUG reqwest::connect starting new connection: https://{{PASSWORD PROTECTED UPSTREAM}}/
          0.098514s  28ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("{{PASSWORD PROTECTED UPSTREAM}}"), port=None
    0.098559s DEBUG hyper_util::client::legacy::connect::dns resolving host="{{PASSWORD PROTECTED UPSTREAM}}"
          0.099636s  29ms DEBUG hyper_util::client::legacy::connect::http connecting to 10.200.22.171:443
          0.099849s  30ms DEBUG hyper_util::client::legacy::connect::http connected to 10.200.22.171:443
          0.101716s  31ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
          0.101766s  31ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", {{PASSWORD PROTECTED UPSTREAM}})
          0.104158s  34ms TRACE hyper_util::client::legacy::pool put; add idle connection for ("https", {{PASSWORD PROTECTED UPSTREAM}})
          0.104179s  34ms DEBUG hyper_util::client::legacy::pool pooling idle connection for ("https", {{PASSWORD PROTECTED UPSTREAM}})
          0.104192s  34ms TRACE uv_auth::middleware Request for http://127.0.0.1:8085/{{PACKAGE NAME}}/ failed with 401 Unauthorized, checking for credentials
          0.104207s  34ms TRACE uv_auth::cache No credentials in cache for realm http://127.0.0.1:8085
          0.104384s  34ms DEBUG uv_auth::middleware Checking netrc for credentials for http://127.0.0.1:8085/{{PACKAGE NAME}}/
    0.104559s TRACE uv_resolver::resolver Received package metadata for: {{PACKAGE NAME}}
      0.104617s  35ms DEBUG uv_resolver::resolver Searching for a compatible version of {{PACKAGE NAME}} (==0.1.0)
      0.104641s  35ms TRACE uv_resolver::candidate_selector selecting candidate for package {{PACKAGE NAME}} with range Range { segments: [(Included("0.1.0"), Included("0.1.0"))] } with 0 remote versions
    0.104657s  35ms DEBUG uv_resolver::resolver No compatible version found for: {{PACKAGE NAME}}
    0.104669s  35ms  INFO pubgrub::internal::core Start conflict resolution because incompat satisfied:
    {{PACKAGE NAME}} ==0.1.0 is forbidden
    0.104699s  35ms  INFO pubgrub::internal::core prior cause: root ==0a0.dev0 is forbidden
  × No solution found when resolving dependencies:
  ╰─▶ Because {{PACKAGE NAME}} was not found in the package registry and you require
      {{PACKAGE SPECIFIER}}, we can conclude that the requirements are unsatisfiable.
```
</details>

---

_Comment by @charliermarsh on 2024-07-30 12:18_

I think @zanieb will know what's going on here when they're back.

---

_Label `network` added by @charliermarsh on 2024-07-30 12:18_

---

_Comment by @zanieb on 2024-07-30 17:00_

All of our credential lookup happens on the _request_ URL but it sounds like we need to check the _response_ URL to support redirects. Can a 302 occur conditionally on the presence of authentication? i.e. what if the flow is this:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. uv makes a request to {{URL 1}} with found credentials 
4. {{URL 1}} is not redirected anymore

If this is possible, we can't always use the response URL for credential lookups. I guess we'd need to try both?

Just to clarify, the current behavior is:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. The request fails, uv looks for credentials for {{URL 1}} and does not find them

Currently, we don't follow redirects during request retries i.e. we request the first URL again.

Should the expected behavior be:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. The request fails
4. uv looks for credentials for {{URL 2}} and finds them
5. uv makes an authenticated request to {{URL 2}}

or (this feels wrong):

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. The request fails
5. uv looks for credentials for {{URL 2}} and finds them
6. uv retries the request to {{URL 1}} with the credentials for {{URL 2}}

or:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. The request fails
4. uv looks for credentials for {{URL 1}} and doesn't find them
7. uv looks for credentials for {{URL 2}} and finds them
8. uv makes an authenticated request to {{URL 2}}

which also has a variant:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
3. The request fails
4. uv looks for credentials for {{URL 1}} and finds them
6. uv makes an authenticated request to {{URL 1}}

---

_Assigned to @zanieb by @zanieb on 2024-07-30 17:00_

---

_Label `needs-design` added by @zanieb on 2024-07-30 17:00_

---

_Comment by @RichardDRJ on 2024-07-31 09:18_

First, just wanted to say thanks for the rapid response - appreciate it!

> Can a 302 occur conditionally on the presence of authentication?

This is a good question - it's not applicable to my specific usecase, but I think it's a possibility. For example, maybe an unauthenticated request to a given repository for a package redirects to public pypi, so if you're authenticated you see pre-release versions? Seems very niche though.

In my view, the expected behaviour should be a variant of this:

> 1. uv makes an unauthenticated request to {{URL 1}}
> 2. {{URL 1}} returns a 302 redirecting to {{URL 2}}
> 3. The request fails
> 4. uv looks for credentials for {{URL 2}} and finds them
> 5. uv makes an authenticated request to {{URL 2}}

... where in step (2), any auth details are added to the redirected request _before_ {{URL 2}} is called the first time - in other words, steps (4) and (5) are done before any request is made to {{URL 2}}, and so (3) doesn't happen in the happy path. So that behaviour becomes:

1. uv makes an unauthenticated request to {{URL 1}}
2. {{URL 1}} returns a 302 pointing to {{URL 2}} <--- At this point, no request is made to {{URL 2}}
3. uv looks for credentials for {{URL 2}} and finds them
4. uv makes an authenticated request to {{URL 2}}

This appears to be what Pip does (using requests under the hood) - within [SessionRedirectMixin.resolve_redirects()](https://github.com/pypa/pip/blob/8c7df92eca1d927bb441a6b15e92966ee553aca9/src/pip/_vendor/requests/sessions.py#L159), it calls [rebuildAuth()](https://github.com/pypa/pip/blob/8c7df92eca1d927bb441a6b15e92966ee553aca9/src/pip/_vendor/requests/sessions.py#L246), which strips any auth from the request on redirect to a new host, and then checks in `.netrc` to see if auth is configured for the new host.

In the error case, if the request to {{URL 2}} fails, I think it's then still fine to retry the request to {{URL 1}} (and follow the same redirect logic if it's valid). I could see there being scope for handling a 401 or 403 in a request chain slightly differently (i.e. looking for credentials again), but with the above where we look for credentials before making the {{URL 2}} request in the first place, I'd expect that to be less of a concern.

-----

As far as I can tell a more granular version of the current flow is:

1. uv tells reqwest to make a request to {{URL 1}}
2. reqwest-middleware calls the uv auth middleware to attach auth if necessary
3. reqwest makes a request to {{URL 1}}
4. {{URL 1}} returns a 302 pointing to {{URL 2}}
5. reqwest makes a request to {{URL 2}}

(Worth caveating this by saying I've not got much Rust experience, so my digging is based on what I could find briefly yesterday and I may well be missing something)

Crucially, between points (4) and (5), reqwest-middleware doesn't call the auth middleware again to add auth to the request to {{URL 2}} - I'm wondering whether this is something it can be convinced to do (although couldn't see anything obvious in either reqwest or reqwest-middleware to allow this).

Failing that, I wonder whether the 302-handling logic can be hoisted up into uv itself by adding `.policy(redirect::Policy::none())` to the client builder. In this case, the 302 would, as far as I can tell ([link1](https://docs.rs/reqwest/latest/src/reqwest/redirect.rs.html#137), [link2](https://docs.rs/reqwest/latest/reqwest/redirect/struct.Attempt.html#method.stop)), be returned as the `Ok` result, allowing uv to handle it directly.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-15 07:58_

---

_Unassigned @zanieb by @jtfmumm on 2025-04-15 07:58_

---

_Referenced in [astral-sh/uv#12920](../../astral-sh/uv/pulls/12920.md) on 2025-04-16 15:36_

---

_Closed by @jtfmumm on 2025-04-18 12:56_

---

_Referenced in [astral-sh/uv#13255](../../astral-sh/uv/issues/13255.md) on 2025-05-01 18:26_

---

_Comment by @zanieb on 2025-05-01 18:27_

We've reverted the fix here, and will now track this in https://github.com/astral-sh/uv/issues/13255

---

_Comment by @jtfmumm on 2025-06-16 12:17_

We have a new fix for this problem on #13754. It would be very helpful if anyone would be willing to test that PR on their own private index. You could build that PR or you could also use:

```
uvx -v --from "uv @ git+https://github.com/astral-sh/uv@jtfm/update-redirect-handling" uv add <pkg-from-private-index> --default-index https://<username>:<password>@<private-index-url>
```

filling in `<pkg-from-private-index>`, `<username>`, `<password>`, and `<private-index-url>` with your own values.

---

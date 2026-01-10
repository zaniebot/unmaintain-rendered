---
number: 2099
title: Fetching Error with Custom Index
type: issue
state: closed
author: hmc-cs-mdrissi
labels:
  - bug
  - registry
  - needs-mre
assignees: []
created_at: 2024-03-01T02:48:16Z
updated_at: 2024-03-05T03:38:14Z
url: https://github.com/astral-sh/uv/issues/2099
synced_at: 2026-01-10T01:23:12Z
---

# Fetching Error with Custom Index

---

_Issue opened by @hmc-cs-mdrissi on 2024-03-01 02:48_

Hi, I'm looking forward to try uv. At work we use custom index server for security built on [pypicloud](https://pypicloud.readthedocs.io/en/latest/). When I try to install an internal library with --index-url I receive an error message of,

```
Caused by: Unexpected fragment (expected `#sha256=...`) on URL: 11eb659cf9b2e36455d3f41f0...
```
I clipped the fragment but it's exactly 64 character ascii string. My exact command was,

```
uv --verbose pip install internal-lib --index-url internal-index
```

The same command using pip works. The verbose logs are (clipping out some company specific names),



<details>

```
uv_resolver::resolver::choose_version package=internal-lib
     uv_resolver::resolver::package_wait package_name=internal-lib
 uv_resolver::resolver::process_request request=Versions internal-lib
   uv_client::registry_client::simple_api package=internal-lib
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/mdrissi/Library/Caches/uv/simple-v3/a3d7793f86811a03/internal-lib.rkyv
 uv_resolver::resolver::process_request request=Prefetch internal-lib *
 uv_client::cached_client::from_path_sync path="/Users/mdrissi/Library/Caches/uv/simple-v3/a3d7793f86811a03/internal-lib.rkyv"
          0.154248s   0ms DEBUG uv_client::cached_client No cache entry for: intenal-index/internal-lib/
       uv_client::cached_client::fresh_request url="url"
       uv_client::cached_client::new_cache file=/Users/mdrissi/Library/Caches/uv/simple-v3/a3d7793f86811a03/internal-lib.rkyv
       uv_client::registry_client::parse_simple_api package=internal-lib
         uv_client::html::parse url=url
         error: Received some unexpected HTML from url/
  Caused by: Unexpected fragment (expected `#sha256=...`) on URL: 11eb659cf9b2e36455d3f41f03...
```
</details> 

I'll try to ask the security team that maintains custom index if there's any more information I can provide. Hopefully this reproduces with any `pypicloud` index. Unsure if relevant, but I also know files are hosted in GCS. The error does seem to imply it's able to fetch some html response.

I installed uv today, uv --version shows `uv 0.1.13 (9ce5170e6 2024-02-29)`.

---

_Comment by @charliermarsh on 2024-03-01 03:13_

Thanks! So, looking at the error, I think it's printing the entire fragment. Is the fragment `#11eb659cf9b2e36455d3f41f03...` (truncated at the end)? The spec suggests it should start with `#sha256=...` or `#md5=...` and so on.

---

_Comment by @charliermarsh on 2024-03-01 03:14_

I'll try spinning this up myself.

---

_Comment by @charliermarsh on 2024-03-01 03:21_

I'm having trouble getting `pypicloud` to run, it looks like I'd need to pin some deps in some direction.

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 03:24_

I do not see a # in error message url. It starts with 11eb not #11eb. copying just last part of error exactly besides clipping at end,

Caused by: Unexpected fragment (expected `#sha256=...`) on URL: 11eb659cf9b2…

If there’s an extra verbose mode/log file to find response I could try to share that cleaned. Or any other steps I could try that may reveal more info.



---

_Comment by @charliermarsh on 2024-03-01 03:30_

Oh yes, sorry -- my question is about what's actually present in the HTML file that the index is using. Like... if you go to https://pypi.org/simple/requests/, and view source, you'll see that every entry looks like:

```
<a href="https://files.pythonhosted.org/packages/4b/ad/d536b2e572e843fda13e4458c67f937b05ce359722c1e4cdad35ba05b6e3/requests-0.2.1.tar.gz#sha256=d54eb33499f018fc6bd297613bf866f8d134629c8e02964aab6ef951f460e41e" >requests-0.2.1.tar.gz</a><br />
```

Notice how the end of the URL is `#sha256=d54eb...`? I'm mostly wondering what the structure of the piece after the fragment is, in your case. Your index should support an identical URL structure, so you can go to `internal-index.com/simple/internal-package/` to view it. Does the URL end with `#11eb659cf9b2...`? Is there anything else to the structure of it? I just don't know what that value _is_ and why the server is including it.

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 03:55_

Not too familiar with rust, but am familiar with pip/python so I added breakpoints inside pip to take a look at where/how it processes the index html file. That logic lives around [here](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/index/package_finder.py#L784).

The response content looks like

```
<a href="0.2.997/internal_lib-0.2.997-py3-none-any.whl#9599d3a5a9cbb4203f91395c93a06004eba3c78e0af0dc..." data-requires-python="&gt;=3.7">internal_lib-0.2.997-py3-none-any.whl</a>

```

If there are any other pip intermediate variables that'd be helpful I can add those too.

edit: I think it's url is relative unlike pypi case. pip ends up concatenating href value to the index to the form the full url.

---

_Comment by @charliermarsh on 2024-03-01 03:59_

That's perfect, thanks. Does [`find_hash_url_fragment`](https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/models/link.py#L74) match anything, or is it returning `None`?

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 04:00_

It returns None.

---

_Comment by @charliermarsh on 2024-03-01 04:01_

Okay, cool. (Relative URL is totally fine, by the way.)

So pip is effectively ignoring the fragment, while we're raising an error. It feels like a bug in pypiserver or in its configuration, since it _looks_ like a hash, but there should be a hash algorithm at the start, like:

```html
<a href="0.2.997/internal_lib-0.2.997-py3-none-any.whl#sha256=9599d3a5a9cbb4203f91395c93a06004eba3c78e0af0dc653b8cbed3eef77176" data-requires-python="&gt;=3.7">internal_lib-0.2.997-py3-none-any.whl</a>
```

The code in pypiserver seems to be roughly here: https://github.com/pypiserver/pypiserver/blob/50c7a78f4f4e288d023d667873b4cbac44e0915c/pypiserver/backend.py#L267. But I imagine that's much harder for you to debug.

---

_Comment by @charliermarsh on 2024-03-01 04:02_

If it's a common thing, we could just loosen the validation and make these warnings for a missing hash prefix.

---

_Comment by @charliermarsh on 2024-03-01 04:07_

Mostly just trying to figure out how the server got into this state, e.g., whether there's an older pypiserver version that did this consistently.

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 04:27_

Yeah it's easy for me to get any pip/response info, but our internal index server unsure I have access (nor awareness) to debug. Hopefully I can get one of security engineers that works on our index server to see if they are aware why the prefix is missing.

edit: My personal guess is our setup has proxy internal server and I have a feeling we only ever use one hash type (say sha256) so it's unnecessary to include which type it is. The proxy might also already be doing validation. That's gut guess though as I don't have full knowledge on index setup.

---

_Comment by @charliermarsh on 2024-03-01 05:12_

If you can get any more info I'd really appreciate it!

---

_Label `needs-mre` added by @charliermarsh on 2024-03-01 05:37_

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 07:19_

Hmm, I found the code for index server where it constructs that url `internal_lib-0.2.997-py3-none-any.whl#9599d3a5a9cbb4203f91395c93a06004eba3c78e0af0dc`. And I think it's just a small bug in our internal index server. pypicloud has some support for plugin code and the url present actually comes from my company specific plugin. I'll file a pr tomorrow to get it fixed.

It's up to you on whether uv should be as permissive as pip or to close this as expected. I'm unsure if pip's choice was intentionally permissive (maybe other custom index servers have similar issues), or it's accidental.

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 21:35_

The good news is fixing fragment fixes the original error. The bad news is it still fails to install with a different later error message. As before I'm comparing `pip install --index-url=INDEX internal-lib` vs `uv pip install --index-url=INDEX internal-lib` where pip succeeds and uv fails.

Here's the new stack trace,

<details>


```
 uv::requirements::from_source source=internal-lib
    0.022144s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/company/Dev/.venvs/bento_uv2
    0.022492s DEBUG uv_interpreter::interpreter Probing interpreter info for: /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.080152s DEBUG uv_interpreter::interpreter Found Python 3.9.16 for: /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.088255s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.089335s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.389671s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.389752s   0ms DEBUG uv_resolver::resolver Adding direct dependency: internal-lib*
   uv_resolver::resolver::choose_version package=internal-lib
     uv_resolver::resolver::package_wait package_name=internal-lib
 uv_resolver::resolver::process_request request=Versions internal-lib
   uv_client::registry_client::simple_api package=internal-lib
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/77feaa605ae7f1e2/internal-lib.rkyv
 uv_resolver::resolver::process_request request=Prefetch internal-lib *
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/77feaa605ae7f1e2/internal-lib.rkyv"
          0.390265s   0ms DEBUG uv_client::cached_client No cache entry for: https://index/python/virtual/internal-lib/
       uv_client::cached_client::fresh_request url="https://index/python/virtual/internal-lib/"
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/77feaa605ae7f1e2/internal-lib.rkyv
       uv_client::registry_client::parse_simple_api package=internal-lib
         uv_client::html::parse url=https://AUTH_CODE@index/python/virtual/internal-lib/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=internal-lib==0.1.602402
     uv_client::registry_client::wheel_metadata built_dist=internal-lib==0.1.602402
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/77feaa605ae7f1e2/internal-lib/internal-lib-0.1.602402-py3-none-any.msgpack
        1.235388s 845ms DEBUG uv_resolver::resolver Searching for a compatible version of internal-lib (*)
        1.235404s 845ms DEBUG uv_resolver::resolver Selecting: internal-lib==0.1.602402 (internal-lib-0.1.602402-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/77feaa605ae7f1e2/internal-lib/internal-lib-0.1.602402-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=internal-lib, version=0.1.602402
     uv_resolver::resolver::distributions_wait package_id=internal-lib-0.1.602402
              1.235539s   0ms DEBUG uv_client::cached_client No cache entry for: https://index/python/virtual/internal-lib/0.1.602402/internal-lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2
           uv_client::cached_client::fresh_request url="https://index/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"
error: Failed to download: internal-lib==0.1.602402
  Caused by: HTTP status client error (404 Not Found) for url (https://index/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2)
```

</details> 

It does look like uv makes more progress and some requests work out. Like before if there's any pip intermediate values that'd be helpful for debugging I can find them. I'll try to find exact urls that pip requests. I notice uv requests url with fragment and maybe pip is requesting the url without the fragment?

---

_Comment by @charliermarsh on 2024-03-01 21:42_

Nice! To confirm, pip is hitting a URL like `https://index/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl`? So, it's the same URL, without the fragment?

---

_Comment by @hmc-cs-mdrissi on 2024-03-01 21:48_

https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_internal/network/download.py#L116 Yup. Pip sends request without the fragment. I suspect that will fix this error and fingers crossed uv would work for me then.

---

_Comment by @charliermarsh on 2024-03-01 21:50_

Oh wow, I had no idea. Okay, that's a bug on our side then.

---

_Label `bug` added by @charliermarsh on 2024-03-01 21:50_

---

_Comment by @charliermarsh on 2024-03-01 21:50_

I'll take a look now (but need to log off in a few, so may not resolve it in the next few mins).

---

_Referenced in [astral-sh/uv#2123](../../astral-sh/uv/pulls/2123.md) on 2024-03-01 22:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-01 23:49_

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 21:21_

Continuing the discussion here, I think at moment my current hypothesis is inconsistent authentication headers between different requests and that logging request headers or pointing me to where key requests (index/wheel) fetch are being sent and I'll try to learn basic rust to do some print debugging.

---

_Label `registry` added by @zanieb on 2024-03-04 21:23_

---

_Comment by @charliermarsh on 2024-03-04 21:27_

@hmc-cs-mdrissi - Here's an example branch that should log at least the headers: https://github.com/astral-sh/uv/pull/2175

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 22:01_

Thank you. Here are the new logs,


<details><summary>Error Logs</summary>


```
 uv::requirements::from_source source=internal-lib
    0.003078s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/company/Dev/.venvs/bento_uv2
    0.003938s  WARN uv_interpreter::interpreter Broken cache entry at /Users/pa-loaner/Library/Caches/uv/interpreter-v0/30abbf5ed2b4051e.msgpack, removing: array had incorrect length, expected 6
    0.004402s DEBUG uv_interpreter::interpreter Probing interpreter info for: /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.055560s DEBUG uv_interpreter::interpreter Found Python 3.9.16 for: /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.056456s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/company/Dev/.venvs/bento_uv2/bin/python
    0.063417s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.349018s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.349380s   0ms DEBUG uv_resolver::resolver Adding direct dependency: internal-lib*
   uv_resolver::resolver::choose_version package=internal-lib
     uv_resolver::resolver::package_wait package_name=internal-lib
 uv_resolver::resolver::process_request request=Versions internal-lib
   uv_client::registry_client::simple_api package=internal-lib
        0.350011s   0ms DEBUG uv_client::registry_client simple_single_index: Request { method: GET, url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("registry.company-dev.net")), port: None, path: "/python/virtual/internal-lib/", query: None, fragment: None }, headers: {"authorization": Sensitive, "accept-encoding": "gzip", "accept": "application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01"} }
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/de35cb29fdfd3639/internal-lib.rkyv
 uv_resolver::resolver::process_request request=Prefetch internal-lib *
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/de35cb29fdfd3639/internal-lib.rkyv"
          0.350806s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/internal-lib/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/internal-lib/"
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/de35cb29fdfd3639/internal-lib.rkyv
       uv_client::registry_client::parse_simple_api package=internal-lib
         uv_client::html::parse url=https://LCAv1:eyJhbGciOiJFUzI1NiIsImtpZCI6InBlcnNvbmFsOkpUNG1tVXZHYWdRamNCWnhiWVMyMFVaWU9YRWplRXl6VVR3REl3N294TmciLCJ0eXAiOiJqd3QifQ.eyJpc3MiOiJtZHJpc3NpQHNuYXBjaGF0LmNvbSIsImF1ZCI6InJlZ2lzdHJ5LnNuYXAtZGV2Lm5ldCIsImlhdCI6MTcwOTU4ODMwNCwiZXhwIjoxNzA5NTkxOTA0fQ.hXpbvOLHLp0Yc9MbCMHJEhxfZ3X5sN0Ry5ibgnM_TWzhfo7-OeQFq-Cz7ZCJVe4tU8hsD7cnjLEQfLi1McX6MA@registry.company-dev.net/python/virtual/internal-lib/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=internal-lib==0.1.602402
     uv_client::registry_client::wheel_metadata built_dist=internal-lib==0.1.602402
         14.928886s   0ms DEBUG uv_client::registry_client wheel_metadata_no_pep658: Request { method: HEAD, url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("registry.company-dev.net")), port: None, path: "/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl", query: None, fragment: Some("sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2") }, headers: {"authorization": Sensitive, "accept-encoding": "identity"} }
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/de35cb29fdfd3639/internal-lib/internal_lib-0.1.602402-py3-none-any.msgpack
       14.929255s  14s  DEBUG uv_resolver::resolver Searching for a compatible version of internal-lib (*)
       14.929312s  14s  DEBUG uv_resolver::resolver Selecting: internal-lib==0.1.602402 (internal_lib-0.1.602402-py3-none-any.whl)
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/de35cb29fdfd3639/internal-lib/internal_lib-0.1.602402-py3-none-any.msgpack"
   uv_resolver::resolver::get_dependencies package=internal-lib, version=0.1.602402
     uv_resolver::resolver::distributions_wait package_id=internal-lib-0.1.602402
             14.930694s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"
error: Failed to download: internal-lib==0.1.602402
  Caused by: HTTP status client error (404 Not Found) for url (https://registry.company-dev.net/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2)


```

</details> 

Highlighting header logs,


```
        0.350011s   0ms DEBUG uv_client::registry_client simple_single_index: Request { method: GET, url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("registry.company-dev.net")), port: None, path: "/python/virtual/internal-lib/", query: None, fragment: None }, headers: {"authorization": Sensitive, "accept-encoding": "gzip", "accept": "application/vnd.pypi.simple.v1+json, application/vnd.pypi.simple.v1+html;q=0.2, text/html;q=0.01"} }
```

is successful request (index).

The unsuccessful request (wheel),

```
         14.928886s   0ms DEBUG uv_client::registry_client wheel_metadata_no_pep658: Request { method: HEAD, url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("registry.company-dev.net")), port: None, path: "/python/virtual/internal-lib/0.1.602402/internal_lib-0.1.602402-py3-none-any.whl", query: None, fragment: Some("sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2") }, headers: {"authorization": Sensitive, "accept-encoding": "identity"} }
```

Very unsure now as both requests have an authorization and failed request url is perfect match with pip's request. Only other noticeable diff I see is method type is different (HEAD vs GET). Could authorization for different http request types potentially differ? Or maybe index we have doesn't support HEAD request?

---

_Comment by @charliermarsh on 2024-03-04 22:03_

Perhaps the index is returning 404 on HEAD request? (You're seeing a 404 now, right?)

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 22:04_

Yes the error is 404. I'm asking security if they're aware whether our index server supports HEAD requests. My confidence is low that's issue, just most noticeable difference in pip vs uv's request I see.

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 22:07_

Ok I successfully confirmed this is likely issue. If I use curl to send a get request it works. The same request (headers) with HEAD request returns 404. I don't think our index server supports HEAD is the issue.

---

_Comment by @charliermarsh on 2024-03-04 22:10_

Okay, let me see if I can add fallback.

---

_Comment by @charliermarsh on 2024-03-04 23:20_

@hmc-cs-mdrissi - Could you try https://github.com/astral-sh/uv/pull/2186?

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 23:44_

It succeeded. I am now able to install internal-lib. I'll try a more complex command next (install several internal-libraries), but very promising.

---

_Referenced in [astral-sh/uv#2186](../../astral-sh/uv/pulls/2186.md) on 2024-03-05 00:35_

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 01:36_

Hmm, I tested installing our full environment of ~80 dependencies with ~400 transitive using your PR. Some installs work, some give 404 errors still. `pip` works for all. So I think current pr definitely helps but doesn't fix all my index auth issues yet.

Failed install logs, this one uses actual public package. Some other public packages work, so something specific about path/requests it takes.

<details><summary>Failed Install</summary>


```
     Running `target/debug/uv --verbose pip install '--index-url=https://LCAv1:AUTH@registry.company-dev.net/python/virtual/' kubernetes-stubs==22.6.0`
 uv::requirements::from_source source=kubernetes-stubs==22.6.0
    0.001443s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2
    0.001823s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.9.16, skipping probing: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.001879s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.008456s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.276457s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.276945s   0ms DEBUG uv_resolver::resolver Adding direct dependency: kubernetes-stubs==22.6.0
   uv_resolver::resolver::choose_version package=kubernetes-stubs
     uv_resolver::resolver::package_wait package_name=kubernetes-stubs
 uv_resolver::resolver::process_request request=Versions kubernetes-stubs
   uv_client::registry_client::simple_api package=kubernetes-stubs
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/kubernetes-stubs.rkyv
 uv_resolver::resolver::process_request request=Prefetch kubernetes-stubs ==22.6.0
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/kubernetes-stubs.rkyv"
          0.279096s   1ms DEBUG uv_client::cached_client Found fresh response for: https://registry.company-dev.net/python/virtual/kubernetes-stubs/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=kubernetes-stubs==22.6.0
     uv_client::registry_client::wheel_metadata built_dist=kubernetes-stubs==22.6.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/kubernetes-stubs/kubernetes_stubs-22.6.0-py2.py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/kubernetes-stubs/kubernetes_stubs-22.6.0-py2.py3-none-any.msgpack"
        0.280425s   3ms DEBUG uv_resolver::resolver Searching for a compatible version of kubernetes-stubs (==22.6.0)
        0.280478s   3ms DEBUG uv_resolver::resolver Selecting: kubernetes-stubs==22.6.0 (kubernetes_stubs-22.6.0-py2.py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=kubernetes-stubs, version=22.6.0
     uv_resolver::resolver::distributions_wait package_id=kubernetes-stubs-22.6.0
              0.280802s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/kubernetes-stubs/22.6.0/kubernetes_stubs-22.6.0-py2.py3-none-any.whl#sha256=a5b192f655df30880599d5848bff831a4a5508d54441caec57f494f31d69a4b3
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/kubernetes-stubs/22.6.0/kubernetes_stubs-22.6.0-py2.py3-none-any.whl#sha256=a5b192f655df30880599d5848bff831a4a5508d54441caec57f494f31d69a4b3"
          0.466836s 186ms  WARN uv_client::registry_client Range requests not supported for kubernetes_stubs-22.6.0-py2.py3-none-any.whl; downloading wheel
error: Failed to download: kubernetes-stubs==22.6.0
  Caused by: HTTP status client error (404 Not Found) for url (https://registry.company-dev.net/python/virtual/kubernetes-stubs/22.6.0/kubernetes_stubs-22.6.0-py2.py3-none-any.whl#sha256=a5b192f655df30880599d5848bff831a4a5508d54441caec57f494f31d69a4b3)
```

</details> 


<details><summary>Good Install Run</summary>



```
(bento_uv2) pa-loaner@C02DVAQNMD6R uv % cargo run -- --verbose pip install --index-url=$INDEX_URL company-lib==0.1.602402
    Finished dev [unoptimized + debuginfo] target(s) in 0.49s
     Running `target/debug/uv --verbose pip install '--index-url=https://LCAv1:AUTH@registry.company-dev.net/python/virtual/' company-lib==0.1.602402`
 uv::requirements::from_source source=company-lib==0.1.602402
    0.001434s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2
    0.002028s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.9.16, skipping probing: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.002089s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.007879s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.275747s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.276215s   0ms DEBUG uv_resolver::resolver Adding direct dependency: company-lib==0.1.602402
   uv_resolver::resolver::choose_version package=company-lib
     uv_resolver::resolver::package_wait package_name=company-lib
 uv_resolver::resolver::process_request request=Versions company-lib
   uv_client::registry_client::simple_api package=company-lib
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/company-lib.rkyv
 uv_resolver::resolver::process_request request=Prefetch company-lib ==0.1.602402
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/company-lib.rkyv"
          0.278668s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/company-lib/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/company-lib/"
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/company-lib.rkyv
       uv_client::registry_client::parse_simple_api package=company-lib
         uv_client::html::parse url=https://LCAv1:AUTH@registry.company-dev.net/python/virtual/company-lib/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=company-lib==0.1.602402
     uv_client::registry_client::wheel_metadata built_dist=company-lib==0.1.602402
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/company-lib/company_lib-0.1.602402-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/company-lib/company_lib-0.1.602402-py3-none-any.msgpack"
        2.528605s   2s  DEBUG uv_resolver::resolver Searching for a compatible version of company-lib (==0.1.602402)
        2.528682s   2s  DEBUG uv_resolver::resolver Selecting: company-lib==0.1.602402 (company_lib-0.1.602402-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=company-lib, version=0.1.602402
     uv_resolver::resolver::distributions_wait package_id=company-lib-0.1.602402
              2.529225s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/company-lib/0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/company-lib/0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"
          2.620270s  92ms  WARN uv_client::registry_client Range requests not supported for company_lib-0.1.602402-py3-none-any.whl; downloading wheel
        3.541278s   1s  DEBUG uv_resolver::resolver Adding transitive dependency: grpcio-tools>=1.35.0
        3.541343s   1s  DEBUG uv_resolver::resolver Adding transitive dependency: protobuf>=3.12.0
        3.541378s   1s  DEBUG uv_resolver::resolver Adding transitive dependency: googleapis-common-protos>=1.52.0
   uv_resolver::resolver::choose_version package=grpcio-tools
     uv_resolver::resolver::package_wait package_name=grpcio-tools
 uv_resolver::resolver::process_request request=Versions grpcio-tools
   uv_client::registry_client::simple_api package=grpcio-tools
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio-tools.rkyv
 uv_resolver::resolver::process_request request=Versions protobuf
   uv_client::registry_client::simple_api package=protobuf
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio-tools.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/protobuf.rkyv
 uv_resolver::resolver::process_request request=Versions googleapis-common-protos
   uv_client::registry_client::simple_api package=googleapis-common-protos
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/protobuf.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/googleapis-common-protos.rkyv
 uv_resolver::resolver::process_request request=Prefetch googleapis-common-protos >=1.52.0
 uv_resolver::resolver::process_request request=Prefetch protobuf >=3.12.0
 uv_resolver::resolver::process_request request=Prefetch grpcio-tools >=1.35.0
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/googleapis-common-protos.rkyv"
          3.543078s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/grpcio-tools/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/grpcio-tools/"
          3.543356s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/protobuf/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/protobuf/"
          3.543632s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/googleapis-common-protos/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/googleapis-common-protos/"
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/protobuf.rkyv
       uv_client::registry_client::parse_simple_api package=protobuf
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/googleapis-common-protos.rkyv
       uv_client::registry_client::parse_simple_api package=googleapis-common-protos
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio-tools.rkyv
       uv_client::registry_client::parse_simple_api package=grpcio-tools
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=googleapis-common-protos==1.62.0
     uv_client::registry_client::wheel_metadata built_dist=googleapis-common-protos==1.62.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/googleapis-common-protos/googleapis_common_protos-1.62.0-py2.py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/googleapis-common-protos/googleapis_common_protos-1.62.0-py2.py3-none-any.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=grpcio-tools==1.62.0
     uv_client::registry_client::wheel_metadata built_dist=grpcio-tools==1.62.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio-tools/grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio-tools/grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=protobuf==4.25.3
     uv_client::registry_client::wheel_metadata built_dist=protobuf==4.25.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/protobuf/protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/protobuf/protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.msgpack"
        4.068518s 526ms DEBUG uv_resolver::resolver Searching for a compatible version of grpcio-tools (>=1.35.0)
        4.068596s 526ms DEBUG uv_resolver::resolver Selecting: grpcio-tools==1.62.0 (grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.whl)
   uv_resolver::resolver::get_dependencies package=grpcio-tools, version=1.62.0
     uv_resolver::resolver::distributions_wait package_id=grpcio-tools-1.62.0
              4.068863s   2ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/pypi/f0/43/c9d8f75ddf08e2a0a27db243c13a700c3cc7ec615b545b697cf6f715ad92/googleapis_common_protos-1.62.0-py2.py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/pypi/f0/43/c9d8f75ddf08e2a0a27db243c13a700c3cc7ec615b545b697cf6f715ad92/googleapis_common_protos-1.62.0-py2.py3-none-any.whl.metadata"
              4.069087s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/pypi/de/09/cf4acd0c8b21613a39f84b7f4b8b31b799569d26c14c98b4f5699e23235f/grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.whl.metadata
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/pypi/de/09/cf4acd0c8b21613a39f84b7f4b8b31b799569d26c14c98b4f5699e23235f/grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.whl.metadata"
              4.069285s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/pypi/f3/bf/26deba06a4c910a85f78245cac7698f67cedd7efe00d04f6b3e1b3506a59/protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.whl.metadata
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/pypi/f3/bf/26deba06a4c910a85f78245cac7698f67cedd7efe00d04f6b3e1b3506a59/protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/protobuf/protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.msgpack
           uv_client::registry_client::parse_metadata21
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/googleapis-common-protos/googleapis_common_protos-1.62.0-py2.py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio-tools/grpcio_tools-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack
           uv_client::registry_client::parse_metadata21
        4.522503s 453ms DEBUG uv_resolver::resolver Adding transitive dependency: protobuf>=4.21.6, <5.0.dev0
        4.522609s 453ms DEBUG uv_resolver::resolver Adding transitive dependency: grpcio>=1.62.0
        4.522674s 453ms DEBUG uv_resolver::resolver Adding transitive dependency: setuptools*
   uv_resolver::resolver::choose_version package=protobuf
     uv_resolver::resolver::package_wait package_name=protobuf
        4.523027s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of protobuf (>=4.21.6, <5.0.dev0)
        4.523086s   0ms DEBUG uv_resolver::resolver Selecting: protobuf==4.25.3 (protobuf-4.25.3-cp37-abi3-macosx_10_9_universal2.whl)
   uv_resolver::resolver::get_dependencies package=protobuf, version=4.25.3
     uv_resolver::resolver::distributions_wait package_id=protobuf-4.25.3
   uv_resolver::resolver::choose_version package=googleapis-common-protos
     uv_resolver::resolver::package_wait package_name=googleapis-common-protos
        4.523490s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of googleapis-common-protos (>=1.52.0)
        4.523543s   0ms DEBUG uv_resolver::resolver Selecting: googleapis-common-protos==1.62.0 (googleapis_common_protos-1.62.0-py2.py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=googleapis-common-protos, version=1.62.0
     uv_resolver::resolver::distributions_wait package_id=googleapis-common-protos-1.62.0
        4.523837s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: protobuf>=3.19.5, <3.20.0 | >3.20.0, <3.20.1 | >3.20.1, <4.21.1 | >4.21.1, <4.21.2 | >4.21.2, <4.21.3 | >4.21.3, <4.21.4 | >4.21.4, <4.21.5 | >4.21.5, <5.0.0.dev0
   uv_resolver::resolver::choose_version package=grpcio
     uv_resolver::resolver::package_wait package_name=grpcio
 uv_resolver::resolver::process_request request=Versions grpcio
   uv_client::registry_client::simple_api package=grpcio
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio.rkyv
 uv_resolver::resolver::process_request request=Versions setuptools
   uv_client::registry_client::simple_api package=setuptools
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio.rkyv"
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/setuptools.rkyv
 uv_resolver::resolver::process_request request=Prefetch protobuf >=4.21.6, <5.0.dev0
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/setuptools.rkyv"
 uv_resolver::resolver::process_request request=Prefetch setuptools *
 uv_resolver::resolver::process_request request=Prefetch grpcio >=1.62.0
          4.525009s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/grpcio/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/grpcio/"
          4.525274s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/setuptools/
       uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/setuptools/"
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/grpcio.rkyv
       uv_client::registry_client::parse_simple_api package=grpcio
       uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/5b8e1d2d8a236b08/setuptools.rkyv
       uv_client::registry_client::parse_simple_api package=setuptools
   uv_resolver::version_map::from_metadata
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=setuptools==69.0.3
     uv_client::registry_client::wheel_metadata built_dist=setuptools==69.0.3
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/setuptools/setuptools-69.0.3-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/setuptools/setuptools-69.0.3-py3-none-any.msgpack"
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=grpcio==1.62.0
     uv_client::registry_client::wheel_metadata built_dist=grpcio==1.62.0
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio/grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio/grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack"
        4.975554s 451ms DEBUG uv_resolver::resolver Searching for a compatible version of grpcio (>=1.62.0)
        4.975613s 451ms DEBUG uv_resolver::resolver Selecting: grpcio==1.62.0 (grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.whl)
   uv_resolver::resolver::get_dependencies package=grpcio, version=1.62.0
     uv_resolver::resolver::distributions_wait package_id=grpcio-1.62.0
              4.975847s   1ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/pypi/55/3a/5121b58b578a598b269537e09a316ad2a94fdd561a2c6eb75cd68578cc6b/setuptools-69.0.3-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/pypi/55/3a/5121b58b578a598b269537e09a316ad2a94fdd561a2c6eb75cd68578cc6b/setuptools-69.0.3-py3-none-any.whl.metadata"
              4.976057s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/pypi/b3/61/9a3c14f1890e3275817b264334b02176c2ad106415eae6dd45b2a4b65ec3/grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.whl.metadata
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/pypi/b3/61/9a3c14f1890e3275817b264334b02176c2ad106415eae6dd45b2a4b65ec3/grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/setuptools/setuptools-69.0.3-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/grpcio/grpcio-1.62.0-cp39-cp39-macosx_10_10_universal2.msgpack
           uv_client::registry_client::parse_metadata21
   uv_resolver::resolver::choose_version package=setuptools
     uv_resolver::resolver::package_wait package_name=setuptools
        5.272621s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of setuptools (*)
        5.272682s   0ms DEBUG uv_resolver::resolver Selecting: setuptools==69.0.3 (setuptools-69.0.3-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=setuptools, version=69.0.3
     uv_resolver::resolver::distributions_wait package_id=setuptools-69.0.3
Resolved 6 packages in 4.99s
    5.273850s DEBUG uv_installer::plan Requirement already satisfied: googleapis-common-protos==1.62.0
    5.273894s DEBUG uv_installer::plan Requirement already satisfied: grpcio==1.62.0
    5.273918s DEBUG uv_installer::plan Requirement already satisfied: grpcio-tools==1.62.0
    5.274100s DEBUG uv_installer::plan Identified uncached requirement: company-lib ==0.1.602402
    5.274143s DEBUG uv_installer::plan Requirement already satisfied: protobuf==4.25.3
    5.274165s DEBUG uv_installer::plan Requirement already satisfied: setuptools==69.0.3
    5.274322s DEBUG uv_installer::plan Preserving seed package: wheel==0.42.0
    5.274349s DEBUG uv_installer::plan Preserving seed package: pip==24.1.dev0 (from file:///Users/pa-loaner/Snapchat/Dev/pip)
 uv_installer::downloader::download total=1
   uv_installer::downloader::get_wheel name=company-lib==0.1.602402, size=None, url="0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("company-lib"), version: "0.1.602402", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: None, filename: "company_lib-0.1.602402-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.7" }])), size: None, upload_time_utc_ms: None, url: RelativeUrl("https://LCAv1:AUTH@registry.company-dev.net/python/virtual/company-lib/", "0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"), yanked: None }, index: Url(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "LCAv1", password: Some("PASSWORD"), host: Some(Domain("registry.company-dev.net")), port: None, path: "/python/virtual/", query: None, fragment: None }, given: Some("https://LCAv1:AUTH@registry.company-dev.net/python/virtual/") }) }))
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/company-lib/company_lib-0.1.602402-py3-none-any.http
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/company-lib/company_lib-0.1.602402-py3-none-any.http"
              5.275504s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.company-dev.net/python/virtual/company-lib/0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2
           uv_client::cached_client::fresh_request url="https://registry.company-dev.net/python/virtual/company-lib/0.1.602402/company_lib-0.1.602402-py3-none-any.whl#sha256=01c7adb9b24cef1326fbca1bce8c00fe4c611247a515764804928a7272c6b2f2"
           uv_client::cached_client::new_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/5b8e1d2d8a236b08/company-lib/company_lib-0.1.602402-py3-none-any.http
           uv_distribution::distribution_database::download wheel=company-lib==0.1.602402
Downloaded 1 package in 3.94s
 uv_installer::installer::install num_wheels=1
Installed 1 package in 280ms
 + company-lib==0.1.602402
```

</details> 

---

_Comment by @charliermarsh on 2024-03-05 01:39_

> Failed install logs, this one uses actual public package.

What do you mean by "public package" Is the index is a proxy, and the "failed" example should be proxying to PyPI?

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 01:44_

Index server supports both public and private (internal) packages. For public ones it proxies. I've gotten some public packages and some private ones to install successfully. Some fail. I still don't know what distinguishes failing package vs one that works fine.

I also double checked and package that failed is technically internal (it's public but got patched and re-uploaded internally so looks like internal). Although maybe fact it's both internal and package name exists on pypi might have some relevance (or just luck need to try more packages).

I need to review logs first on any ideas.

edit: Both runs (good and bad) have,

```
          2.620270s  92ms  WARN uv_client::registry_client Range requests not supported for company_lib-0.1.602402-py3-none-any.whl; downloading wheel
```

edit: I found another purely internal package that failed to install while other pure internal ones succeed.

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 03:17_

I made a mistake. We have two index servers, staging and prod version. The staging only contains some internal packages and it's what I was using to test (since there was other sha256= fix earlier). Failed install issue was because some packages were missing.

After swapping to using prod index server I think it mostly works. It got pretty far, and eventually failed here

```
error: Failed to download: data-mesh-cli==0.0.66
  Caused by: The wheel data_mesh_cli-0.0.66-py3-none-any.whl is not a valid zip file
  Caused by: an upstream reader returned an error: request or response body error: operation timed out
  Caused by: request or response body error: operation timed out
  Caused by: operation timed out
```

Which looks quite similar to a different [open issue](https://github.com/astral-sh/uv/issues/1912) and I think that package is several hundred megabytes. Unsure why pip doesn't timeout though and if there's some large wheel optimization/laxer timeout needed.

---

_Closed by @charliermarsh on 2024-03-05 03:18_

---

_Comment by @charliermarsh on 2024-03-05 03:19_

Oh sorry, this just got closed since I merged the change that fixed the HEAD requests for you. Do you want to file anything else as separate issues?

---

_Comment by @hmc-cs-mdrissi on 2024-03-05 03:21_

I think closing it is fine. You've resolved main issue and one leftover issue already has an [open issue](https://github.com/astral-sh/uv/issues/1912). My guess is any massive (500 MB wheel) may run into some timeouts?

Thank you very much for debugging all of this with me. I understand company internal indices are challenge to reproduce and see where issue lies.

---

_Comment by @charliermarsh on 2024-03-05 03:38_

Thank _you_! It's really hard to debug internal indices but I'm grateful when people can be so actively involved in the process. The way I think of it is: if you experienced this, odds are someone else will too, so it's a great use of time whenever we can find a source of unreliability like the 404-on-HEAD thing here.

---

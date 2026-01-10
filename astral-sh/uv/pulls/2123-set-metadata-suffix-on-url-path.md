```yaml
number: 2123
title: "Set `.metadata` suffix on URL path"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/fragment
created_at: 2024-03-01T22:54:30Z
updated_at: 2024-03-04T20:51:09Z
url: https://github.com/astral-sh/uv/pull/2123
synced_at: 2026-01-10T14:54:43Z
```

# Set `.metadata` suffix on URL path

---

_Pull request opened by @charliermarsh on 2024-03-01 22:54_

## Summary

Ensures that we don't add the `.metadata` suffix after the fragment, if it exists.

---

_Label `bug` added by @charliermarsh on 2024-03-01 22:54_

---

_Label `compatibility` added by @charliermarsh on 2024-03-01 22:54_

---

_Comment by @charliermarsh on 2024-03-01 23:48_

@hmc-cs-mdrissi -- are you interested in testing this branch?

---

_Comment by @charliermarsh on 2024-03-01 23:49_

@pradyunsg -- sorry to bother, it was pointed out to me that `pip` strips fragments when making HTTP requests. Do you know if this is encoded in any spec?

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 00:15_

Yes I am happy to. First time building rust codebase and fairly smooth experience.

It sadly failed though,

<details><summary>Verbose Logs</summary>

```
     Running `target/debug/uv --verbose pip install '--index-url=https://AUTH@index/python/virtual/' internal-lib`
 uv::requirements::from_source source=internal-lib
    0.001651s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2
    0.002148s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.9.16, skipping probing: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.002237s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.010349s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.306961s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.307338s   0ms DEBUG uv_resolver::resolver Adding direct dependency: internal-lib*
   uv_resolver::resolver::choose_version package=internal-lib
     uv_resolver::resolver::package_wait package_name=internal-lib
 uv_resolver::resolver::process_request request=Versions internal-lib
   uv_client::registry_client::simple_api package=internal-lib
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/28d1ce4468d5ce6b/internal-lib.rkyv
 uv_resolver::resolver::process_request request=Prefetch internal-lib *
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/28d1ce4468d5ce6b/internal-lib.rkyv"
          0.325625s  17ms DEBUG uv_client::cached_client Found fresh response for: https://index/python/virtual/internal-lib/
   uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=internal-lib==0.1.602402
     uv_client::registry_client::wheel_metadata built_dist=internal-lib==0.1.602402
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/28d1ce4468d5ce6b/internal-lib/internal-lib-0.1.602402-py3-none-any.msgpack
        0.618904s 311ms DEBUG uv_resolver::resolver Searching for a compatible version of internal-lib (*)
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/wheels-v0/index/28d1ce4468d5ce6b/internal-lib/internal-lib-0.1.602402-py3-none-any.msgpack"
        0.619005s 311ms DEBUG uv_resolver::resolver Selecting: internal-lib==0.1.602402 (internal-lib-0.1.602402-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=internal-lib, version=0.1.602402
     uv_resolver::resolver::distributions_wait package_id=internal-lib-0.1.602402
              0.619291s   0ms DEBUG uv_client::cached_client No cache entry for: index/python/virtual/internal-lib/0.1.602402/internal-lib-0.1.602402-py3-none-any.whl
           uv_client::cached_client::fresh_request url="index/python/virtual/internal-lib/0.1.602402/internal-lib-0.1.602402-py3-none-any.whl"
error: Failed to download: internal-lib==0.1.602402
  Caused by: HTTP status client error (404 Not Found) for url (index/python/virtual/internal-lib/0.1.602402/internal-lib-0.1.602402-py3-none-any.whl)
```

</details> 

`cargo run -- --verbose pip install --index-url=$INDEX_URL internal-lib` being exact command I used, while pip install --index-url=$INDEX_URL works fine. Unsure what remaining differences if any exist in request uv sends vs pip sends especially as uv is able to get index page successfully so I know authentication works for some requests.

---

_Comment by @charliermarsh on 2024-03-02 00:22_

Thank you! That's a bummer. So it's an _identical_ URL to what pip is requesting, right? My only remaining guess then is that we're losing the auth somewhere, and the server is responding with a 404.

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 00:33_

Yes the url is identical to pip based on breakpointing. Auth headers is my best guess and the headers being used for some requests (index fetch vs wheel fetch) are inconsistent. I see pip has PipSession object that manages auth and the wheel fetch does store the needed authentication tokens.

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 00:33_

If we could log the headers in error message used along with url I can compare exact headers pip uses vs uv.

edit: Here's pip's headers

```
{'User-Agent': 'pip/24.1.dev0 {"ci":null,"cpu":"x86_64","distro":{"name":"macOS","version":"14.3"},"implementation":{"name":"CPython","version":"3.9.16"},"installer":{"name":"pip","version":"24.1.dev0"},"openssl_version":"OpenSSL 1.1.1t  7 Feb 2023","python":"3.9.16","rustc_version":"1.76.0","setuptools_version":"69.0.3","system":{"name":"Darwin","release":"23.3.0"}}', 'Accept-Encoding': 'identity', 'Accept': '*/*', 'Connection': 'keep-alive', 'Authorization': 'Basic TENBdjE6ZXlKaGJHY2lPaUpGVXpJMU5pSXN...'}
```

I'm assuming authorization being key piece. The authorization header is same for both pip's wheel request and index request.

edit 2: Error message is also same as with fragment. So it's possible that fragment being present was a red herring for my index server and only auth headers is the issue. May still be worth for pip compatibility excluding the fragment.

---

_Comment by @charliermarsh on 2024-03-02 03:27_

Are the two URLs (for index fetch vs. wheel fetch) on different domains, or do they use different schemes, or anything like that?

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 04:06_

The first url is,

`https://registry.company-dev.net/python/virtual/lib-name/`

The second url is,

`https://registry.company-dev.net/python/virtual/lib-name/0.1.602402/lib-name-0.1.602402-py3-none-any.whl`

So the url is exact same. I may take a look tomorrow at uv closer, just not confident as it'll be my first time reading rust code and figuring out how to use a rust debugger.

edit: The index url if it helps looks like,

`https://LCAv1:SECURITY_TOKEN@registry.company-dev.net/python/virtual/`

---

_Comment by @charliermarsh on 2024-03-02 04:08_

Okay, thanks. And how do you typically provide the credentials? (We have this `safe_copy_url_auth` method that copies the credentials from one URL to the next if they are on the same "realm", which is why I asked about the domains.)

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 04:10_

The credentials are contained within the index-url. Not sure you saw the edit I made to show the index url.

---

_Comment by @charliermarsh on 2024-03-02 04:18_

Okay cool, yeah, I would expect this to work. I can push a branch that adds more logging for the headers and such tomorrow if you'd like. Would love to get this working for you since if you're running into this issue, I'm sure you won't be the only one.

---

_Comment by @hmc-cs-mdrissi on 2024-03-02 04:18_

A few other things is I am able to successfully curl the wheel fetch url with a simple command like,

```
curl -v -H "authorization: basic `echo -n "LCAv1:AUTH_TOKEN" | base64` https://registry.company-dev.net/python/virtual/lib-name/0.1.602402/lib-name-0.1.602402-py3-none-any.whl
```

So I think the only important thing is header being used and that index fetching successfully finding latest version feels like a sign that uv has the right authorization header for first fetch. The pip authorization header looks same as one in that curl command.

edit: More logging would be very helpful and will definitely try it.

---

_Comment by @charliermarsh on 2024-03-04 19:45_

@hmc-cs-mdrissi - What kind of logging would be most useful? Like, all headers?

---

_Comment by @hmc-cs-mdrissi on 2024-03-04 19:55_

Yes headers for each request sent. Most important two requests being index fetch + wheel fetch.

Alternatively if you could point me to the code where these requests are sent I could try to do some print debugging

---

_Comment by @pradyunsg on 2024-03-04 19:59_

> Do you know if this is encoded in any spec?

I just saw this.

I don't think it's written in any packaging spec but I'm pretty sure the hash fragment has to be dropped when querying the server since that's how URLs work? The fragment part is usually meant to reference something within the resource, eg anchors on HTML pages, and isn't sent to the server.

From https://url.spec.whatwg.org/#concept-url-fragment:

> A URL’s fragment is either null or an ASCII string that can be used for further processing on the resource the URL’s other components identify. 

I don't think I've ever seen someone send an HTTP request with # fragment in the path segment, and I'm pretty sure it'll fail for most servers.

I'm actually surprised if PyPI actually responds to a malformed request like that, if that is indeed what y'all are doing.


---

_Comment by @charliermarsh on 2024-03-04 20:14_

Honestly very possible that `reqwest` is dropping the fragments already.

---

_Comment by @pradyunsg on 2024-03-04 20:37_

Ah, yea, that makes sense! It would be a bit of a surprise if it wasn't. 😅

---

_Comment by @charliermarsh on 2024-03-04 20:46_

Merging a minimal version of this that just fixes one erroneous site.

---

_Renamed from "Drop hash fragment when downloading from registry" to "Set `.metadata` suffix on URL path" by @charliermarsh on 2024-03-04 20:47_

---

_Comment by @charliermarsh on 2024-03-04 20:47_

@hmc-cs-mdrissi -- Merging but I don't expect this to fix the problem here -- let's continue in the linked issue.

---

_Merged by @charliermarsh on 2024-03-04 20:51_

---

_Closed by @charliermarsh on 2024-03-04 20:51_

---

_Branch deleted on 2024-03-04 20:51_

---

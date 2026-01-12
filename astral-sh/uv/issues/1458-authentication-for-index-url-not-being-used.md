```yaml
number: 1458
title: Authentication for index-url not being used
type: issue
state: closed
author: inigohidalgo
labels:
  - bug
  - registry
assignees: []
created_at: 2024-02-16T09:07:46Z
updated_at: 2024-02-23T12:34:15Z
url: https://github.com/astral-sh/uv/issues/1458
synced_at: 2026-01-12T15:58:28Z
```

# Authentication for index-url not being used

---

_@inigohidalgo_

I am running 
```bash
uv pip install package-name --index-url https://username:personalaccesstoken@pkgs.dev.azure.com/[redacted]/pypi/simple/
```

It is being correctly parsed:

```
parse url=Url { scheme: "https", cannot_be_a_base: false, username: "username", password: Some("personalaccesstoken"), host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/[redacted]/pypi/simple/package-name/", query: None, fragment: None }
```


But then when it goes to query the index, it seems to not be using the user and password:

```
uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/[redacted]-py3-none-any.whl#sha256=[redacted]"
```

Which results in the error

```
Caused by: HTTP status client error (405 Method Not Allowed) for url (https://pkgs.dev.azure.com/[redacted]-py3-none-any.whl#sha256=[redacted])
```

---

_Comment by @inigohidalgo on 2024-02-16 09:22_

Just saw #1371, probably the same issue. I get the same 405, and my feed is also azure artifacts. Feel free to close in favor of that one.

Adding an additional datapoint since I just saw #1388

From what I can tell, that isn't the issue here, as the URLs queried from pip and from uv seem to be the same, except for the extra sha at the end which shouldn't make a difference.

uv

`https://pkgs.dev.azure.com/[my-org]/_packaging/[redacted]/pypi/download/pdm/2.12.3/pdm-2.12.3-py3-none-any.whl#sha256=27eddf71434906e39db3f448d35ea5ee1f4d0f557de39fc932205f7dfc82f902`

pip

`https://pkgs.dev.azure.com/[my-org]/_packaging/[redacted]/pypi/download/pdm/2.12.3/pdm-2.12.3-py3-none-any.whl`

---

_Comment by @inigohidalgo on 2024-02-16 10:14_

@VMRuiz what does your `PIP_EXTRA_INDEX_URL` look like? Do you use a personal access token or are you using keyring for authentication with the artifacts feed?

I get your `401 Unauthorized` when I pass the index_url like this `https://pkgs.dev.azure.com/[my-org]/_packaging/[my-feed]/pypi/simple/`

But when I supply the personal access token like in my OP, the error is the 405.

In your case, it seems to be failing at an earlier step (`process_request request=Prefetch pdm *`), which seems to be working fine when I supply the PAT, and instead it is failing in a later step, trying to download a specific wheel.

---

_Comment by @VMRuiz on 2024-02-16 10:29_

My PIP_EXTRA_INDEX_URL is like: `https://username:password@url` . However, I'm not using Azure but https://github.com/chriskuehl/dumb-pypi as PyPI index. Sorry for the confusion. 

---

_Comment by @datajoely on 2024-02-16 13:25_

Also seeing this with JFrog PyPI indexes

---

_Comment by @charliermarsh on 2024-02-16 14:21_

Thanks! Will take a look.

---

_Label `index` added by @zanieb on 2024-02-16 14:38_

---

_Label `bug` added by @zanieb on 2024-02-16 14:38_

---

_Comment by @mfurquimdev on 2024-02-16 15:31_

I think I'm having the same issues when trying to fetch packages from a private PyPI server. I get `401 Unauthorized` when using `--index-url`, even though `PYPI_USER` and `PYPI_PASS` are correctly being resolved.

I have nothing at `PIP_EXTRA_INDEX_URL`.

```bash
$ uv pip compile --verbose --index-url="https://${PYPI_USER}:${PYPI_PASS}@pypi.voltaware.com/simple" pyproject.toml --output-file=requirements.txt
[...]
uv_client::html::parse url=Url { scheme: "https", cannot_be_a_base: false, username: "correct-username", password: Some("correct-password"), host: Some(Domain("pypi.voltaware.com")), port: None, path: "/simple/[redacted]/", query: None, fragment: None }
[...]
error: Failed to download: [redacted]
  Caused by: The wheel [redacted]-py3-none-any.whl is not a valid zip file
  Caused by: an upstream reader returned an error: io error occurred: HTTP status client error (401 Unauthorized) for url (https://pypi.voltaware.com/packages/[redacted]-py3-none-any.whl#sha256=[redacted])
  Caused by: io error occurred: HTTP status client error (401 Unauthorized) for url (https://pypi.voltaware.com/packages/[redacted]-py3-none-any.whl#sha256=[redacted])
  Caused by: HTTP status client error (401 Unauthorized) for url (https://pypi.voltaware.com/packages/[redacted]-py3-none-any.whl#sha256=[redacted])
```

---

_Assigned to @zanieb by @zanieb on 2024-02-16 21:57_

---

_Comment by @J3ronimo on 2024-02-17 10:48_

Same here. I have user and token in a Gitlab url passed with `uv pip install --extra-index-url ...`, which makes it find the package (so the credentials must be OK). but then fails with 

```
error: Failed to download: ...
  Caused by: HTTP status client error (401 Unauthorized) for url {url}
```

where `url` is missing the url-encoded credentials now. Can't say if they were sent in HTTP headers though.

---

_Comment by @olivierlefloch on 2024-02-19 00:09_

I believe the issue for Azure Artifacts is that `uv` runs `HEAD` requests, which Azure Artifacts does not seem to allow. I've confirmed this behavior via proxying of `uv` requests / replaying via `curl`. Authenticated `GET` requests with `--index-url` specified work, but `HEAD` requests are failing. Here's a sample session:

```
❯ python3 -m uv pip install --index-url=https://REDACTED:REDACTED@pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/simple/ --upgrade --verbose -r requirements/dev.txt 2>&1 | grep private-package
        0.131560s   0ms DEBUG uv_resolver::resolver Adding direct dependency: private-package==REDACTED
 uv_resolver::resolver::process_request request=Versions private-package
   uv_client::registry_client::simple_api package=private-package
       uv_client::cached_client::read_and_parse_cache file=/Users/olivierlefloch/Library/Caches/uv/simple-v1/REDACTED/private-package.REDACTED
        0.133447s   2ms DEBUG uv_resolver::resolver Adding direct dependency: private-package==REDACTED
          0.133694s   0ms WARN uv_client::cached_client Broken cache entry at /Users/olivierlefloch/Library/Caches/uv/simple-v1/REDACTED/private-package.REDACTED, removing: failed to open file `/Users/olivierlefloch/Library/Caches/uv/simple-v1/REDACTED/private-package.REDACTED`
          0.136200s   2ms DEBUG uv_client::cached_client No cache entry for: https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/simple/private-package/
       uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/simple/private-package/"
        0.635864s 504ms DEBUG uv_resolver::resolver Adding direct dependency: private-package==REDACTED
       uv_client::cached_client::new_cache file=/Users/olivierlefloch/Library/Caches/uv/simple-v1/REDACTED/private-package.REDACTED
       uv_client::registry_client::parse_simple_api package=private-package
         uv_client::html::parse url=https://REDACTED:REDACTED@pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/simple/private-package/
 uv_resolver::resolver::process_request request=Prefetch private-package ==REDACTED
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=private-package==REDACTED
     uv_client::registry_client::wheel_metadata built_dist=private-package==REDACTED
           uv_client::cached_client::read_and_parse_cache file=/Users/olivierlefloch/Library/Caches/uv/wheels-v0/index/REDACTED/private-package/private_package-REDACTED-py3-none-any.msgpack
              0.756403s   0ms WARN uv_client::cached_client Broken cache entry at /Users/olivierlefloch/Library/Caches/uv/wheels-v0/index/REDACTED/private-package/private_package-REDACTED-py3-none-any.msgpack, removing: failed to open file `/Users/olivierlefloch/Library/Caches/uv/wheels-v0/index/REDACTED/private-package/private_package-REDACTED-py3-none-any.msgpack`
              0.756439s   0ms DEBUG uv_client::cached_client No cache entry for: https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/REDACTED/private_package-REDACTED-py3-none-any.whl#sha256=REDACTED
           uv_client::cached_client::fresh_request url="https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/REDACTED/private_package-REDACTED-py3-none-any.whl#sha256=REDACTED"
error: Failed to download: private-package==REDACTED
  Caused by: HTTP status client error (405 Method Not Allowed) for url (https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/REDACTED/private_package-REDACTED-py3-none-any.whl#sha256=REDACTED)
```

The failed HEAD request looks like:

```
HEAD https://pkgs.dev.azure.com/REDACTED/_packaging/REDACTED/pypi/download/private-package/1.2.3/private-package-1.2.3-cp310-cp310-macosx_11_0_arm64.whl
```

I _imagine_ that the `HEAD` call in `wheel_metadata_no_pep658` here:

https://github.com/astral-sh/uv/blob/main/crates/uv-client/src/registry_client.rs#L441

might be related, with the error handling in `is_http_range_requests_unsupported` ( https://github.com/astral-sh/uv/blob/4b92a512189e35c86491221ccc14c2e951936c4e/crates/uv-client/src/error.rs#L160 ) perhaps not covering this case? I was unable to find relevant Azure Artifacts documentation to explain / justify this behavior.

It seems there may be a couple of other issues reported in this thread however. I'd suggest splitting the conversation here perhaps, between Azure Artifacts specific issues and the perhaps more general `401 / Auth` issues others seem to be encountering.

---

_Comment by @zanieb on 2024-02-19 00:15_

Thanks for the information! cc @charliermarsh who was just working with this code.

---

_Comment by @inigohidalgo on 2024-02-19 10:12_

> there may be a couple of other issues reported in this thread however
@olivierlefloch 

Yeah, I've seen the two different error codes:
-  405, which I haven't seen reported by anybody not using Azure Artifacts
- and a 401 when I simply did not pass authentication https://github.com/astral-sh/uv/issues/1458#issuecomment-1948100077

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-19 14:42_

---

_Unassigned @zanieb by @charliermarsh on 2024-02-19 14:42_

---

_Comment by @charliermarsh on 2024-02-19 14:42_

Ahh thank you, ok, I can take this one.

---

_Comment by @olivierlefloch on 2024-02-19 19:10_

@charliermarsh after working around the 405 issue on Azure Artifacts, I'm getting a 401 error:

https://github.com/astral-sh/uv/pull/1713

---

_Unassigned @charliermarsh by @charliermarsh on 2024-02-19 20:48_

---

_Comment by @charliermarsh on 2024-02-19 20:48_

I'm gonna merge this into https://github.com/astral-sh/uv/issues/1371.

---

_Closed by @charliermarsh on 2024-02-19 20:48_

---

```yaml
number: 3205
title: Huge slowdown when using keyring(google artifact registry) in 0.1.34/35 and auth problem in 0.1.36
type: issue
state: closed
author: avelychko12
labels:
  - bug
assignees: []
created_at: 2024-04-23T12:16:35Z
updated_at: 2024-04-24T19:27:38Z
url: https://github.com/astral-sh/uv/issues/3205
synced_at: 2026-01-10T05:31:37Z
```

# Huge slowdown when using keyring(google artifact registry) in 0.1.34/35 and auth problem in 0.1.36

---

_Issue opened by @avelychko12 on 2024-04-23 12:16_

I have a virtualenv where I already have `requests` installed from default pypi, and if I don't use any extra index url then everything is fine, with extra index url with token everything is fine as well
```
$ uv pip install -U requests
Resolved 5 packages in 3ms
Audited 5 packages in 0.11ms
$ export UV_EXTRA_INDEX_URL="https://oauth2accesstoken:$(gcloud auth print-access-token)@europe-python.pkg.dev/project-id/pypi/simple/"
$ uv pip install -U requests
Resolved 5 packages in 304ms
Audited 5 packages in 0.06ms
```
but with keyring it's like x3 slower than in 0.1.33
```
$ export UV_KEYRING_PROVIDER=subprocess
$ export UV_EXTRA_INDEX_URL=https://oauth2accesstoken@europe-python.pkg.dev/project-id/pypi/simple/
$ pip install uv==0.1.33
...
$ uv pip install -U requests
Resolved 5 packages in 1.47s
Audited 5 packages in 0.07ms
$ pip install uv==0.1.34
...
$ uv pip install -U requests
Resolved 5 packages in 4.37s
Audited 5 packages in 0.12ms
$ pip install uv==0.1.35
...
$ uv pip install -U requests
Resolved 5 packages in 4.32s
Audited 5 packages in 0.08ms
```
and it doesn't work with 0.1.36
```
$ pip install uv==0.1.36
...
$ uv pip install -U requests
error: HTTP status client error (401 Unauthorized) for url (https://europe-python.pkg.dev/project-id/pypi/simple/requests/)
```
keyring version: `keyring==25.1.0 keyrings-google-artifactregistry-auth==1.1.2`


---

_Renamed from "Huge slowdown when using keyring(google artifact registry) in 0.1.34/35 and incorrect resolution in 0.1.36" to "Huge slowdown when using keyring(google artifact registry) in 0.1.34/35 and auth problem in 0.1.36" by @avelychko12 on 2024-04-23 12:33_

---

_Comment by @zanieb on 2024-04-23 12:56_

Thanks for the issue! We made a bunch of changes to authentication in #2976 and #3130 to ensure correctness but these come with some performance cost. Regardless it shouldn't be this slow, I'll investigate that.

Sorry to see it works in v0.1.35 and doesn't in v0.1.36, can you provide `RUST_LOG=uv=trace uv pip install -v ...` logs?

---

_Label `bug` added by @zanieb on 2024-04-23 12:56_

---

_Assigned to @zanieb by @zanieb on 2024-04-23 12:56_

---

_Comment by @jenshnielsen on 2024-04-23 13:50_

I am seeing a similar issue with Azure artifacts. Installation worked with 0.1.35 but has broken in 0.1.36. As far as I can see the keyring is never accessed and only unauthorized requests are attempted. 

---

_Comment by @zanieb on 2024-04-23 13:52_

I've put up a fix at #3206.

Will investigate the performance problems separately.

---

_Comment by @avelychko12 on 2024-04-23 14:20_

> Sorry to see it works in v0.1.35 and doesn't in v0.1.36, can you provide `RUST_LOG=uv=trace uv pip install -v ...` logs?

@zanieb do you still need logs? If so, for which version

Also, I have done `uv pip install -vv -U requests` in 0.1.35 and what I can see is that it makes keyring subprocess call for each package
```
...
uv_resolver::resolver::process_request request=Prefetch charset-normalizer >=2, <4
         0.995075s   0ms DEBUG uv_client::cached_client No cache entry for: https://europe-python.pkg.dev/<project>/pypi/simple/charset-normalizer/
      uv_client::cached_client::fresh_request url="https://europe-python.pkg.dev/<project>/pypi/simple/charset-normalizer/"
           0.995103s   0ms DEBUG uv_auth::middleware Checking keyring for credentials for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/charset-normalizer/
        uv_auth::keyring::fetch_subprocess self=KeyringProvider { cache: Mutex { data: <locked>, poisoned: false, .. }, backend: Subprocess }, service_name="https://europe-python.pkg.dev/<project>/pypi/simple/charset-normalizer/", username="oauth2accesstoken"
uv_client::cached_client::from_path_sync path="/home/anton/.cache/uv/simple-v7/e09090b474a8bf5c/certifi.rkyv"
           1.752990s 757ms DEBUG uv_auth::middleware Found credentials in keyring for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/charset-normalizer/
         1.753103s 758ms DEBUG uv_client::cached_client No cache entry for: https://europe-python.pkg.dev/<project>/pypi/simple/idna/
      uv_client::cached_client::fresh_request url="https://europe-python.pkg.dev/<project>/pypi/simple/idna/"
           1.753160s   0ms DEBUG uv_auth::middleware Checking keyring for credentials for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/idna/
        uv_auth::keyring::fetch_subprocess self=KeyringProvider { cache: Mutex { data: <locked>, poisoned: false, .. }, backend: Subprocess }, service_name="https://europe-python.pkg.dev/<project>/pypi/simple/idna/", username="oauth2accesstoken"
           2.496826s 743ms DEBUG uv_auth::middleware Found credentials in keyring for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/idna/
         2.496927s   1s  DEBUG uv_client::cached_client No cache entry for: https://europe-python.pkg.dev/<project>/pypi/simple/urllib3/
      uv_client::cached_client::fresh_request url="https://europe-python.pkg.dev/<project>/pypi/simple/urllib3/"
           2.496952s   0ms DEBUG uv_auth::middleware Checking keyring for credentials for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/urllib3/
        uv_auth::keyring::fetch_subprocess self=KeyringProvider { cache: Mutex { data: <locked>, poisoned: false, .. }, backend: Subprocess }, service_name="https://europe-python.pkg.dev/<project>/pypi/simple/urllib3/", username="oauth2accesstoken"
           3.241410s 744ms DEBUG uv_auth::middleware Found credentials in keyring for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/urllib3/
         3.241505s   2s  DEBUG uv_client::cached_client No cache entry for: https://europe-python.pkg.dev/<project>/pypi/simple/certifi/
      uv_client::cached_client::fresh_request url="https://europe-python.pkg.dev/<project>/pypi/simple/certifi/"
           3.241533s   0ms DEBUG uv_auth::middleware Checking keyring for credentials for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/certifi/
        uv_auth::keyring::fetch_subprocess self=KeyringProvider { cache: Mutex { data: <locked>, poisoned: false, .. }, backend: Subprocess }, service_name="https://europe-python.pkg.dev/<project>/pypi/simple/certifi/", username="oauth2accesstoken"
           4.001753s 760ms DEBUG uv_auth::middleware Found credentials in keyring for https://oauth2accesstoken@europe-python.pkg.dev/<project>/pypi/simple/certifi/
    uv_client::cached_client::get_cacheable 
      uv_client::cached_client::read_and_parse_cache file=/home/anton/.cache/uv/simple-v7/pypi/charset-normalizer.rkyv
...
``` 


---

_Comment by @zanieb on 2024-04-23 14:23_

@avelychko12 thanks for the logs. I believe all this should be addressed by #3206. I'll release that now.

---

_Comment by @avelychko12 on 2024-04-23 16:18_

So, I tried 0.1.37 and problem from 0.1.36 is gone, thx.
But performance is a little bit tricky, because for all project dependencies(>200 from pip freeze) `uv pip install -Ur req.txt`  working for 10s in 0.1.37, but it was 4s in 0.1.33. And during installation all of my 16 CPU cores were fully utilized running python commands for the keyring.  

---

_Comment by @zanieb on 2024-04-23 17:13_

Thanks I'm looking into improving performance further. 

---

_Comment by @zanieb on 2024-04-23 17:15_

Unfortunately the previous implementation had some correctness issues so we may not reach that level of performance with the keyring subprocess.

---

_Comment by @vlad-ivanov-name on 2024-04-24 13:46_

One way to improve performance is to allow custom keyring command. Already now one could probably just substitute `keyring` on the PATH with something faster

---

_Closed by @zanieb on 2024-04-24 14:53_

---

_Comment by @avelychko12 on 2024-04-24 19:27_

> Unfortunately the previous implementation had some correctness issues so we may not reach that level of performance with the keyring subprocess.

0.1.38 works as fast as 0.1.33 (If you already have the latest versions installed, at least)
Thanks a lot 

---

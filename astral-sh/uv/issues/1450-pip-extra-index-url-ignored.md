---
number: 1450
title: PIP_EXTRA_INDEX_URL ignored
type: issue
state: closed
author: gwdekker
labels:
  - compatibility
assignees: []
created_at: 2024-02-16T08:14:58Z
updated_at: 2024-03-06T08:25:34Z
url: https://github.com/astral-sh/uv/issues/1450
synced_at: 2026-01-10T01:23:06Z
---

# PIP_EXTRA_INDEX_URL ignored

---

_Issue opened by @gwdekker on 2024-02-16 08:14_

Trying to run `uv pip compile` as a direct replacement for `python -m piptools compile`, it seems that the environment variable I have set for `PIP_EXTRA_INDEX_URL` is not picked up yet by `uv`. 


---

_Label `compatibility` added by @MichaReiser on 2024-02-16 08:26_

---

_Comment by @VMRuiz on 2024-02-16 09:02_

Also, using `uv pip compile --extra-index-url $PIP_EXTRA_INDEX_URL` doesn't work either if the URL contains credentials like: `https://username:password@example.com`. 

```
error: Failed to download: private-package==0.0.1
  Caused by: HTTP status client error (401 Unauthorized) for url (https://example.com/packages/...)
```

Please let me know if this ticket is a good place to raise this or if I shall open a new one.

---

_Comment by @MichaReiser on 2024-02-16 09:13_

@VMRuiz this sounds like https://github.com/astral-sh/uv/issues/1458 ?

---

_Comment by @VMRuiz on 2024-02-16 09:31_

Yes, it seems like the same issue with a different error code. 

---

_Comment by @charliermarsh on 2024-02-16 15:24_

I don't love reading environment variables that are specifically intended for other tools. It's similar to reading configuration that's intended for other tools -- it can cause problems if they change their own format. I'd prefer to have `UV_EXTRA_INDEX_URL`.

---

_Assigned to @zanieb by @zanieb on 2024-02-16 17:04_

---

_Referenced in [astral-sh/uv#1515](../../astral-sh/uv/pulls/1515.md) on 2024-02-16 17:04_

---

_Comment by @gwdekker on 2024-02-16 18:49_

That makes sense! Breaks the “drop in replacement” but only a tiny bit and for a very good reason. Thanks for the quick follow up and for this amazing package, can hardly wait to use it!

---

_Closed by @zanieb on 2024-02-16 18:54_

---

_Comment by @Pajinek on 2024-02-27 12:14_

It looks that "--index-url" and "--no-build" is not support in requirements file. For example:

```
# file requirements.txt
--extra-index-url https://pypi.example.org/pypi
--no-binary  my-package1

aiohttp==3.9.1
aioredis==2.0.1
...
```


---

_Comment by @charliermarsh on 2024-02-27 13:52_

Index URL is supported, no-build is not. This is covered in other issues.

---

_Comment by @Pajinek on 2024-03-01 12:33_

I am not sure that extra-index-url works good, because that tries to install packages only from extra-index-url

```>> uv pip install -r requirements.txt 
error: HTTP status client error (401 Unauthorized) for url (https://pypi.mysite.com/pypi/httpcore/)
)
```

If I use pip install -r requirements.txt then all packages are installed without problem.

```
# requirements.txt 
--extra-index-url https://pypi.mysite.com/pypi

aiofiles==0.8.0
    # via gcloud-aio-storage
aiohttp==3.9.3
    # via
    #   -r requirements.in
```


---

_Comment by @charliermarsh on 2024-03-01 13:59_

Do you know why your index is returning a 401? If it returns an error code like 404, we would continue to search PyPI.

---

_Comment by @Pajinek on 2024-03-06 08:25_

Hi, I am not sure, but it works with original pip. 

```
>> pip install -i https://pypi.example.com/simple/ aiohttp --verbose 
Looking in indexes: https://pypi.example.com/simple/
Collecting aiohttp
 Downloading https://pypi.example.com/api/package/aiohttp/aiohttp-3.9.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.3 MB)
```

I tried to download package by wget manually and it works as well. 

```
>> wget https://pypi.example.com/api/package/aiohttp/aiohttp-3.9.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Connecting to pypi.example.com (pypi.example.com)|...|:443... connected.
HTTP request sent, awaiting response... 401 Unauthorized
Authentication selected: Basic realm="pypi"
Reusing existing connection to pypi.example.com:443.
HTTP request sent, awaiting response... 302 Found
Location: https://storage.googleapis.com/...
...
HTTP request sent, awaiting response... 200 OK
aiohttp-3.9.3-cp312-cp312-manylinux_2_17_x86 100%[============================================================================================>]   1.25M  --.-KB/s    in 0.05s   

```

---

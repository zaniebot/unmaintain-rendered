```yaml
number: 915
title: Cannot resolve shared object python modules
type: issue
state: closed
author: seysn
labels: []
assignees: []
created_at: 2025-07-30T11:57:28Z
updated_at: 2025-07-30T12:27:30Z
url: https://github.com/astral-sh/ty/issues/915
synced_at: 2026-01-10T02:06:24Z
```

# Cannot resolve shared object python modules

---

_Issue opened by @seysn on 2025-07-30 11:57_

### Summary

We can reproduce this with `netifaces` package which only have a `netifaces.cpython-313-x86_64-linux-gnu.so` file inside `site-packages` :

```
# Adding requests to make sure it's not an environment issue
$ uv add requests netifaces

$ ls .venv/lib/python3.13/site-packages/
__pycache__                  charset_normalizer-3.4.2.dist-info         requests-2.32.4.dist-info
_virtualenv.pth              idna                                       urllib3
_virtualenv.py               idna-3.10.dist-info                        urllib3-2.5.0.dist-info
certifi                      netifaces-0.11.0.dist-info
certifi-2025.7.14.dist-info  netifaces.cpython-313-x86_64-linux-gnu.so
charset_normalizer           requests

$ cat main.py
import requests
import netifaces

print(netifaces.interfaces())
print(requests.get("http://httpbin.org/ip"))

$ python main.py
['lo', 'enp0s31f6', 'wlp0s20f3', 'docker0']
<Response [200]>

$ ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unresolved-import]: Cannot resolve imported module `netifaces`
 --> main.py:2:8
  |
1 | import requests
2 | import netifaces
  |        ^^^^^^^^^
3 |
4 | print(netifaces.interfaces())
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

As you can see, the script is working fine and requests is correctly found, but not netifaces.

### Version

ty 0.0.1-alpha.16

---

_Comment by @AlexWaygood on 2025-07-30 12:01_

Thanks for the report. It looks like this might be a duplicate of https://github.com/astral-sh/ty/issues/487?

---

_Comment by @seysn on 2025-07-30 12:11_

Ah yes I guess you are right, I didn't search with the right terms, sorry.

Also like `types-lxml` in this issue, there's `types-netifaces` which also solve this.

I'll close this issue to avoid duplicates, thanks

---

_Closed by @seysn on 2025-07-30 12:11_

---

_Comment by @AlexWaygood on 2025-07-30 12:12_

> Ah yes I guess you are right, I haven't search with the right terms, sorry.

Oh, don't be sorry -- finding duplicates on GitHub is really difficult. The issue search is terrible :-)

Plus, we know it's really hard to know what terms to search for when it comes to type-checker issues

---

```yaml
number: 14680
title: uvx does not find newly published package version on private index
type: issue
state: closed
author: dev10231235
labels:
  - question
assignees: []
created_at: 2025-07-17T12:21:59Z
updated_at: 2025-08-22T16:00:21Z
url: https://github.com/astral-sh/uv/issues/14680
synced_at: 2026-01-12T16:01:54Z
```

# uvx does not find newly published package version on private index

---

_@dev10231235_

### Summary

I maintain a private pypi tracker - essentially a Caddy http server. 
I publish my private tool on that server - literally just scp of dist/* content to a specific folder.

```bash
uvx --extra-index-url https://user:password@private.tracker.com/artifacts/pypi/simple/ --from "my-admin-tools==0.1.0" my_tool1 --help
```

This works - takes correct version and prints help.

Then, I make a change and publish new version - 0.1.1.

Now I run this - and it prints me this:
```bash
uvx --extra-index-url https://user:password@private.tracker.com/artifacts/pypi/simple/  --from "my-admin-tools==0.1.1" my_tool1 --help

    × No solution found when resolving tool dependencies:                                                                                                            
    ╰─▶ Because there is no version of my-admin-tools==0.1.1 and you require                                                                                         
        my-admin-tools==0.1.1, we can conclude that your requirements are                                                                                            
        unsatisfiable.                                                                                                                                               
        hint: `my-admin-tools` was found on                                                                                                                          
        https://user:password@private.tracker.com/artifacts/pypi/simple/,                                                                                 
        but not at the requested version (my-admin-tools==0.1.1). A                                                                                                  
        compatible version may be available on a subsequent index (e.g.,                                                                                             
        https://pypi.org/simple). By default, uv will only consider versions                                                                                         
        that are published on the first index that contains a given package, to                                                                                      
        avoid dependency confusion attacks. If all indexes are equally trusted,                                                                                      
        use `--index-strategy unsafe-best-match` to consider all versions from                                                                                       
        all indexes, regardless of the order in which they were defined.                                                                                             

```

Okay, I add this flag `--index-strategy unsafe-best-match` to my tool call:

```bash
uvx --extra-index-url https://user:password@private.tracker.com/artifacts/pypi/simple/  --index-strategy unsafe-best-match --from "my-admin-tools==0.1.1" my_tool1 --help
```

This now works.

Publish new version - 0.1.2.

```bash
uvx --extra-index-url https://user:password@private.tracker.com/artifacts/pypi/simple/  --index-strategy unsafe-best-match --from "my-admin-tools==0.1.2" my_tool1 --help

    × No solution found when resolving tool dependencies:                                                                                                            
    ╰─▶ Because there is no version of my-admin-tools==0.1.2 and you require                                                                                         
        my-admin-tools==0.1.2, we can conclude that your requirements are                                                                                            
        unsatisfiable.
```

At this point I executed `uv self update` and this updated `uv` to 0.7.21 and my `uvx` tool worked.



### Platform

Ubuntu 22.04

### Version

0.6.6

### Python version

_No response_

---

_Label `bug` added by @dev10231235 on 2025-07-17 12:22_

---

_Comment by @zanieb on 2025-07-17 12:29_

Are you running into caching of the response from the API? It can take ~10 minutes for a typical index cache to expire. You can use `uvx package@latest` to refresh the cache, or `uvx --refresh-package package  package==version`  

---

_Label `bug` removed by @zanieb on 2025-07-17 12:29_

---

_Label `question` added by @zanieb on 2025-07-17 12:29_

---

_Comment by @zanieb on 2025-07-17 12:30_

Do you know what cache headers your HTTP server is returning for its simple API? 

---

_Comment by @dev10231235 on 2025-07-17 12:58_

I am sure that is not HTTP server caching issue. Caddy is configured like this. 
UPD: this belief is based on a fact that I can open URL in browser and see new version immediately.

```
mydomain {
      handle {
            root * /srv/static

            basic_auth {
                XXXXXXXXXXXXXXXXXXXXXXXXXX
            }
            file_server browse
            encode gzip

            # Basic security headers
            header {
                # Enable HSTS
                Strict-Transport-Security "max-age=31536000; includeSubDomains"
                # Prevent browsers from MIME-sniffing
                X-Content-Type-Options "nosniff"
                # XSS protection
                X-XSS-Protection "1; mode=block"
                # Deny iframe embedding
                X-Frame-Options "DENY"
            }
        }
}
```

I will try with `--refresh-package package` thanks. 

I am confident it is some kind of caching issue, but to me it looks like it its on uvx side. 

---

_Closed by @charliermarsh on 2025-08-22 16:00_

---

---
number: 705
title: "Support for index without `simple` API"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-12-19T18:08:02Z
updated_at: 2023-12-25T00:26:54Z
url: https://github.com/astral-sh/uv/issues/705
synced_at: 2026-01-10T01:23:04Z
---

# Support for index without `simple` API

---

_Issue opened by @zanieb on 2023-12-19 18:08_

With `devpi`, when we make a request for a project at `/<project>` instead of `/+simple/<project>` we get a HTTP 404 because we only accept `application/vnd.pypi.simple.v1+json` and their API returns `application/json` or `text/html` for those endpoints. `pip` will happily install from here, but `puffin pip-install` fails. 

This may be solved by adding support for the `text/html` API? as in #412? but I believe that is specific to the HTML responses from the `simple` API not this format.

For posterity, if we accept `application/json` the server responds with e.g.

```
{
  "result": {
    "0.0.0": {
      "name": "requires-direct-incompatible-versions-3a64108d",
      "version": "0.0.0",
      "summary": "The user requires two incompatible, existing versions of package `a`",
      "home_page": "",
      "author": "",
      "author_email": "",
      "maintainer": "",
      "maintainer_email": "",
      "license": "",
      "description": "",
      "keywords": "",
      "platform": [],
      "classifiers": [],
      "download_url": "",
      "supported_platform": [],
      "comment": "",
      "provides": [],
      "requires": [],
      "obsoletes": [],
      "project_urls": [],
      "provides_dist": [],
      "obsoletes_dist": [],
      "requires_dist": [
        "requires-direct-incompatible-versions-3a64108d-a==1.0.0",
        "requires-direct-incompatible-versions-3a64108d-a==2.0.0"
      ],
      "requires_external": [],
      "requires_python": ">=3.7",
      "description_content_type": "",
      "provides_extras": "",
      "+links": [
        {
          "rel": "releasefile",
          "hash_spec": "sha256=fe998dba25fb844fb1c3399f11fcd93642b40be7166f1db46ca5ae1bfa8f5017",
          "href": "http://localhost:3141/packages/local/+f/fe9/98dba25fb844f/requires_direct_incompatible_versions_3a64108d-0.0.0.tar.gz",
          "log": [
            {
              "what": "upload",
              "who": null,
              "when": [
                2023,
                12,
                19,
                16,
                27,
                9
              ],
              "dst": "packages/local"
            }
          ]
        }
      ]
    }
  },
  "type": "projectconfig"
}
```

---

_Comment by @zanieb on 2023-12-19 18:09_

xrefs
- https://github.com/devpi/devpi/issues/986
- https://github.com/devpi/devpi/issues/801
- https://github.com/dependabot/dependabot-core/issues/7680
- https://github.com/devpi/devpi/issues/1020

---

_Comment by @charliermarsh on 2023-12-21 03:17_

@konstin or @zanieb - Can you help me understand what the `/<project>` URL is vs. the `/simple` version?

---

_Comment by @konstin on 2023-12-21 09:47_

For pypi for example, it's `https://pypi.org/simple/` (list of all projects) vs `https://pypi.org/simple/<project>` (files for a single project)

---

_Comment by @zanieb on 2023-12-21 14:38_

Hm that's not what I'm referring to. There's the Simple API which provides `/simple` and `/simple/<project>` but as shown for `devpi` there's also just `/<project>` which has a different response. I cannot find a standard for that page.

---

_Comment by @charliermarsh on 2023-12-21 14:43_

(Not sure that I want to support this but still curious what it is and if it follows a spec.)

---

_Comment by @zanieb on 2023-12-21 16:45_

I'll try to see if there are other indexes that provide the same.

---

_Comment by @konstin on 2023-12-21 16:53_

I see the torch and jax indexes (https://download.pytorch.org/whl/cu118 and https://storage.googleapis.com/jax-releases/jax_cuda_releases.html) as important targets, they are foundational machine learning libraries with both having only limited pypi support.

---

_Comment by @charliermarsh on 2023-12-24 21:49_

I wonder if this is just something internal to devpi. Tempted to close until we see it elsewhere.

---

_Comment by @zanieb on 2023-12-25 00:26_

Sounds good to me in conjunction with #706 

---

_Closed by @zanieb on 2023-12-25 00:26_

---

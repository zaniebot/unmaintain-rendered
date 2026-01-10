---
number: 12425
title: "```uv pip install``` with ```--find-links``` and ```--extra-index-url```"
type: issue
state: open
author: jmspereira
labels:
  - question
  - cache
assignees: []
created_at: 2025-03-24T10:52:16Z
updated_at: 2025-04-15T12:08:36Z
url: https://github.com/astral-sh/uv/issues/12425
synced_at: 2026-01-10T01:25:19Z
---

# ```uv pip install``` with ```--find-links``` and ```--extra-index-url```

---

_Issue opened by @jmspereira on 2025-03-24 10:52_

### Question

Hey everyone,
First of all, I am sorry if this is duplicated from some existing issue. I couldn't find one with this question. 

I have a wheel of a library, let's call it abc-1.2.3 both in a dist/ folder that I pass with --find-links parameter. However, the same wheel (with the same version) exists in a private pypi which is passed in the --extra-index-url. From my experiments, it appears that uv prefers the wheel existing in repository passed by the --extra-index-url instead of the one in the path passed in --find-links directory.

Is this the desired behavior? Or I am doing something wrong? Is there any way of prefer the wheel existing on the local /dist folder?

Thanks! 


### Platform

Ubuntu 24.04

### Version

0.6.7

---

_Label `question` added by @jmspereira on 2025-03-24 10:52_

---

_Renamed from "``uv pip install``` with ```--find-links``` and ```--extra-index-url```" to "```uv pip install``` with ```--find-links``` and ```--extra-index-url```" by @jmspereira on 2025-03-24 10:52_

---

_Comment by @jmspereira on 2025-03-24 11:40_

Ok, I think I was wrong. The wheel from dist/ folder is installed. The problem that I was having was related to the cache.
This happened during the build of a docker image where:

1) I was building abc-1.2.3 and placing it inside dist/
2) Installing with other requirements passing both --find-links and --extra-index-url flags as mentioned.

However, the changes made locally were not reflected in the installed version. It seems that uv pip install command is not able to tell that the wheel changed.

I was able to obtain the desired behavior by passing the --refresh option to the pip install command. 
Is this the correct solution?

---

_Label `cache` added by @zanieb on 2025-04-01 18:22_

---

_Assigned to @konstin by @zanieb on 2025-04-01 18:23_

---

_Unassigned @konstin by @konstin on 2025-04-15 11:47_

---

_Comment by @konstin on 2025-04-15 12:08_

This is an edge case with `--find-links`, since find links URLs are considered an index, and uv assumes the files in an index are assumed to be static. To ensure a file is not cached, with `--find-links`, you can pass `--reinstall`

---

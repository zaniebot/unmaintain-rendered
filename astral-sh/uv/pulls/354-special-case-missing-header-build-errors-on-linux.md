```yaml
number: 354
title: Special case missing header build errors (on linux)
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: special-case-missing-header
created_at: 2023-11-07T13:15:47Z
updated_at: 2023-11-08T15:26:41Z
url: https://github.com/astral-sh/uv/pull/354
synced_at: 2026-01-12T16:03:54Z
```

# Special case missing header build errors (on linux)

---

_@konstin_

One of the most common errors i observed are build failures due to missing header files. On ubuntu, this generally means that you need to install some `<...>-dev` package that the documentation tells you about, e.g. [mysqlclient](https://github.com/PyMySQL/mysqlclient#linux) needs `default-libmysqlclient-dev`, [some psycopg versions](https://www.psycopg.org/psycopg3/docs/basic/install.html#local-installation) (i remember that this was always required at some earlier point) require `libpq-dev` and pygraphviz wants `graphviz-dev`. This is quite common for many scientific packages (where conda has an advantage because they can provide those package as a dependency).

The error message can be completely inscrutable if you're just a python programmer (or user) and not a c programmer (example: pygraphviz):

```
warning: no files found matching '*.png' under directory 'doc'
warning: no files found matching '*.txt' under directory 'doc'
warning: no files found matching '*.css' under directory 'doc'
warning: no previously-included files matching '*~' found anywhere in distribution
warning: no previously-included files matching '*.pyc' found anywhere in distribution
warning: no previously-included files matching '.svn' found anywhere in distribution
no previously-included directories found matching 'doc/build'
pygraphviz/graphviz_wrap.c:3020:10: fatal error: graphviz/cgraph.h: No such file or directory
 3020 | #include "graphviz/cgraph.h"
      |          ^~~~~~~~~~~~~~~~~~~
compilation terminated.
error: command '/usr/bin/gcc' failed with exit code 1
```

The only relevant part is `Fatal error: graphviz/cgraph.h: No such file or directory`. Why is this file not there and how do i get it to be there?

This is even harder to spot in pip's output, where it's 11 lines above the last line:

![image](https://github.com/astral-sh/puffin/assets/6826232/7a3d7279-e7b1-4511-ab22-d0a35be5e672)

I've special cased missing headers and made sure that the last line tells you the important information: We're missing some header, please check the documentation of {package} {version} for what to install:

![image](https://github.com/astral-sh/puffin/assets/6826232/4bbb8923-5a82-472f-ab1f-9e1471aa2896)

Scrolling up:

![image](https://github.com/astral-sh/puffin/assets/6826232/89a2495a-e188-4288-b534-ad885ee08763)

The difference gets even clearer with a default ubuntu terminal with its 80 columns:

![image](https://github.com/astral-sh/puffin/assets/6826232/49fb27bc-07c6-4b10-a1a1-30ec8e112438)

---

Note that the situation is better for a missing compiler, there i get:

```
[...]
warning: no previously-included files matching '*~' found anywhere in distribution
warning: no previously-included files matching '*.pyc' found anywhere in distribution
warning: no previously-included files matching '.svn' found anywhere in distribution
no previously-included directories found matching 'doc/build'
error: command 'gcc' failed: No such file or directory
---
```
Putting the last line into google, the first two results tell me to `sudo apt-get install gcc`, the third even tells me about `sudo apt install build-essential`


---

_Renamed from "Special case missing header build errors" to "Special case missing header build errors (on linux)" by @konstin on 2023-11-07 13:21_

---

_Comment by @konstin on 2023-11-07 14:30_

For comparison, here's a rust build error i just had:

![image](https://github.com/astral-sh/puffin/assets/6826232/02e9a59b-2e18-43fc-b475-f0148812eb9f)


---

_@zanieb reviewed on 2023-11-07 14:39_

Cool! These are indeed often inscrutable.

---

_@charliermarsh reviewed on 2023-11-07 15:19_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/distribution/source_distribution.rs`:147 on 2023-11-07 15:19_

I don't think we should use `distribution.id()` here since it's just a hash of the URL for URL dependencies, right?

---

_@konstin reviewed on 2023-11-07 15:33_

---

_Review comment by @konstin on `crates/puffin-resolver/src/distribution/source_distribution.rs`:147 on 2023-11-07 15:33_

oh right, do we already have something like a `diplay_id` or should i add one?

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/distribution/source_distribution.rs`:147 on 2023-11-07 15:34_

You could just call `.to_string()` on it for a display ID, I think? That will show `package==version` or `package @ URL`.

---

_@charliermarsh reviewed on 2023-11-07 15:34_

---

_@charliermarsh approved on 2023-11-08 15:10_

---

_@charliermarsh approved on 2023-11-08 15:22_

---

_Comment by @charliermarsh on 2023-11-08 15:23_

@konstin - I'm merging your stuff as I approve to avoid conflicts with refactors that I'm working on now.

---

_Comment by @konstin on 2023-11-08 15:24_

Thanks for rebasing! Yes please merge quickly, we're getting a lot of merge conflicts otherwise

---

_Merged by @charliermarsh on 2023-11-08 15:26_

---

_Closed by @charliermarsh on 2023-11-08 15:26_

---

_Branch deleted on 2023-11-08 15:26_

---

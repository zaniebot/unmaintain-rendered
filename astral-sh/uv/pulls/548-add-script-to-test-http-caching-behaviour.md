```yaml
number: 548
title: Add script to test http caching behaviour
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: konstin/script-to-test-caching-behaviour
created_at: 2023-12-04T19:32:25Z
updated_at: 2024-04-29T13:02:03Z
url: https://github.com/astral-sh/uv/pull/548
synced_at: 2026-01-10T14:37:54Z
```

# Add script to test http caching behaviour

---

_Pull request opened by @konstin on 2023-12-04 19:32_

If we have a correct http caching implementation, this would print `<name>_a` on the first pass and `<name>_b` on the second pass.

Current output:

```console
$ scripts/cache_server/run.sh

# Cache a
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
Resolved 4 packages in 0ms
⠙ Resolving dependencies...                                                                                                                          127.0.0.1 - - [04/Dec/2023 20:27:53] "GET /pkg_source_dist-0.1.0.tar.gz HTTP/1.1" 200 -
127.0.0.1 - - [04/Dec/2023 20:27:53] "GET /pkg_whl-0.1.0-py3-none-any.whl HTTP/1.1" 200 -
Built pkg-source-dist @ http://0.0.0.0:8000/pkg_source_dist-0.1.0.tar.gz                                                                             Downloaded 3 packages in 869ms
Unzipped 3 packages in 1ms
Uninstalled 1 package in 6ms
Installed 4 packages in 1ms
 - pip==23.3.1
 + pkg-path-source-dist @ file:///home/konsti/projects/puffin/scripts/cache_server/cache_a/pkg_path_source_dist-0.1.0.tar.gz
 + pkg-path-whl @ file:///home/konsti/projects/puffin/scripts/cache_server/cache_a/pkg_path_whl-0.1.0-py3-none-any.whl
 + pkg-source-dist @ http://0.0.0.0:8000/pkg_source_dist-0.1.0.tar.gz
 + pkg-whl @ http://0.0.0.0:8000/pkg_whl-0.1.0-py3-none-any.whl
pkg_source_dist_a
pkg_whl_a
pkg_path_dist_a
pkg_path_whl_a

# Cache b
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
Resolved 2 packages in 0ms
Downloaded 1 package in 221ms
Unzipped 1 package in 0ms
Uninstalled 2 packages in 0ms
Installed 2 packages in 0ms
 - pkg-path-source-dist==0.1.0
 + pkg-path-source-dist @ file:///home/konsti/projects/puffin/scripts/cache_server/cache_b/pkg_path_source_dist-0.1.0.tar.gz
 - pkg-path-whl==0.1.0
 + pkg-path-whl @ file:///home/konsti/projects/puffin/scripts/cache_server/cache_b/pkg_path_whl-0.1.0-py3-none-any.whl
pkg_source_dist_a
pkg_whl_a
pkg_path_dist_b
pkg_path_whl_b
Done
```


---

_Comment by @konstin on 2023-12-04 19:32_

Current dependencies on/for this PR:
* main
  * **PR #543** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/543?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #545** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/545?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #548** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/548?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #544** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/544?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/548?utm_source=stack-comment).

---

_Renamed from "Add script to test caching behaviour" to "Add script to test http caching behaviour" by @konstin on 2023-12-06 11:37_

---

_Closed by @zanieb on 2024-02-16 05:17_

---

_Branch deleted on 2024-04-29 13:02_

---

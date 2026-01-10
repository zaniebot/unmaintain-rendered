---
number: 1906
title: Better benchmarking
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2023-01-16T04:52:58Z
updated_at: 2023-06-08T16:59:10Z
url: https://github.com/astral-sh/ruff/issues/1906
synced_at: 2026-01-10T01:22:40Z
---

# Better benchmarking

---

_Issue opened by @not-my-profile on 2023-01-16 04:52_

* [ ] create a script for running the CPython benchmark
* [ ] include the benchmark SVG shown in the README in the repository and add some script to regenerate it
    * [ ] include the versions of the linters in the SVG
    * [ ] add `ruff --select ALL` to the chart
* [ ] introduce some tooling to benchmark the current working directory in comparison to a certain git branch (`main` by default)
* [ ] create tooling for creating a breakdown for the total time ruff takes (how much of it is spend reading the files, parsing them with RustPython, how much time in checkers::ast, checkers::lines, checkers::tokens, etc.)

---

_Comment by @charliermarsh on 2023-01-16 05:30_

For what it’s worth, I care more about better tooling to benchmark Ruff than I do about benchmarking against other tools (and recreating the graph, etc.). Most useful here by far would be automating Ruff’s internal benchmark, measuring it continuously, and breaking it down by where time is spent.

---

_Comment by @charliermarsh on 2023-01-16 05:54_

(I'm actually regenerating the graph for unrelated reasons, so I'll check-in some instructions and such. Also, if helpful, all of the versions are encoded in `scripts/pyproject.toml`.) 

---

_Comment by @charliermarsh on 2023-01-16 06:43_

(Updated the graph and added some more instructions in #1907.)

---

_Comment by @jvdd on 2023-02-02 12:00_

I just discovered codspeed: https://codspeed.io/ - a tool for continuous benchmarking and find it amizing!  

It can monitor [Python](https://docs.codspeed.io/benchmarks/python) & [Rust](https://docs.codspeed.io/benchmarks/rust) benchmarks and gives feedback in pull requests about improvements / regressions - [see example](https://github.com/jvdd/argminmax/pull/13#issuecomment-1413567852).  
After quickly toying around with it, the benchmark results look really stable (better / same stability as I get on my development server).

Some large Rust libraries that interated codspeed:
- https://github.com/pydantic/pydantic-core/pull/332
- https://github.com/samueltardieu/pathfinding/pull/404

P.S.: I am not affiliated / involved with codspeed - just very pleased with it and thought it could be worthwhile to share this with such a performance oriented library as this one :)


---

_Comment by @charliermarsh on 2023-02-02 13:03_

I'm interested in giving it a try :) The author reached out to me a while ago and kindly gave me beta access, but I haven't had a chance to set it up :joy:

---

_Comment by @charliermarsh on 2023-06-08 16:59_

Closing as we added `cargo benchmark` which helps with some of the above. Any remaining we can open as separate tickets in the future if we choose to prioritize.

---

_Closed by @charliermarsh on 2023-06-08 16:59_

---

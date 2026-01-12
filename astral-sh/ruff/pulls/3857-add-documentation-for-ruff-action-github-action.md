```yaml
number: 3857
title: "Add documentation for `ruff-action` (GitHub Action!)"
type: pull_request
state: merged
author: brucearctor
labels: []
assignees: []
merged: true
base: main
head: action
created_at: 2023-04-03T03:56:08Z
updated_at: 2023-04-04T00:04:52Z
url: https://github.com/astral-sh/ruff/pull/3857
synced_at: 2026-01-12T04:28:19Z
```

# Add documentation for `ruff-action` (GitHub Action!)

---

_Pull request opened by @brucearctor on 2023-04-03 03:56_

Following up from now closed PR:  https://github.com/charliermarsh/ruff/pull/3820

This updates ruff docs and outlines the [Ruff-Action](https://github.com/chartboost/ruff-action).

This follows Pyright style:  https://github.com/microsoft/pyright/blob/main/docs/ci-integration.md

Ex ->
Pyright found at: https://github.com/microsoft/pyright
Pyright action found in separate 'org' at:  https://github.com/jakebailey/pyright-action

I'm glad we decided on this direction -- having setup this way is great -- I can more easily maintain/extend [ and as training for some of my engineers ], as well as is the first real step towards an open-source program which I've been meaning to get started with!  

Near-term follow up steps will be to increase documentation.  Do advise re: anything else that makes sense.  


Separately ->
Also, I'm not sure where you're based or your travel inclinations these days.  I'll be in Miami end-of-May, and am to give a conference talk to [Google Developer Student Club Leads](https://gdsc.community.dev/), [GDG organizers](https://gdg.community.dev/), [WTM Ambassadors](https://developers.google.com/womentechmakers/ambassadors), and [Google Developer Experts](https://goo.gle/gde).  [ I'm a GDE ].  The plan was on Python, CI, and in-this-case, likely `ruff`!  So, if you could potentially be around, and open to join, let's chat.  

---

_Comment by @github-actions[bot] on 2023-04-03 04:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.16ms     2.8 MB/sec    1.00     14.2±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    459.0±0.99µs     6.4 MB/sec    1.00    454.3±1.03µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.01ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.01ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1641.2±2.57µs    10.1 MB/sec    1.00   1639.5±6.12µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.9±1.23µs    16.3 MB/sec    1.01    181.8±1.24µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.00ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.0±0.23ms     2.5 MB/sec    1.00     15.8±0.15ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.1 MB/sec    1.00      4.0±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    491.5±6.74µs     6.0 MB/sec    1.00    483.2±7.11µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.8±0.08ms     3.7 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.08ms     4.9 MB/sec    1.00      8.0±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1781.2±16.60µs     9.3 MB/sec    1.00  1758.8±18.71µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    194.3±2.97µs    15.2 MB/sec    1.00    192.5±4.43µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @brucearctor on 2023-04-03 15:59_

@ssweber do see docs/info and the [Ruff-Action Project](https://github.com/chartboost/ruff-action).  We're waiting for the docs to get merged to this repo, but the action should be fine/usable.  Definitely share feedback, any issues you come across, etc.  We should be able to quickly address any problems, extend to new features, and increase docs as needed.  

---

_Comment by @ssweber on 2023-04-03 16:33_

thanks. giving it a whirl.

---

_@charliermarsh reviewed on 2023-04-03 16:58_

---

_Review comment by @charliermarsh on `docs/usage.md`:85 on 2023-04-03 16:58_

Looking at the YAML, should this be `args: --select B`?

---

_@brucearctor reviewed on 2023-04-03 17:23_

---

_Review comment by @brucearctor on `docs/usage.md`:85 on 2023-04-03 17:23_

🤦 -- had updated the code, but not the docs with that change.  Docs now fixed, good catch.  Updated both here and in the action repo.  

---

_Comment by @charliermarsh on 2023-04-03 19:39_

What are your thoughts on ownership of the action? Would you be open to either or both of (1) PRing on https://github.com/charliermarsh/ruff-action, or (2) moving the action under our GitHub Org once it's setup in the next month or so? I'd like all the "official" Ruff projects to live under the same ownership structure. For example, right now, the pre-commit action is under https://github.com/charliermarsh/ruff-pre-commit and will move under the GitHub Org once it's setup.


---

_Comment by @charliermarsh on 2023-04-03 19:40_

> Also, I'm not sure where you're based or your travel inclinations these days. I'll be in Miami end-of-May, and am to give a conference talk to [Google Developer Student Club Leads](https://gdsc.community.dev/), [GDG organizers](https://gdg.community.dev/), [WTM Ambassadors](https://developers.google.com/womentechmakers/ambassadors), and [Google Developer Experts](https://goo.gle/gde). [ I'm a GDE ]. The plan was on Python, CI, and in-this-case, likely ruff! So, if you could potentially be around, and open to join, let's chat.

That's amazing! It's unlikely I'll be able to join, I'm traveling for PyCon in late April and I have a baby at home so I'm trying to avoid traveling in back-to-back months. I'd love to help support however I can though.


---

_Comment by @brucearctor on 2023-04-03 20:04_

> What are your thoughts on ownership of the action? Would you be open to either or both of (1) PRing on https://github.com/charliermarsh/ruff-action, or (2) moving the action under our GitHub Org once it's setup in the next month or so? I'd like all the "official" Ruff projects to live under the same ownership structure. For example, right now, the pre-commit action is under https://github.com/charliermarsh/ruff-pre-commit and will move under the GitHub Org once it's setup.

GitHub Org:  Setting one up is pretty straightforward, I mean if just about an org that isn't your name, that could be done today.  I imagine more you'll want to think about structure [ and legal aspects ], ex: perhaps you are going to try to build a business around ruff [ or 'high performance' python tooling ], or donate such things to a foundation and have it live there [ ex: Github.com/apache/ruff, github.com/apache/ruff-pre-commit, github.com/apache/ruff-action ]....  Happy to walk you/ruff through the ASF Incubating process, ex: https://incubator.apache.org/ if that's of interest.  

Since I can use this currently as a way to bootstrap an OSS program at Chartboost, I do like the thought of it living there initially, gives nice company talking points [ I recently started there, so a decent 'win' for me, and serves as foundation for doing more of similar ].  Donating to a proper organization also sounds sensible once the to-be-created org is ready... Again, that's a great step corporation-wise, not only openly sharing but also giving, esp. to a place that then would be a suitable long-term home :-) 

---

_Comment by @brucearctor on 2023-04-03 20:36_

I now already see 2 blogposts!  1st being that chartboost created this usable thing to serve engineers within the company as well as the wider ruff community [ sounds like already a known user ], and a later 2nd one that we donated to a good home/community for better/common support/utilization


---

_Comment by @charliermarsh on 2023-04-03 23:13_

Ok, that sounds reasonable! I do plan to setup the Org and move the official Ruff projects under it, it's just blocked on some other stuff and I want to do it "all at once", ideally before the end of April. I may ask if you'd be willing to transfer the project at that time, but for now we can link to the chartboost repo.

I'm gonna make some tweaks to the docs to ensure consistent voice and tone with the rest of the Ruff docs, you're welcome to pull in those changes or not to the chartboost repo, totally up to you!


---

_Comment by @brucearctor on 2023-04-03 23:16_

Definitely...  can pull in improved docs.  

---

_Renamed from "GitHub Action!" to "Add documentation for `ruff-action` (GitHub Action!)" by @charliermarsh on 2023-04-03 23:42_

---

_Merged by @charliermarsh on 2023-04-03 23:47_

---

_Closed by @charliermarsh on 2023-04-03 23:47_

---

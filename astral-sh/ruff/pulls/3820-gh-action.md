```yaml
number: 3820
title: GH-Action
type: pull_request
state: closed
author: brucearctor
labels: []
assignees: []
base: main
head: main
created_at: 2023-03-30T17:48:01Z
updated_at: 2023-04-03T03:37:01Z
url: https://github.com/astral-sh/ruff/pull/3820
synced_at: 2026-01-12T15:55:13Z
```

# GH-Action

---

_@brucearctor_

Accidentally closed:  https://github.com/charliermarsh/ruff/pull/3756 ( and didn't seem like I was able to re-open it )

Addresses:  https://github.com/charliermarsh/ruff/issues/3289

---

_Comment by @github-actions[bot] on 2023-03-30 17:58_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.25ms     2.4 MB/sec    1.00     16.7±0.19ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.01      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    530.8±5.55µs     5.6 MB/sec    1.01    538.0±4.93µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.02      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.08ms     4.8 MB/sec    1.01      8.6±0.10ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1910.5±17.03µs     8.7 MB/sec    1.01  1936.6±14.85µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.3±2.95µs    13.8 MB/sec    1.01    214.7±3.37µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.01      3.9±0.05ms     6.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.8±0.38ms     2.8 MB/sec    1.00     14.7±0.29ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.08ms     4.4 MB/sec    1.00      3.8±0.08ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   451.3±12.56µs     6.5 MB/sec    1.00   449.2±17.41µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.19ms     4.1 MB/sec    1.00      6.2±0.31ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.11ms     5.4 MB/sec    1.00      7.4±0.09ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1637.7±34.09µs    10.2 MB/sec    1.00  1628.3±37.28µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.1±6.89µs    16.7 MB/sec    1.01    179.4±9.02µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.09ms     7.4 MB/sec    1.00      3.4±0.11ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @ssweber on 2023-03-31 10:41_

(this open pull is missing the docs. didn’t know if that was intentional or an oversight)

---

_Comment by @charliermarsh on 2023-03-31 14:35_

Ok, sorry for the back-and-forth on this, but I've discussed it with @MichaReiser and @konstin and we've decided that we'd prefer to use a Pyright-like setup. That is, we'd prefer to put the action in https://github.com/charliermarsh/ruff-action, and point to it from the docs. The benefit is that we can version the action separately. As-is, we wouldn't really have any way to signal changes in the action's API or configuration, since it'd be coupled to Ruff versions.

Are you open to PR'ing the action YAML in that repo, and then docs here? Then I can publish it to the Marketplace?

---

_Comment by @brucearctor on 2023-03-31 15:33_

> (this open pull is missing the docs. didn’t know if that was intentional or an oversight)

Oversight ...

---

_Comment by @brucearctor on 2023-03-31 15:36_

> Ok, sorry for the back-and-forth on this, but I've discussed it with @MichaReiser and @konstin and we've decided that we'd prefer to use a Pyright-like setup. That is, we'd prefer to put the action in https://github.com/charliermarsh/ruff-action, and point to it from the docs. The benefit is that we can version the action separately. As-is, we wouldn't really have any way to signal changes in the action's API or configuration, since it'd be coupled to Ruff versions.
> 
> Are you open to PR'ing the action YAML in that repo, and then docs here? Then I can publish it to the Marketplace?

OK - can mimick pyright setup.

---

_Comment by @brucearctor on 2023-03-31 16:28_

Array vs String.  Where are people at on preference, @MichaReiser , @charliermarsh , @konstin ?  Ex: https://github.com/charliermarsh/ruff/pull/3756#discussion_r1151500881 ...

I imagine better to just put in place what is most desired than needing to refactor/version-bump to accommodate.  

---

_Comment by @charliermarsh on 2023-03-31 16:50_

It looks like Pyright uses a string and calls it `extra-args`. Black uses a string and calls it `options` (that's where we got the current setup AFAICT). Personally I'm fine to use a string. I think for me `args` or `extra-args` is more clearly "command-line arguments" vs. options which I would expect to be an object. `extra-args` seems reasonable if we're passing in _some_ arguments via `src`.

So that to me suggests `extra-args`, a string. What do you think?


---

_Comment by @brucearctor on 2023-03-31 17:02_

That works: I'll get things cleaned up and PR together - if not today should be able to address over the weekend.  Naturally, if others have strong opinions for another course of action, please share.  

---

_Comment by @charliermarsh on 2023-03-31 17:05_

Thank you, and no rush. I know it might be a little frustrating to have back-and-forth but API decisions like this tend to be an iterative process as we build agreement. Really appreciate your work here.

---

_Comment by @brucearctor on 2023-03-31 17:18_

This is nothing new, API discussions generally need to occur and consensus built - esp. for a public facing interface that shouldn't have breaking changes too often.  

Plenty familiar as I'm a committer and [PMC](https://www.apache.org/dev/pmc.html) member ( or equivalent ) on a number of OSS projects [ esp [ASF](https://www.apache.org/) ], and often guide OSS journeys, or lead intro to OSS trainings.  It just takes a little bit to figure out how people want to work/collaborate ( the specifics of any given "community" )

---

_Comment by @brucearctor on 2023-03-31 19:22_

Upon further reflection, I think we should go with `args` ...  `extra-args` seems slightly/potentially confusing.  "Extra" has the connotation of more/additional, yet the args would become the args not append to something existing.  Sound good @charliermarsh ?  

Anything else you can think of?  Otherwise, should be able to mimic pyright setup easily.  THEN can follow up with more improvements, additional docs [ ex: to cover the not basic use-cases, etc ]

---

_Comment by @ssweber on 2023-03-31 21:21_

Is there a good reason to split up the command line args?

hope I’m not being a bother.

```
with:
    command-line-args: “check .”



---

_Comment by @brucearctor on 2023-03-31 21:36_

@ssweber -- Comments welcome, at least for consideration.

```yaml
name: Ruff

on: [push, pull_request]

jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0
```

^^ as written that runs `ruff check .`


```yaml
name: Ruff

on: [push, pull_request]

jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0
        with:
            src: /path/to/code/
```

^^ would run `ruff check /path/to/code/`

```yaml
name: Ruff

on: [push, pull_request]

jobs:
  ruff:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: charliermarsh/ruff@v0
        with:
            args: --select ANN
```

would run `ruff --select ANN .` ( I'll eventually add docs, and extra information for how this action can actually accommodate the `--fix use case`, but that requires utilizing some permissions and other actions, so a phase 2 thing :-) )

etc.  

Then you can update either the path, or the args ( or version ) independent of eachother.  Why would I want to have to specify a '.' if that's the most likely setting for users, it is a nice default and better to not have to mention unless wanting to override.  Naturally, I'll cleanup docs to include these things.

Did that answer your question?


---

_Comment by @ssweber on 2023-03-31 22:14_

Yes, thank you for the detailed response!

---

_Closed by @brucearctor on 2023-04-03 03:37_

---

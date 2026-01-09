---
number: 5474
title: Installed package versions depend upon argument order in uv pip install
type: issue
state: closed
author: larsevj
labels:
  - question
assignees: []
created_at: 2024-07-26T10:55:48Z
updated_at: 2024-08-19T16:53:53Z
url: https://github.com/astral-sh/uv/issues/5474
synced_at: 2026-01-07T13:12:17-06:00
---

# Installed package versions depend upon argument order in uv pip install

---

_Issue opened by @larsevj on 2024-07-26 10:55_

Hi, I noticed when installing a package that the versions installed using `uv pip install` depends upon the order of the packages given to uv whereas when using pip the versions installed are consistent. For instance running
`uv pip install importlib_metadata pysnyk` produces the following environment:
```
Package            Version
------------------ --------
certifi            2024.7.4
charset-normalizer 3.3.2
idna               3.7
importlib-metadata 8.2.0
mashumaro          1.24
msgpack            1.0.8
poetry-version     0.1.5
pysnyk             0.3.1
pyyaml             6.0.1
requests           2.32.3
tomlkit            0.5.11
typing-extensions  4.12.2
urllib3            2.2.2
zipp               3.19.2

```

Whereas if you run `uv pip install pysnyk importlib_metadata` you get the following environment:
```
Package            Version
------------------ --------
certifi            2024.7.4
charset-normalizer 3.3.2
decorator          5.1.1
deprecation        2.1.0
idna               3.7
importlib-metadata 6.11.0
mashumaro          3.13.1
packaging          24.1
py                 1.11.0
pysnyk             0.9.19
requests           2.32.3
retry              0.9.2
typing-extensions  4.12.2
urllib3            2.2.2
zipp               3.19.2
```

Pip seems to always produce the last result no matter the order of the packages, and from reading the docs I would expect `uv pip install` to produce the same result as pip here consistently?

The examples above were produced with python3.11 and uv 0.2.29.


---

_Comment by @charliermarsh on 2024-07-26 14:09_

I think this is reasonable. There are many equally-valid resolutions given that set of constraints, and so we have to prioritize packages in some order when solving. Modulo everything else, we prioritize in the order in which they were provided by the user. So, notice that in the first resolution, you have a newer `importlib-metadata` but an older `pysnyk`, whereas in the second, you have an older `importlib-metadata` but a newer `pysnyk`.

The following would _not_ be ok though:

1. If we produced a resolution in which _both_ `pysnyk` and `importlib-metadata` were at lower versions than in some other ordering.
2. If we produced inconsistent resolutions for the same ordering of dependencies when run multiple times.

But otherwise, I think this is ok. If one of the two outputs is desired, it should be encoded in the dependency constraints (e.g., `pysynk >= 0.9.0`).


---

_Label `question` added by @charliermarsh on 2024-07-26 14:09_

---

_Comment by @larsevj on 2024-07-26 15:22_

The difference between the two results above as I see it is that `pysnyk` depends upon `importlib-metadata` and not the other way around. 
If I understood the resolution strategy correctly, if you have for instance a package Y that depends upon numpy and has pinned it to `numpy<2`.  Then if you do `uv pip install numpy Y` (or lists numpy before package Y in you requirements file), then `uv` will install the latest numpy version `2.0.1` and then it will find a version of Y that does not have numpy pinned, which is propably a version released before the announcement of the numpy 2 release, so it will most certainly not work with numpy 2?
Maybe I am missing something here, but it seems to me like this has the potential of creating environments with incompatible versions?


---

_Comment by @charliermarsh on 2024-07-26 17:34_

Yes, that's possible and would be correct behavior. There's a good chance that pip would do exactly the same thing.

---

_Comment by @charliermarsh on 2024-07-26 17:35_

We can't invent constraints that don't exist. Who knows, the inverse order could be equally problematic.

---

_Comment by @larsevj on 2024-07-26 19:27_

So far pip has done the same thing consistently on the test cases I have tried, but maybe I just have not found the proper corner case. 
The thing that surprises me the most about the examples above is the fact that a package that has no dependencies itself, (e.g. numpy) will if placed properly "determine" the version of the package that depends upon it. I would have thought that the opposite would always be the case in simple cases like this. That you start resolving from the highest version of the package that has the other packages as dependencies. 
I guess the output I would have expected might be similar to the one uv would have produced if you did a reverse topological sort on the packages before passing them to uv (at least in these simple cases). Or since this is a situation that happens when you explicitly specify a package in your requirements that is a dependency of some of the other packages, I would expect the same result as if I had not specified the package explicitly in the requirements file.
But this might not be reasonable, and I might have to go and read up on how dependency resolution algorithms work.
Anyway since this seems like it is intended from `uv` side, you can probably close the issue, and I thank you for giving some answers to my question.

---

_Comment by @konstin on 2024-07-29 08:16_

>  If I understood the resolution strategy correctly, if you have for instance a package Y that depends upon numpy and has pinned it to numpy<2. Then if you do uv pip install numpy Y (or lists numpy before package Y in you requirements file), then uv will install the latest numpy version 2.0.1 and then it will find a version of Y that does not have numpy pinned, which is propably a version released before the announcement of the numpy 2 release, so it will most certainly not work with numpy 2?

As a dependency resolution algorithm, we need to trust the packages that their metadata is correct (with the escape hatches of constraints and overrides), so when a package adds a new constraint, we have to assume that the new release has become incompatible. When a package does not provide upper bounds, we have to assume it's compatible with all versions, we have no means to figure out the actual bound in uv.

> The thing that surprises me the most about the examples above is the fact that a package that has no dependencies itself, (e.g. numpy) will if placed properly "determine" the version of the package that depends upon it. I would have thought that the opposite would always be the case in simple cases like this. That you start resolving from the highest version of the package that has the other packages as dependencies.

While this is possible algorithmically, it would be prohibitively expensive in uv's case. The by far slowest part is fetching metadata (even when we just read it from the cache), so we make the decision which package to prioritize without metadata. Another aspect is that the number of dependencies can change between releases, so we'd need to come up with a reliable, robust topological prioritization. Our current rules are using tighter specifiers first (`numpy==...` before `torch>=...`), which helps trying fewer wrong versions, and otherwise apply them in the order we first saw a package.

---

_Comment by @Enayaaa on 2024-07-30 09:11_

I recently had a quite confusing problem related to this. I don't think I understand why the resolver installs the version it does.

**requirements.txt**:
```txt
sphinx
sphinx_rtd_theme
```
**environment**:
```
Package                       Version
----------------------------- --------
alabaster                     1.0.0
babel                         2.15.0
certifi                       2024.7.4
charset-normalizer            3.3.2
docutils                      0.21.2
idna                          3.7
imagesize                     1.4.1
jinja2                        3.1.4
markupsafe                    2.1.5
packaging                     24.1
pygments                      2.18.0
requests                      2.32.3
snowballstemmer               2.2.0
sphinx                        8.0.2
sphinx-rtd-theme              0.5.1
sphinxcontrib-applehelp       2.0.0
sphinxcontrib-devhelp         2.0.0
sphinxcontrib-htmlhelp        2.1.0
sphinxcontrib-jsmath          1.0.1
sphinxcontrib-qthelp          2.0.0
sphinxcontrib-serializinghtml 2.0.0
tomli                         2.0.1
urllib3                       2.2.2
```

**requirements.txt**:
```txt
sphinx_rtd_theme
sphinx
```
**environment:**
```
Package                       Version
----------------------------- --------
alabaster                     0.7.16
babel                         2.15.0
certifi                       2024.7.4
charset-normalizer            3.3.2
docutils                      0.20.1
idna                          3.7
imagesize                     1.4.1
jinja2                        3.1.4
markupsafe                    2.1.5
packaging                     24.1
pygments                      2.18.0
requests                      2.32.3
snowballstemmer               2.2.0
sphinx                        7.4.7
sphinx-rtd-theme              2.0.0
sphinxcontrib-applehelp       2.0.0
sphinxcontrib-devhelp         2.0.0
sphinxcontrib-htmlhelp        2.1.0
sphinxcontrib-jquery          4.1
sphinxcontrib-jsmath          1.0.1
sphinxcontrib-qthelp          2.0.0
sphinxcontrib-serializinghtml 2.0.0
tomli                         2.0.1
urllib3                       2.2.2
```
I guess I can omit listing `sphinx` here since `sphinx_rtd_theme` already requires `sphinx`, but what would be the rule of thumb here for properly listing your deps in order? Is it the package at the top of the dependency tree first, in this case `sphinx_rtd_theme`?

Does anybody know what `pip` would do here, since order does not seem to matter with pip?

---

_Comment by @konstin on 2024-07-30 11:41_

We have `sphinx-rtd-theme==2.0.0: sphinx>=5, <8`, so a resolver has to make a decision whether to pick the highest version of the theme, or the latest version of sphinx. From a resolver's perspective, both options are equally valid, while i'd expect you want the newest theme version with sphinx 7. It seems that pip does this choice in a different order than uv, where uv considers the order in your input file, pip doesn't. Your best path moving forward is probably adding lower bounds for the theme and for sphinx, this will make any resolver produce the result you want.

From a pubgrub perspective, this is an interesting problem: Pubgrub works so that we first select a package, and then try all versions for that package. That means that we select either sphinx or sphinx-rtd-theme before we know that there is a sphinx-rtd-theme -> sphinx dep we want to consider. What we'd need to do to generate the desirable solution and this and the original case is: We have picked sphinx, and we have found a compatible version of sphinx. We next select sphinx-rtd-theme, and try the first version of sphinx-rtd-theme. We see a dep onto `sphinx` that is incompatible. We would now have to switch prioritization, lock in that version of sphinx-rtd-theme, remove the selection of sphinx from the partial solution (something that pubgrub doesn't support atm) and start iterating over versions of sphinx instead.

---

_Referenced in [astral-sh/uv#5796](../../astral-sh/uv/issues/5796.md) on 2024-08-05 17:27_

---

_Referenced in [astral-sh/uv#6209](../../astral-sh/uv/issues/6209.md) on 2024-08-19 15:32_

---

_Referenced in [astral-sh/uv#6211](../../astral-sh/uv/pulls/6211.md) on 2024-08-19 16:34_

---

_Closed by @zanieb on 2024-08-19 16:53_

---

_Closed by @zanieb on 2024-08-19 16:53_

---

_Comment by @zanieb on 2024-08-19 16:53_

Discussion welcome to continue here — just closing as I've documented this now.

---

_Referenced in [astral-sh/uv#7575](../../astral-sh/uv/issues/7575.md) on 2024-09-20 17:14_

---

_Referenced in [HarrisonKramer/optiland#305](../../HarrisonKramer/optiland/pulls/305.md) on 2025-09-04 07:23_

---

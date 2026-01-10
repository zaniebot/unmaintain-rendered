---
number: 2230
title: "Python tests pass when packages installed with `pip` outside venv , but not with `uv pip install --system`"
type: issue
state: closed
author: strickvl
labels:
  - bug
assignees: []
created_at: 2024-03-06T06:46:09Z
updated_at: 2024-03-07T20:22:27Z
url: https://github.com/astral-sh/uv/issues/2230
synced_at: 2026-01-10T01:23:14Z
---

# Python tests pass when packages installed with `pip` outside venv , but not with `uv pip install --system`

---

_Issue opened by @strickvl on 2024-03-06 06:46_

[This is maybe a weird one, and not quite sure to what extent it's driven by `uv` vs other things, but the change made was adding `uv` so I'm going to ask here.]

We have a series of unit tests that run on 4 versions of Python in Windows, Ubuntu and MacOS runners in github actions. I'm switching our CI to use `uv pip install` since it offers a big speedup for installation times. For each runner, we set up Python with `actions/setup-python@v5.0.0` as is standard and then we install all our packages using [this script](https://github.com/zenml-io/zenml/blob/feature/use-uv-ci/scripts/install-zenml-dev.sh). Take note that we're using `uv pip install --system` to install the packages.

Later on we run our unit tests etc. Previously, before the changes [in the PR / branch](https://github.com/zenml-io/zenml/pull/2442) all unit tests passed without issue. Since using `uv`, two tests fail on [essentially the same issue](https://github.com/zenml-io/zenml/blob/4cdbe487d8792327eb3e8942c42929eab59ef2ca/tests/unit/utils/test_source_utils.py#L110-L114) across all our Windows runners:

```python
    assert source_utils.resolve(defaultdict) == Source(
        module=defaultdict.__module__,
        attribute=defaultdict.__name__,
        type=SourceType.BUILTIN,
    )
```

(Code for the `resolve` function is [here](https://github.com/zenml-io/zenml/blob/28eb2f5f60264c18bf788faf6a0380e961a68658/src/zenml/utils/source_utils.py) + [for the test here](https://github.com/zenml-io/zenml/blob/a24ccb12d4cfd5652388b031ca828600981a1925/tests/unit/utils/test_source_utils.py#L110-L113).)

[Here](https://github.com/zenml-io/zenml/actions/runs/8100734877/job/22171677679) you can see the two failures. Our source resolution util is behaving differently now that we're using `uv` to install our packages. I know there were [some issues and discussions](https://github.com/astral-sh/uv/issues?q=is%3Aissue+windows+) around Windows installations so I thought I'd ask if there's a known issue here where for some reason when using `uv`'s `collections` version of the `defaultdict` our code resolves it as an unknown sourcetype with `uv` but as builtin without `uv`?

---

_Comment by @charliermarsh on 2024-03-06 20:20_

That is... interesting. I'm not immediately sure what could be going wrong there. I'm obliged to confirm that `uv` is the only change in that branch, and that there isn't something else that could be at fault? :)

---

_Label `bug` added by @charliermarsh on 2024-03-06 20:20_

---

_Comment by @strickvl on 2024-03-07 13:19_

Correct. https://github.com/zenml-io/zenml/pull/2442/files you can see the only changes that have been made in this branch relate to using `uv` instead of `pip`. The actual runs for the runners (I reran them fresh to confirm just now) can be viewed [here](https://github.com/zenml-io/zenml/actions/runs/8187349253?pr=2442). (It's the `ci-slow.yaml` that triggers the Windows runners to run the unit tests, and all those Windows runners fail for the reason explained above.)

A pity you didn't already have something that springs to mind. Suggests that maybe it's actually nothing to do with `uv`, though don't have too many bright ideas on how to pin that down, esp given the only thing changed in this PR is switching out `pip` for `uv`.

---

_Comment by @charliermarsh on 2024-03-07 14:19_

How would you describe the failure informally? Like, "It now thinks that `collections` is a third-party module instead of a standard-library module, so there must be some `collections` somewhere in path" or similar.

---

_Comment by @strickvl on 2024-03-07 14:26_

https://github.com/zenml-io/zenml/actions/runs/8190492597 is my attempt to isolate what's going on, logging the state of the following:

```bash
python -c "import sys; print(f'Python version: {sys.version}')"
python -c "import collections; print(f'collections module file path: {collections.__file__}')"
python -c "import collections; print(f'collections module name: {collections.__name__}')"
python -c "from collections import defaultdict; print(f'defaultdict type: {type(defaultdict)}')"
python -c "import sys; from collections import defaultdict; from zenml.config.source import Source, SourceType; from zenml.utils import source_utils; print(f'source_utils.resolve(defaultdict): {source_utils.resolve(defaultdict)}'); print(f'Source object: {Source(module=defaultdict.__module__, attribute=defaultdict.__name__, type=SourceType.BUILTIN)}')"
python -c "import sys; print(f'Python executable: {sys.executable}')"
python -c "import sys; print(f'Module search paths: {sys.path}')"
echo "PYTHONPATH: $PYTHONPATH"
echo "PATH: $PATH"
uv --version
pip list
```

I hope that will shed some light on what's going on. (Full workflow [here](https://github.com/zenml-io/zenml/actions/runs/8190492597/workflow).)

---

_Assigned to @konstin by @konstin on 2024-03-07 15:51_

---

_Comment by @strickvl on 2024-03-07 17:23_

![CleanShot 2024-03-07 at 18 14 02@2x](https://github.com/astral-sh/uv/assets/3348134/02d68a45-f0a5-4a59-ad45-bfe23b380d57)

They both output the same thing when run this way, interestingly. So outside the test suite, both `pip` and `uv` installations resolve the 'wrong' way on Windows.

So for some reason it's resolving `collections` as a non-standard-lib module, as you said, for Windows whereas it's find it just fine within Ubuntu / MacOS.

---

_Comment by @strickvl on 2024-03-07 20:22_

Having dug into it a bit further, I've noticed that some caching settings on our CI seem to have been the thing that have changed. I reran the tests with the old behaviour restored and the tests pass. We'll have to dig into this on our side to better understand why the caching changes the behaviour of the tests, but I'm now sure that it has nothing to do with `uv`. Thanks for the prompt to dig further and apologies for any wasted time.

---

_Closed by @strickvl on 2024-03-07 20:22_

---

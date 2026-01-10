---
number: 9322
title: "`uv pip compile` Ignores Lower Bound for Transitive Dependency"
type: issue
state: closed
author: vladislavlh
labels:
  - question
assignees: []
created_at: 2024-11-21T14:37:18Z
updated_at: 2024-11-21T15:14:10Z
url: https://github.com/astral-sh/uv/issues/9322
synced_at: 2026-01-10T01:24:39Z
---

# `uv pip compile` Ignores Lower Bound for Transitive Dependency

---

_Issue opened by @vladislavlh on 2024-11-21 14:37_

Hello `uv` team! 👋

First, I want to express my gratitude for the effort and dedication you’ve put into building and maintaining `uv`. It’s a fantastic tool, and I’ve been enjoying exploring its capabilities.

That said, I recently encountered a small issue while trying to compile dependencies with `uv`. Below are the details:

---

#### **Environment**
- **uv version**: `uv 0.5.3 (56d362208 2024-11-19)`
- **Platform**: `MacOS Sequoia 15.1.1`, 
- **Python version**: `Python 3.12.7`

---

#### **Steps to Reproduce**
Given the following input file `reqs.in`:

```
shap
```
When running the command:
```bash
uv pip compile reqs.in -o reqs.txt --verbose
```

##### **Observed Behaviour**
The resulting `reqs.txt` includes a pinned version of `numba` that does not respect the lower bound specified in the `shap` library's `pyproject.toml` file:
```
numba==0.53.1
```
However, as specified in shap's [pyproject.toml](https://github.com/shap/shap/blob/master/pyproject.toml#L24), the dependency for numba should have a lower bound of `>=0.54`.

From the verbose output, we see:
```
DEBUG Adding transitive dependency for shap==0.46.0: numba*
```
This indicates that `uv` did not pick up the correct lower bound for `numba`. The next steps, however, appear to have executed correctly. For example:
```
DEBUG Adding transitive dependency for numba==0.53.1: numpy>=1.15
```
It found the latest version of `numba` that doesn't have an upper bound of an accepted `numpy==2.1.3`

##### **Expected Behaviour**
The compiled reqs.txt should respect the lower bound for `numba` and pin a version `>=0.54` that is compatible with other dependencies. For instance, `pip-compile` correctly produces an output that includes:
```
numba==0.60.0
numpy==2.0.2
...
```

---

#### **Workaround**
Adding an explicit requirement to `reqs.in` resolves the issue:
```
numba>=0.60.0
```

---

Thank you again for all the incredible work you’re doing with `uv`. Let me know if you need additional details.

---

_Comment by @charliermarsh on 2024-11-21 14:42_

I think that may have been added since the most recent `shap` release. If you look at the [metadata](https://files.pythonhosted.org/packages/04/58/b2ea558ec8d9ed3728e83dfacb1b920c54a1a1f6feee2632c04676c3c1e9/shap-0.46.0-cp312-cp312-win_amd64.whl.metadata), it doesn't include the constraint:

```txt
Metadata-Version: 2.1
Name: shap
Version: 0.46.0
Summary: A unified approach to explain the output of any machine learning model.
Author-email: Scott Lundberg <slund1@cs.washington.edu>
License: MIT License
Project-URL: Repository, http://github.com/shap/shap
Project-URL: Documentation, https://shap.readthedocs.io/en/latest/index.html
Project-URL: Release Notes, https://shap.readthedocs.io/en/latest/release_notes.html
Classifier: Operating System :: Microsoft :: Windows
Classifier: Operating System :: POSIX
Classifier: Operating System :: Unix
Classifier: Operating System :: MacOS
Classifier: Programming Language :: Python :: 3.9
Classifier: Programming Language :: Python :: 3.10
Classifier: Programming Language :: Python :: 3.11
Classifier: Programming Language :: Python :: 3.12
Classifier: Intended Audience :: Information Technology
Classifier: Intended Audience :: Science/Research
Classifier: Topic :: Scientific/Engineering
Classifier: Topic :: Scientific/Engineering :: Artificial Intelligence
Classifier: Development Status :: 5 - Production/Stable
Classifier: License :: OSI Approved :: MIT License
Requires-Python: >=3.9
Description-Content-Type: text/markdown
License-File: LICENSE
Requires-Dist: numpy
Requires-Dist: scipy
Requires-Dist: scikit-learn
Requires-Dist: pandas
Requires-Dist: tqdm >=4.27.0
Requires-Dist: packaging >20.9
Requires-Dist: slicer ==0.0.8
Requires-Dist: numba
...
```

---

_Comment by @charliermarsh on 2024-11-21 14:42_

Yeah, it looks like the last release was in June but that constraint was committed in October.

---

_Label `question` added by @charliermarsh on 2024-11-21 14:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-21 14:42_

---

_Comment by @charliermarsh on 2024-11-21 14:43_

If you install from Git, we should respect it (as expected). Otherwise, you'll need to ask the `shap` team to publish a new release to PyPI.

---

_Closed by @charliermarsh on 2024-11-21 14:43_

---

_Comment by @charliermarsh on 2024-11-21 14:43_

(Happy to answer follow-up questions.)

---

_Comment by @vladislavlh on 2024-11-21 14:49_

Thank you for the prompt reply @charliermarsh!

Yeah, the `shap` library is known to not be actively maintained, so I'm not really surprised to have encountered this issue...

That said, and this might be slightly out of scope for this issue, I'm still trying to fully grasp how `uv` works. I'm curious to understand why `pip-compile` is able to adhere to the lower bound requirement even though it's not explicitly listed in the library's metadata. Could you shed some light on this? 

---

_Comment by @charliermarsh on 2024-11-21 14:56_

It's a specific issue with recent Numba and NumPy releases. pip-compile generates a different resolution, but both resolutions are technically valid. The main issue tracking that kind of undesirable solution is here: https://github.com/astral-sh/uv/issues/8157. I wrote up an explanation of the specifics of Numba and NumPy here: https://github.com/astral-sh/uv/issues/6281#issuecomment-2316240724.

---

_Comment by @vladislavlh on 2024-11-21 15:14_

Makes sense to me, thank you for the clarification!
I have no more questions, thank you for your time!

---

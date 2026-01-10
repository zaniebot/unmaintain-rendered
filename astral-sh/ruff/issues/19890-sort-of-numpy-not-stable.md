```yaml
number: 19890
title: Sort of Numpy not stable
type: issue
state: closed
author: ax3l
labels:
  - question
  - isort
  - needs-mre
assignees: []
created_at: 2025-08-13T05:49:57Z
updated_at: 2025-12-10T17:31:23Z
url: https://github.com/astral-sh/ruff/issues/19890
synced_at: 2026-01-10T11:09:59Z
```

# Sort of Numpy not stable

---

_Issue opened by @ax3l on 2025-08-13 05:49_

### Summary

Hi,

I use ruff to clean up my pybind11-stubgen generated files.

Since a recent updated, I noticed that the sorting off
```py
import numpy
import numpy as np
```
does not result in stable sorts, flipping the two lines semingly randomly. See the commits `Update Stub Files`, e.g., [here](https://github.com/BLAST-ImpactX/impactx/commit/2cde3d7cdfcdbb3b09411a4baf4ad9c94ce3aa74) and [here](https://github.com/BLAST-ImpactX/impactx/commit/711eb54d48f2feeb46e39014d7554d81117f131e)
https://github.com/BLAST-ImpactX/impactx/commits/development/

While the import appears a bit useless, this is auto-generated code for module bindings of pybind11 and not really controllable/cleanable by hand.

### Version

ruff v0.12.8

---

_Comment by @MichaReiser on 2025-08-13 06:35_

Hmm, interesting and thanks for reporting.

From looking at your `pyproject.toml` I can see that you also configured isort. Can you try disabling isort and only use ruff? Can you tell me more about how you run Ruff, specifically in the situations that resulted in the stub file changes?

Running ruff on 2cde3d7cdfcdbb3b09411a4baf4ad9c94ce3aa74 changes the import order of numpy in `distribution_input_helpers.pyi` to 

```py
from __future__ import annotations

import numpy
import numpy as np
```

Ruff makes more changes to other files that aren't numpy-related.


Ruff doesn't change any numpy import (other than adding a blank line) for 711eb54d48f2feeb46e39014d7554d81117f131e

---

_Label `question` added by @MichaReiser on 2025-08-13 06:36_

---

_Comment by @ax3l on 2025-08-19 16:12_

Thanks @MichaReiser!

I do indeed have the old isort configs in my `pyproject.toml`, but I do not use it in [pre-commits](https://github.com/BLAST-ImpactX/impactx/blob/25.08/.pre-commit-config.yaml) anymore. Is that option for isort interfering?

---

_Comment by @MichaReiser on 2025-08-19 16:31_

It should not. I just wonder if VS code or any other tool picks it up.

---

_Comment by @ax3l on 2025-08-19 16:38_

Yep, so my workflow just to make it as reproducible as possible is:
- I run a [GitHub action](https://github.com/BLAST-ImpactX/impactx/blob/25.08/.github/workflows/stubs.yml) that:
  - [regenerates `.pyi` files](https://github.com/BLAST-ImpactX/impactx/blob/25.08/.github/update_stub.sh) via [pybind11-stubgen](https://github.com/sizmailov/pybind11-stubgen)
  - via [pre-commit](https://github.com/BLAST-ImpactX/impactx/blob/25.08/.github/update_stub.sh) using `pre-commit run -a || true`, I run ruff-format on those files to clean them up and make the produced files stable (e.g., imports)

---

_Comment by @MichaReiser on 2025-08-19 16:45_

Yeah, not sure what's happening. I suggest running ruff locally the next time this happens with the commit before/after.

The only case where we've seen similar issues is if there's a `numpy` folder in your project (even if not commited), because Ruff would then start categorizing `numpy` as first party (because you have a local `numpy` package)

---

_Comment by @ax3l on 2025-08-19 17:48_

Interesting! I exclusively run this in GH actions on `development` and never locally. I always have `numpy` installed there.

---

_Comment by @ax3l on 2025-08-19 19:55_

The flipping I see is between
```py
from __future__ import annotations
import numpy as np
import numpy

__all__: list[str] = ["np", "twiss"]
```
and
```py
from __future__ import annotations
import numpy
import numpy as np

__all__: list[str] = ["np", "twiss"]
```

The `import numpy as np` import is actually unused ([in the `.pyi` file](https://github.com/BLAST-ImpactX/impactx/blob/25.08/src/python/impactx/distribution_input_helpers.pyi)). In the [original `.py` file](https://github.com/BLAST-ImpactX/impactx/blob/25.08/src/python/impactx/distribution_input_helpers.py) it is used.

---

_Comment by @MichaReiser on 2025-08-20 13:43_

Running ruff in your repository (on the given commit) gives me the following output:

```
uvx ruff@latest check --output-format=concise --select I001
Installed 1 package in 1ms
src/python/impactx/MADXParser.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/__init__.pyi:15:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/distribution_input_helpers.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/extensions/KnownElementsList.pyi:10:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/extensions/__init__.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/__init__.pyi:15:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/distribution.pyi:5:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/elements/__init__.pyi:5:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/elements/mixin.pyi:5:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/elements/transformation.pyi:5:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/impactx_pybind/wakeconvolution.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/madx_to_impactx.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
src/python/impactx/plot/__init__.pyi:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 13 errors.
[*] 13 fixable with the `--fix` option.
```

and, for `distributions`, the fix flips the ordering of the numpy import and adds a blank line between the `from __future__` import


However, running pre-commit with `--all-files` doesn't flag any of the erros, unless if I remove the `types_or: [ python ]` configuration. I'm not familiar with pre-commit myself but it seems that `python` excludes `pyi` files? 

Ruff's pre commit uses `types_or: [python, pyi, jupyter]` by default. So you can either remove the `types_or` in your configuration (if you don't have any notebooks or if you're okay with Ruff checking them too), or use `types_or: [python, pyi]` 

---

_Comment by @ax3l on 2025-08-20 15:43_

> However, running pre-commit with --all-files doesn't flag any of the errors, unless if I remove the `types_or: [ python ]` configuration. I'm not familiar with pre-commit myself but it seems that python excludes pyi files?

Yes, I removed  the `pyi` files since pybind11 v3 w/ pybind11-stubgen generated some missing imports. So I only ran ruff formatting on it, not run checks.

I think if I kept the old logic: running ruff check first (which removes the unused import) and then ruff format it would not have the flip issue.

It seems that I have to anyway get the generated `.pyi` code to be valid enough to pass ruff check, which will once it works again remove the unused extra numpy import...

---

_Comment by @MichaReiser on 2025-08-20 15:45_

> It seems that I have to anyway get the generated .pyi code to be valid enough to pass ruff check, which will once it works again remove the unused extra numpy import...

You could consider enabling a subset of the rules (or none at all) for generated files:

https://docs.astral.sh/ruff/settings/#lint_extend-per-file-ignores

---

_Comment by @ax3l on 2025-08-20 16:41_

I worked on a post-generation replace fix for the stubs now so ruff check can run again.

https://github.com/BLAST-ImpactX/impactx/pull/1106

I notice though that it still does the sorting, so I manually rewrote the file that also uses `import numpy as np` so I only see one public import left.

I still think there is a formatting bug in ruff format that causes the unstable rotation, but luckily can avoid the pattern for this one file for now where I saw it.

```py
from __future__ import annotations
import numpy as np
import numpy

__all__: list[str] = ["np", "twiss"]

def twiss(
    beta_x: numpy.float64
   ):
   """Docstring"""
```

now written as
```py
from __future__ import annotations
import numpy

__all__: list[str] = ["numpy", "twiss"]

def twiss(
    beta_x: numpy.float64
   ):
   """Docstring"""
```

---

_Comment by @MichaReiser on 2025-08-20 16:46_

I'm not sure what you mean by unstable rotation. 

Can you share the two inputs that resulted in unstable sorting?

---

_Comment by @futurewasfree on 2025-09-14 10:13_

Same or related here, also I change after 0.13 upgrade. 
Most likely related to this: https://astral.sh/blog/ruff-v0.13.0#full-module-paths-are-now-used-to-verify-first-party-modules
numpy ends up moved to third-party group. Haven't observed the same with 0.12

---

_Label `needs-mre` added by @MichaReiser on 2025-09-15 08:59_

---

_Comment by @MichaReiser on 2025-09-15 08:59_

> Same or related here, also I change after 0.13 upgrade. Most likely related to this: [astral.sh/blog/ruff-v0.13.0#full-module-paths-are-now-used-to-verify-first-party-modules](https://astral.sh/blog/ruff-v0.13.0#full-module-paths-are-now-used-to-verify-first-party-modules) numpy ends up moved to third-party group. Haven't observed the same with 0.12

This sounds correct to me. Do you have a local `numpy` directory (which would explain why the behavior changed with 0.13)?

---

_Comment by @futurewasfree on 2025-09-15 13:17_

nope. I could only find occurrences under `.venv`.  Is there anything else I could check on my side?

---

_Comment by @MichaReiser on 2025-09-15 14:58_

Not sure. Could you share your configuration? If public, could you share the commit where the ordering "changed"?

@dylwil3 do you have any other ideas?

---

_Comment by @dylwil3 on 2025-09-15 15:15_

I think I'd need a little more info to try to reproduce/investigate. Also I am probably misunderstanding something basic: 

> numpy ends up moved to third-party group.

is it not _correct_ to have `numpy` in the "third-party" group?

---

_Comment by @MichaReiser on 2025-09-15 15:39_

> is it not correct to have numpy in the "third-party" group?

It is. I guess at this point it's more about understanding why it changed ;)

---

_Comment by @futurewasfree on 2025-09-16 06:38_

No problem with numpy under 3rd party group :)

My config:

```
fix = true
show-fixes = true
src = ["."]
target-version = "py312"
[format]
quote-style = "double"
[lint]
ignore = ["E712"]
extend-select = ["I"]
```

I also observe way more inconsistencies with a new version:
1. 1st party dependencies are not always alphabetically sorted
2. I see 3rd party dependencies appearing after all 1st parties. E.g. it was the case for `httpx`
3. Sometimes it's even one 1st party group, then `dagster_k8s`, another group of 1st parties.

Overall, unfortunately it seems quite broken in the latest version. Problem is also that VSCode gives me users the latest version of extension (and ruff). So it's either I upgrade and live with all incorrect sorting, or need to install and specify certain binary outside of extension.

Sorry, it's not a public repo and I can not share.

---

_Comment by @MichaReiser on 2025-09-16 07:49_

Hmm, it's difficult to help without a more concrete example (especially given that the error seem to be all over the place). 


Are you only experiencing this issue in VS Code or on the CLI too? Does the result differ if you run ruff without caching (`ruff check --no-cache`)?

---

_Comment by @dylwil3 on 2025-09-16 12:06_

I'm sorry to hear there seems to be some trouble @futurewasfree ! I'm not quite sure how the change would have caused (1) and (3) - In theory, the new behavior should only ever result in some packages that used to be classified as first-party now being classified as third-party.

Certainly the most helpful thing would be something we could reproduce, but one thing you could do to help diagnose yourself would be to inspect the debug output `ruff check -v path/to/yourfile.py` and see if certain packages are being classified as first/third-party that you don't expect. If so, you could see if there are any possible conflicts in the directory structure of your project where certain file paths are named the same as module import paths.

---

_Label `isort` added by @dylwil3 on 2025-09-16 12:31_

---

_Comment by @futurewasfree on 2025-09-16 14:14_

Yes, I’ll try debugging on my own using verbose flags.
I might also create a reproducible example to share publicly when I have time.

---

_Comment by @futurewasfree on 2025-09-21 11:58_

I think I understand what’s happening now: it’s mainly related to editable installs specified via `tool.uv.sources`, which are now being listed as third-party dependencies. This was the main change since 0.12.

I saw a similar thread discussing this, and in my opinion, such dependencies should also be treated as first-party.

For now, the known-first-party setting under `lint.isort` seems like a good workaround.

---

_Comment by @MichaReiser on 2025-09-22 08:26_

@futurewasfree would you be able to create an example project that demonstrates the issue?

Like do you have something like this:

```
libs/
  - utility/
    - python code

services/
  - service-a
	- python code
	- depends on utility
```

Where `utility` is a dependency of `service` and installed as an editable install? 

If so, you can try using [`src`](https://docs.astral.sh/ruff/settings/#src) and set it to `src = ["libs", "services"]`



---

_Closed by @MichaReiser on 2025-12-10 17:31_

---

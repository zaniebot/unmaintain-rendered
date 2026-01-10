```yaml
number: 1840
title: "Can't import rlax library when installed via uv install"
type: issue
state: closed
author: Artur-Galstyan
labels:
  - bug
assignees: []
created_at: 2024-02-21T22:53:53Z
updated_at: 2024-02-24T13:31:36Z
url: https://github.com/astral-sh/uv/issues/1840
synced_at: 2026-01-10T05:40:32Z
```

# Can't import rlax library when installed via uv install

---

_Issue opened by @Artur-Galstyan on 2024-02-21 22:53_

Hi there. First off, I'm loving this project!

Here's the bug and how to reproduce it.

1. In an empty folder run `uv venv`. (I'm using Python 3.11.6, though I don't think that matters here)
2. Run `source .venv/bin/activate`
3. Run `uv pip install rlax`
4. After it's installed, create a file and in it import rlax like so: `import rlax`
5. Then get this very strange error

<details>
<summary>Toggle me to see the long error </summary>
<br>
ImportError                               Traceback (most recent call last)
Cell In[1], line 3
      1 import jax.numpy as jnp
      2 import jax
----> 3 import rlax

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/rlax/__init__.py:24](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/rlax/__init__.py#line=23)
     22 from rlax._src.clipping import clip_gradient
     23 from rlax._src.clipping import huber_loss
---> 24 from rlax._src.distributions import categorical_cross_entropy
     25 from rlax._src.distributions import categorical_importance_sampling_ratios
     26 from rlax._src.distributions import categorical_kl_divergence

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/rlax/_src/distributions.py:28](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/rlax/_src/distributions.py#line=27)
     25 import warnings
     27 import chex
---> 28 import distrax
     29 import jax.numpy as jnp
     31 Array = chex.Array

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/distrax/__init__.py:18](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/distrax/__init__.py#line=17)
     15 """Distrax: Probability distributions in JAX."""
     17 # Bijectors.
---> 18 from distrax._src.bijectors.bijector import Bijector
     19 from distrax._src.bijectors.bijector import BijectorLike
     20 from distrax._src.bijectors.block import Block

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/distrax/_src/bijectors/bijector.py:27](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/distrax/_src/bijectors/bijector.py#line=26)
     23 import jax.numpy as jnp
     24 from tensorflow_probability.substrates import jax as tfp
---> 27 tfb = tfp.bijectors
     29 Array = chex.Array
     32 class Bijector(jittable.Jittable, metaclass=abc.ABCMeta):

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/lazy_loader.py:53](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/lazy_loader.py#line=52), in LazyLoader.__getattr__(self, item)
     52 def __getattr__(self, item):
---> 53   module = self._load()
     54   return getattr(module, item)

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/lazy_loader.py:40](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/lazy_loader.py#line=39), in LazyLoader._load(self)
     38   self._on_first_access = None
     39 # Import the target module and insert it into the parent's namespace
---> 40 module = importlib.import_module(self.__name__)
     41 if self._parent_module_globals is not None:
     42   self._parent_module_globals[self._local_name] = module

File [~/.pyenv/versions/3.11.6/lib/python3.11/importlib/__init__.py:126](http://localhost:8888/~/.pyenv/versions/3.11.6/lib/python3.11/importlib/__init__.py#line=125), in import_module(name, package)
    124             break
    125         level += 1
--> 126 return _bootstrap._gcd_import(name[level:], package, level)

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/__init__.py:41](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/__init__.py#line=40)
     39 from tensorflow_probability.python.version import __version__
     40 # from tensorflow_probability.substrates.jax.google import staging  # DisableOnExport  # pylint:disable=line-too-long
---> 41 from tensorflow_probability.substrates.jax import bijectors
     42 from tensorflow_probability.substrates.jax import distributions
     43 from tensorflow_probability.substrates.jax import experimental

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/bijectors/__init__.py:19](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/bijectors/__init__.py#line=18)
     15 """Bijective transformations."""
     17 # pylint: disable=unused-import,wildcard-import,line-too-long,g-importing-member
---> 19 from tensorflow_probability.substrates.jax.bijectors.absolute_value import AbsoluteValue
     20 from tensorflow_probability.substrates.jax.bijectors.ascending import Ascending
     21 # from tensorflow_probability.substrates.jax.bijectors.batch_normalization import BatchNormalization

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/bijectors/absolute_value.py:17](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/substrates/jax/bijectors/absolute_value.py#line=16)
      1 # Copyright 2018 The TensorFlow Probability Authors.
      2 #
      3 # Licensed under the Apache License, Version 2.0 (the "License");
   (...)
     13 # limitations under the License.
     14 # ============================================================================
     15 """AbsoluteValue bijector."""
---> 17 from tensorflow_probability.python.internal.backend.jax.compat import v2 as tf
     19 from tensorflow_probability.substrates.jax.bijectors import bijector
     20 from tensorflow_probability.substrates.jax.internal import assert_util

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/__init__.py:18](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/__init__.py#line=17)
     15 """Experimental Numpy backend."""
     17 from tensorflow_probability.python.internal.backend.jax import __internal__
---> 18 from tensorflow_probability.python.internal.backend.jax import bitwise
     19 from tensorflow_probability.python.internal.backend.jax import compat
     20 from tensorflow_probability.python.internal.backend.jax import config

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/bitwise.py:19](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/bitwise.py#line=18)
     15 """Numpy bitwise ops."""
     17 import numpy as onp; import jax.numpy as np
---> 19 from tensorflow_probability.python.internal.backend.jax import _utils as utils
     21 __all__ = [
     22     'bitwise_xor',
     23     'left_shift',
     24 ]
     27 bitwise_xor = utils.copy_docstring(
     28     'tf.bitwise.bitwise_xor',
     29     lambda x, y, name=None: np.bitwise_xor(x, y))  # pylint: disable=unused-argument

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/_utils.py:22](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/_utils.py#line=21)
     19 import types
     21 import numpy as onp; import jax.numpy as np
---> 22 from tensorflow_probability.python.internal.backend.jax import nest
     24 try:
     25   from tensorflow.python.ops import array_ops  # pylint: disable=g-direct-tensorflow-import,g-import-not-at-top,unused-import

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/nest.py:30](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tensorflow_probability/python/internal/backend/jax/nest.py#line=29)
     28 from collections import abc as collections_abc
     29 import types
---> 30 import tree as dm_tree
     32 # pylint: disable=g-import-not-at-top
     33 try:

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tree/__init__.py:23](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tree/__init__.py#line=22)
     20 import sys
     21 from typing import Mapping, Sequence, Text, TypeVar, Union
---> 23 from .sequence import _is_attrs
     24 from .sequence import _is_namedtuple
     25 from .sequence import _sequence_like

File [~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tree/sequence.py:19](http://localhost:8888/~/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tree/sequence.py#line=18)
     17 from collections import abc as collections_abc
     18 import types
---> 19 from tree import _tree
     21 # pylint: disable=g-import-not-at-top
     22 try:

ImportError: dlopen(/Users/arturgalstyan/Workspace/jaxRL/.venv/lib/python3.11/site-packages/tree/_tree.cpython-311-darwin.so, 0x0002): symbol not found in flat namespace '__ZN4absl12lts_2021032416strings_internal9CatPiecesESt16initializer_listINS0_11string_viewEE'
</details>

If you run the exact same steps - without `uv` of course and using the vanilla `pip` experience, then this error does not happen!

Edit: [Link to the rlax package in question](https://github.com/google-deepmind/rlax/tree/master)

---

_Label `bug` added by @charliermarsh on 2024-02-21 22:54_

---

_Comment by @Artur-Galstyan on 2024-02-21 22:56_

Probably/maybe relevant: I'm on a Mac! 

---

_Comment by @charliermarsh on 2024-02-21 22:59_

Thanks!

---

_Comment by @hauntsaninja on 2024-02-21 23:35_

I was sort of able to repro (got a different error, but pip worked) when using arm64 Python on macOS. I think there's something incorrect about uv's handling of arm64 vs x86_64 deps.

---

_Comment by @charliermarsh on 2024-02-22 00:06_

Interesting, will take a look... Perhaps we have a bug in our tags somewhere.

---

_Comment by @charliermarsh on 2024-02-22 00:52_

It looks like we're using the universal wheel for `dm-tree`, while pip is using the ARM wheel.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-22 01:01_

---

_Closed by @charliermarsh on 2024-02-22 01:38_

---

_Comment by @charliermarsh on 2024-02-22 05:00_

Should be fixed in v0.1.7 (out now).

---

_Comment by @Artur-Galstyan on 2024-02-22 07:05_

Absolutely amazing! Thanks!

---

_Comment by @Artur-Galstyan on 2024-02-24 09:09_

@charliermarsh I think we might have to reopen this issue :( 

I've tested it once again on uv 0.10 and I still get the same error 

<details>
<summary> Click me to show the error </summary>
<code>
➜  testing_ground1 uv venv
Using Python 3.11.6 interpreter at /Users/arturgalstyan/.pyenv/versions/3.11.6/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
➜  testing_ground1 source .venv/bin/activate
(testing_ground1) ➜  testing_ground1 uv pip install rlax
Resolved 19 packages in 93ms
Installed 19 packages in 81ms
 + absl-py==2.1.0
 + chex==0.1.85
 + cloudpickle==3.0.0
 + decorator==5.1.1
 + distrax==0.1.5
 + dm-env==1.6
 + dm-tree==0.1.8
 + gast==0.5.4
 + jax==0.4.24
 + jaxlib==0.4.24
 + ml-dtypes==0.3.2
 + numpy==1.26.4
 + opt-einsum==3.3.0
 + rlax==0.1.6
 + scipy==1.12.0
 + six==1.16.0
 + tensorflow-probability==0.23.0
 + toolz==0.12.1
 + typing-extensions==4.9.0
(testing_ground1) ➜  testing_ground1 python3 test.py 
Traceback (most recent call last):
  File "/Users/arturgalstyan/Workspace/testing_ground1/test.py", line 1, in <module>
    import rlax
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/rlax/__init__.py", line 24, 
in <module>
    from rlax._src.distributions import categorical_cross_entropy
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/rlax/_src/distributions.py",
 line 28, in <module>
    import distrax
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/distrax/__init__.py", line 1
8, in <module>
    from distrax._src.bijectors.bijector import Bijector
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/distrax/_src/bijectors/bijec
tor.py", line 27, in <module>
    tfb = tfp.bijectors
          ^^^^^^^^^^^^^
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/lazy_loader.py", line 53, in __getattr__
    module = self._load()
             ^^^^^^^^^^^^
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/lazy_loader.py", line 40, in _load
    module = importlib.import_module(self.__name__)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/arturgalstyan/.pyenv/versions/3.11.6/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/subst
rates/jax/__init__.py", line 41, in <module>
    from tensorflow_probability.substrates.jax import bijectors
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/subst
rates/jax/bijectors/__init__.py", line 19, in <module>
    from tensorflow_probability.substrates.jax.bijectors.absolute_value import AbsoluteValue
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/subst
rates/jax/bijectors/absolute_value.py", line 17, in <module>
    from tensorflow_probability.python.internal.backend.jax.compat import v2 as tf
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/backend/jax/__init__.py", line 18, in <module>
    from tensorflow_probability.python.internal.backend.jax import bitwise
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/backend/jax/bitwise.py", line 19, in <module>
    from tensorflow_probability.python.internal.backend.jax import _utils as utils
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/backend/jax/_utils.py", line 22, in <module>
    from tensorflow_probability.python.internal.backend.jax import nest
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tensorflow_probability/pytho
n/internal/backend/jax/nest.py", line 30, in <module>
    import tree as dm_tree
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tree/__init__.py", line 23, 
in <module>
    from .sequence import _is_attrs
  File "/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tree/sequence.py", line 19, 
in <module>
    from tree import _tree
ImportError: dlopen(/Users/arturgalstyan/Workspace/testing_ground1/.venv/lib/python3.11/site-packages/tree/_tree.cpyth
on-311-darwin.so, 0x0002): symbol not found in flat namespace '__ZN4absl12lts_2021032416strings_internal9CatPiecesESt1
6initializer_listINS0_11string_viewEE'
(testing_ground1) ➜  testing_ground1 uv --version
uv 0.1.10
</code>
</details>

`test.py` simply imports `rlax` and does nothing else.

---

_Comment by @charliermarsh on 2024-02-24 11:50_

Can you try running “uv cache clean” beforehand? Otherwise, uv will contain to use the universal wheel.

---

_Comment by @Artur-Galstyan on 2024-02-24 13:31_

Indeed, that did the trick! Thanks!

---

---
number: 9664
title: "[Feature Request] [NPY] Detect accidental cubic runtime cost due to `numpy.trace`"
type: issue
state: open
author: randolf-scholz
labels:
  - rule
assignees: []
created_at: 2024-01-29T00:56:35Z
updated_at: 2025-06-16T17:26:31Z
url: https://github.com/astral-sh/ruff/issues/9664
synced_at: 2026-01-10T01:22:49Z
---

# [Feature Request] [NPY] Detect accidental cubic runtime cost due to `numpy.trace`

---

_Issue opened by @randolf-scholz on 2024-01-29 00:56_

There is a very common mistake people make in numerical code that incurs an accidental cubic runtime cost: They implement `np.trace(A @ B)`, which costs $O(n^3)$ instead of the equivalent `np.tensordot(A.T, B)` which costs $O(n^2)$, or `np.einsum` if batch dimensions are involved. (see [Frobenius Inner Product](https://en.wikipedia.org/wiki/Frobenius_inner_product))

[GitHub is full of this issue](https://github.com/search?q=np.trace+%40&type=code), but I don't blame people, since book authors should take more care to point this out. This not just affects `numpy`, but basically all related numerical libraries (`jax`, `torch`, `tensorflow`, etc.)

Basically, one needs to look out for `trace` being used with a matrix multiplication inside. A simple check would look something like:

```pyhton
match node:
    case Call(
        func=Attribute(value=Name(id="np"), attr="trace"),
        args=[BinOp(op=MatMult()) | Call(func=Attribute(attr="dot"))],
    ):
        # use np.tensordot instead.
```

One can probably make a more clever version that also detect cases like `np.trace(2*(A@B))`.

---

_Label `rule` added by @charliermarsh on 2024-01-29 00:59_

---

_Comment by @randolf-scholz on 2024-01-29 01:52_

Probably one should be a bit conservative here, since transposing can also incur a surprisingly large cost, and only flag cases where one doesn't need to transpose. A few benchmarks:

|                        | 4       | 16      | 64      | 256     | 512    | 1024   | 2048   |
|------------------------|---------|---------|---------|---------|--------|--------|--------|
| `np.trace(A @ B)`      | 3.78 µs | 4.37 µs | 17.9 µs | 376 µs  | 2.2 ms | 11 ms  | 66 ms  |
| `np.trace(A.T @ B)`    | 3.93 µs | 4.47 µs | 17.4 µs | 482 µs  | 2.2 ms | 10 ms  | 76 ms  |
| `np.tensordot(A, B)`   | 12.8 µs | 12.8 µs | 12.9 µs | 47.4 µs | 44 µs  | 76 µs  | 471 µs |
| `np.tensordot(A.T, B)` | 13.3 µs | 13.7 µs | 15.9 µs | 511 µs  | 1.6 ms | 6.8 ms | 42 ms  |


`np.tensordot(A, B)` has some overhead for small inputs, but is orders of magnitude so for large inputs. `np.tensordot(A.T, B)` is a bit faster than `np.trace(A @ B)` for large inputs, but the transposition overhead hurts.

So, the bad cases one wants to find are:

1. Basic case: `np.trace(A.T.dot(B))`, `np.trace(A.T @ B)`, `np.trace(A.dot(B.T))`, `np.trace(A @ B.T)` ⇝ use `np.tensordot(A, B)` 
2. Basic case with scalar: `np.trace(c*A.T.dot(B))` or `np.trace(A.T.dot(c*B))`, etc. ⇝ use `c*np.tensordot(A, B)` 
3. More complex cases: such as this one [`np.trace(ct.T @ c0_inv @ ct @ c0_inv)`](https://github.com/statsmodels/statsmodels/blob/23faea30e30ff759c3c9d3f1d57864b2aa68c827/statsmodels/tsa/vector_ar/vecm.py#L2281) are likely much harder to detect without also generating false positives. (should be `tensordot(z, z)` with `z=c0_inv @ ct`, but requires knowledge that `c0_inv` is symmetric)

The last one also does [another sin, they invert](https://www.johndcook.com/blog/2010/01/19/dont-invert-that-matrix/) the `sigma_u` matrix, which is usually bad practice. In this example, one can make the whole thing $O(n^2)$ by using Cholesky factorization instead of inverting and tensordot instead of trace.





---

_Referenced in [numpy/numpy#25713](../../numpy/numpy/issues/25713.md) on 2024-01-29 02:43_

---

_Referenced in [astral-sh/ruff#12349](../../astral-sh/ruff/issues/12349.md) on 2024-07-18 11:56_

---

_Comment by @randolf-scholz on 2024-07-18 12:29_

<details> <summary>Reran the script with torch==2.3.1 and numpy==2.0.0 </summary>

```python
import torch as pt
N = 64
A = pt.randn(N, N)
B = pt.randn(N, N)
%timeit pt.trace(A @ B)
%timeit pt.trace(A.T @ B)
%timeit pt.tensordot(A, B)
%timeit pt.tensordot(A.T, B)
```

```python
import numpy as np
N = 64
A = np.random.randn(N, N)
B = np.random.randn(N, N)
%timeit np.trace(A @ B)
%timeit np.trace(A.T @ B)
%timeit np.tensordot(A, B)
%timeit np.tensordot(A.T, B)
```

</details>

Results:

|                      | 64      | 256    | 1024    | 2048   |
|----------------------|--------:|-------:|--------:|-------:|
| np.trace(A @ B)      | 11.7 μs | 852 μs | 6730 μs | 108 ms |
| np.trace(A.T @ B)    | 11.9 μs | 971 μs | 7670 μs | 115 ms |
| np.tensordot(A, B)   | 10.9 μs | 23 μs  |   90 μs | 2 ms   |
| np.tensordot(A.T, B) | 13.5 μs | 120 μs | 2920 μs | 44 ms  |
| pt.trace(A @ B)      | 6.3 μs  | 51 μs  | 2610 μs | 22 ms  |
| pt.trace(A.T @ B)    | 7.9 μs  | 53 μs  | 2650 μs | 23 ms  |
| pt.tensordot(A, B)   | 30.5 μs | 269 μs | 4040 μs | 16 ms  |
| pt.tensordot(A.T, B) | 38.8 μs | 309 μs | 4740 μs | 19 ms  |

In `numpy` the overhead is patched, but `torch` is slower until very large arrays. I'll open an issue.

---

_Referenced in [pytorch/pytorch#145731](../../pytorch/pytorch/issues/145731.md) on 2025-01-27 14:58_

---

```yaml
number: 957
title: set a lower limit for size of literal unions we will do precise type inference of operations for
type: issue
state: closed
author: carljm
labels:
  - performance
  - fatal
  - set-theoretic types
assignees: []
created_at: 2025-08-08T17:17:30Z
updated_at: 2025-12-05T02:01:50Z
url: https://github.com/astral-sh/ty/issues/957
synced_at: 2026-01-12T15:54:24Z
```

# set a lower limit for size of literal unions we will do precise type inference of operations for

---

_@carljm_

For example, we know that if we add `Literal[1, 2]` to `Literal[3]`, we get `Literal[4, 5]`. But in some cyclic scenarios, something like `x += 1` can result in an ever-expanding union (until we hit the Salsa cycle limit). We currently set an overall limit of 100 on literal-unions size, which is lower than the 200 Salsa cycle limit, but this still means we can iterate 100 times before we converge -- that's a lot.

We don't want to decrease our overall literal-unions size limit any lower than 100 (in fact ideally we'd increase it -- people should be able to explicitly create large literal unions). But we should set a much _lower_ limit on the size of literal unions we are willing to do literal-math on (and beyond that limit we should just fall back to e.g. `int`). I think a limit in the low double-digits would be reasonable here. This would also significantly reduce the amount of fixpoint iteration needed in cyclic cases before we converge.

(This should apply not just to literal ints, but also operations on literal strings and bytes, too.)



---

_Label `performance` added by @carljm on 2025-08-08 17:17_

---

_Label `set-theoretic types` added by @carljm on 2025-08-08 17:17_

---

_Comment by @erictraut on 2025-08-08 17:51_

In case it's helpful, here are [the conditions](https://github.com/microsoft/pyright/blob/8b02aa84ec916697e76d5ee69299a89d555ec98c/packages/pyright-internal/src/analyzer/operations.ts#L587) under which pyright performs literal math. 

1. The operation cannot be in a loop (a `for` or `while`)
2. The operation must be acting on a local variable, as opposed to an attribute or subscript expression or a variable defined in some outer scope
3. The resulting union cannot be greater than 64 in size (a somewhat arbitrary number, but one that seems to work well in practice)

---

_Comment by @carljm on 2025-08-20 17:54_

This would fix #660.

---

_Assigned to @sharkdp by @sharkdp on 2025-08-22 13:59_

---

_Comment by @MichaReiser on 2025-09-22 13:10_

Another instance of this:


```py
class C:
    def __init__(self: "C"):
        self.x1 = 0
        self.x2 = 1
        self.x3 = 0
     
    def f1(self: "C"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2

    def f2(self: "C"):
        self.x1 = self.x2 + self.x3
        self.x2 = self.x1 + self.x3
        self.x3 = self.x1 + self.x2
```



---

_Added to milestone `GA` by @carljm on 2025-09-22 20:14_

---

_Unassigned @sharkdp by @sharkdp on 2025-09-30 07:59_

---

_Comment by @carljm on 2025-10-24 15:23_

Another case (from #1423 ):

```py
from typing import Self

class A:
    def __init__(self: Self):
        self.a = 0
        self.b = 0

    def foo(self: Self):
        if self.a:
            self.b = self.b + 1

    def bar(self: Self):
        if str(self.b):
            self.a = self.b
```

---

_Comment by @mtshiba on 2025-10-28 17:25_

I think we can leverage the approach in astral-sh/ruff#20566 for type inference of recursively defined values.
That is, by using `Divergent` as a "cycle marker", we can detect that "the value is recursively defined".
Conversely, as long as an expression is not recursively defined, we can assume that the union type of the value is at most a finite set, and it can be as large as the user desires.

However, it would be impossible to generally distinguish between expressions that are "recursively defined but have finite possible states" and expressions that have "infinitely many possible states" - this would be as difficult as the halting problem, or even more.

However, I think the following heuristics might work well:
* If the input types (types involved in union) are all finite (e.g., bool, a tuple of finite types), the output set is also finite. In this case, we may not need to specify threshold.
* If the input types are infinite (e.g., int, str), the output set may be infinite. If the value is recursively defined, widening is performed at a certain threshold (smaller than the current setting).

There is a possibility that the finite saturation case may be widened by mistake, but I don't think this will be a serious problem in most practical cases.

---

_Comment by @carljm on 2025-10-30 15:58_

@mtshiba It seems to me that the fix described in the OP here (limiting the size of literal unions we are willing to create, as pyright does) would address all problematic cases arising from potentially-infinite unions of literals, without any need for the use of `Divergent`, and no other significant downsides. It wouldn't address e.g. infinitely-nested generic types (those are the cases where we need `Divergent`). I don't think solving this issue requires distinguishing "finite" vs "potentially infinite"[^1] types, or determining whether an expression is recursively defined (beyond what that already happens via Salsa and fixpoint iteration.)

[^1]: Technically all of these types are finite in ty. We only support integer literals up to what fits in an i64, and we limit the length of string and bytes literals. But obviously in both cases the number of possible values is far beyond what we can handle in a union with good performance, or reach in fixpoint iteration within an acceptable number of iterations.

---

_Label `fatal` added by @carljm on 2025-10-30 15:59_

---

_Comment by @carljm on 2025-10-30 16:01_

Since (with type-of-self landed) this is now showing up as a major performance regression in real user codebases, I think it is higher priority to fix. Tempted to mark it for beta milestone.

---

_Comment by @mtshiba on 2025-11-04 16:59_

> [@mtshiba](https://github.com/mtshiba) It seems to me that the fix described in the OP here (limiting the size of literal unions we are willing to create, as pyright does) would address all problematic cases arising from potentially-infinite unions of literals, without any need for the use of `Divergent`, and no other significant downsides. It wouldn't address e.g. infinitely-nested generic types (those are the cases where we need `Divergent`). I don't think solving this issue requires distinguishing "finite" vs "potentially infinite"[1](#user-content-fn-1-7647dddaba5a72a3ba2e68914f1654e5) types, or determining whether an expression is recursively defined (beyond what that already happens via Salsa and fixpoint iteration.)

The examples I'm thinking of are as follows:

```python
from typing import Literal

def f(bit: Literal[0, 1]):
   i1 = bit
   i2 = 2*i1 + bit
   i3 = 2*i2 + bit
   i4 = 2*i3 + bit
   i5 = 2*i4 + bit
   i6 = 2*i5 + bit
   i7 = 2*i6 + bit
   i8 = 2*i7 + bit

   reveal_type(i1)  # Literal[0, 1]
   reveal_type(i2)  # Literal[0, 1, 2, 3]
   reveal_type(i3)  # Literal[0, 1, 2, 3, 4, 5, 6, 7]
   reveal_type(i4)  # ...
   reveal_type(i5)
   reveal_type(i6)
   reveal_type(i7)
   reveal_type(i8)
```

The type of `i8` is expected to be a literal type with 256 elements, but since both pyright and ty perform widening beforehand, it becomes `int`. However, in terms of computational load, there should still be a fair bit of headroom.

On the other hand, widening should be performed promptly in the following case:

 ```python
class C:
    def __init__(self):
        self.i = 0

    def inc(self):
       self.i = self.i + 1

reveal_type(C().i)
 ```

---

What I propose from the discussion so far is to define two thresholds for widening.

For values ​​that are not recursively defined, allow literal types to have a significantly larger number of literal elements than the current threshold (the reason for the threshold is simply to prevent literals from being created with unrealistic number of elements, which would put a huge strain on the computer). I don't think literal-math will be too costly for a few hundred elements, since the fixed point calculation converges in one go.

For values ​​that are recursively defined, set the threshold smaller than the current one to speed up the convergence of fixed-point iteration.

Setting a single threshold without recursion tracking forces a trade-off between the desire to allow literal types with as many elements as possible for non-recursively defined value types and the desire to allow calculations for recursively defined value types to converge as quickly as possible.




---

_Comment by @mtshiba on 2025-11-04 17:11_

The above proposal does not preclude work to lower the limit in order to improve performance sooner.
For values ​​that are not defined recursively, the limit can be raised later.

---

_Comment by @carljm on 2025-11-04 17:35_

Yes, that all makes sense. I think if we are able to reliably detect recursively-defined values, the two thresholds is certainly nice to have, but it will also be OK, probably for quite a while, if we share a single (low) threshold. That is, it's OK if the first example widens fairly quickly (even already by i4), and it's probably also OK if the second example iterates a couple tens of times.

---

_Comment by @mtshiba on 2025-11-28 09:23_

astral-sh/ruff#20566 has been merged, so I'll try to see if my idea works.

---

_Closed by @carljm on 2025-12-05 02:01_

---

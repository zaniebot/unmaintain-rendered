```yaml
number: 8758
title: Consider conditionally applying rule PLR6201
type: issue
state: closed
author: ofek
labels:
  - rule
assignees: []
created_at: 2023-11-18T20:37:12Z
updated_at: 2024-10-24T09:05:34Z
url: https://github.com/astral-sh/ruff/issues/8758
synced_at: 2026-01-10T11:09:51Z
```

# Consider conditionally applying rule PLR6201

---

_Issue opened by @ofek on 2023-11-18 20:37_

When the sequences are small, the cost of instantiation of a set in fact negatively impacts performance:

```
❯ python -m timeit "{0, '0', 9, '9', 8, '8', 7, '7', 6, '6'}"
2000000 loops, best of 5: 105 nsec per loop
❯ python -m timeit "[0, '0', 9, '9', 8, '8', 7, '7', 6, '6']"
5000000 loops, best of 5: 43.6 nsec per loop
❯ python -m timeit "(0, '0', 9, '9', 8, '8', 7, '7', 6, '6')"
50000000 loops, best of 5: 8.45 nsec per loop
❯ python -m timeit -s "seq = {0, '0', 9, '9', 8, '8', 7, '7', 6, '6'}" "0 in seq"
10000000 loops, best of 5: 20.6 nsec per loop
❯ python -m timeit -s "seq = [0, '0', 9, '9', 8, '8', 7, '7', 6, '6']" "0 in seq"
20000000 loops, best of 5: 17.9 nsec per loop
❯ python -m timeit -s "seq = (0, '0', 9, '9', 8, '8', 7, '7', 6, '6')" "0 in seq"
20000000 loops, best of 5: 17.5 nsec per loop
❯ python -m timeit -s "seq = {0, '0', 9, '9', 8, '8', 7, '7', 6, '6'}" "6 in seq"
10000000 loops, best of 5: 19.8 nsec per loop
❯ python -m timeit -s "seq = [0, '0', 9, '9', 8, '8', 7, '7', 6, '6']" "6 in seq"
5000000 loops, best of 5: 85.5 nsec per loop
❯ python -m timeit -s "seq = (0, '0', 9, '9', 8, '8', 7, '7', 6, '6')" "6 in seq"
5000000 loops, best of 5: 90.1 nsec per loop
```

---

_Label `rule` added by @charliermarsh on 2023-11-18 23:07_

---

_Comment by @tdulcet on 2023-11-20 12:12_

There are optimizations here that are not accounted for in your benchmarks. See [What’s New In Python 3.2](https://docs.python.org/3/whatsnew/3.2.html#optimizations) for example. Also see #8322 and https://github.com/pylint-dev/pylint/issues/4776.

---

_Comment by @flying-sheep on 2023-11-20 13:34_

Yup, with these, the `x in {...}` pattern wins out against list and tuple literals unless it’s the first element:

```console
$ python -m timeit '0 in {0, "0", 9, "9", 8, "8", 7, "7", 6, "6"}'
20000000 loops, best of 5: 13.6 nsec per loop
$ python -m timeit '0 in [0, "0", 9, "9", 8, "8", 7, "7", 6, "6"]'
20000000 loops, best of 5: 11.5 nsec per loop
$ python -m timeit '0 in (0, "0", 9, "9", 8, "8", 7, "7", 6, "6")'
20000000 loops, best of 5: 11.4 nsec per loop
$ python -m timeit '6 in {0, "0", 9, "9", 8, "8", 7, "7", 6, "6"}'
20000000 loops, best of 5: 12.9 nsec per loop
$ python -m timeit '6 in [0, "0", 9, "9", 8, "8", 7, "7", 6, "6"]'
5000000 loops, best of 5: 52.3 nsec per loop
$ python -m timeit '6 in (0, "0", 9, "9", 8, "8", 7, "7", 6, "6")'
5000000 loops, best of 5: 56.5 nsec per loop
```

The manual version is maybe even a tiny bit faster, but this is noise level.

```console
$ python -m timeit -s 'seq = {0, "0", 9, "9", 8, "8", 7, "7", 6, "6"}' '0 in seq'
20000000 loops, best of 5: 12.5 nsec per loop
$ python -m timeit -s 'seq = [0, "0", 9, "9", 8, "8", 7, "7", 6, "6"]' '0 in seq'
20000000 loops, best of 5: 11.2 nsec per loop
$ python -m timeit -s 'seq = (0, "0", 9, "9", 8, "8", 7, "7", 6, "6")' '0 in seq'
20000000 loops, best of 5: 10.5 nsec per loop
$ python -m timeit -s 'seq = {0, "0", 9, "9", 8, "8", 7, "7", 6, "6"}' '6 in seq'
20000000 loops, best of 5: 15.3 nsec per loop
$ python -m timeit -s 'seq = [0, "0", 9, "9", 8, "8", 7, "7", 6, "6"]' '6 in seq'
5000000 loops, best of 5: 53.8 nsec per loop
$ python -m timeit -s 'seq = (0, "0", 9, "9", 8, "8", 7, "7", 6, "6")' '6 in seq'
5000000 loops, best of 5: 49.6 nsec per loop
```

---

_Comment by @oprypin on 2023-11-20 13:38_

@flying-sheep Thanks! Could you also please run an equivalent benchmark for a 3-item collection? 🙏

---

_Comment by @flying-sheep on 2023-11-20 13:42_

Microoptimizations like this are rarely indicative of real life performance. I’m simply pointing out that the optimization mentioned by @tdulcet is indeed applied, so using a inline set literal is preferable, as it’s equivalent to creating a frozenset ahead of time.

But sure, here you go. Finding the 3rd item in a sequence is slower than finding it in a set, only finding the first item in a sequence is very very slightly faster than finding that item in a set.

So `if x in {...}` is best. Ruff does it right.

----

Peephole optimized:

```console
$ python -m timeit '0 in {0, "0", 9}'
20000000 loops, best of 5: 13.7 nsec per loop
$ python -m timeit '0 in [0, "0", 9]'
20000000 loops, best of 5: 11.4 nsec per loop
$ python -m timeit '0 in (0, "0", 9)'
20000000 loops, best of 5: 11.3 nsec per loop
$ python -m timeit '9 in {0, "0", 9}'
20000000 loops, best of 5: 12.8 nsec per loop
$ python -m timeit '9 in [0, "0", 9]'
10000000 loops, best of 5: 21.4 nsec per loop
$ python -m timeit '9 in (0, "0", 9)'
10000000 loops, best of 5: 21.5 nsec per loop
```

… has the same performance as manually optimized:

```console
$ python -m timeit -s 'seq = {0, "0", 9}' '0 in seq'
20000000 loops, best of 5: 12.7 nsec per loop
$ python -m timeit -s 'seq = [0, "0", 9]' '0 in seq'
20000000 loops, best of 5: 11.3 nsec per loop
$ python -m timeit -s 'seq = (0, "0", 9)' '0 in seq'
20000000 loops, best of 5: 10.5 nsec per loop
$ python -m timeit -s 'seq = {0, "0", 9}' '9 in seq'
20000000 loops, best of 5: 12.2 nsec per loop
$ python -m timeit -s 'seq = [0, "0", 9]' '9 in seq'
10000000 loops, best of 5: 21.7 nsec per loop
$ python -m timeit -s 'seq = (0, "0", 9)' '9 in seq'
20000000 loops, best of 5: 20 nsec per loop
```

---

_Comment by @zanieb on 2023-11-20 15:31_

Thanks for the details @flying-sheep 

---

_Closed by @zanieb on 2023-11-20 15:31_

---

_Comment by @ashrub-holvi on 2024-10-24 09:05_

Would be nice to add link to this ticket to docs as many people trying to benchmark it:
```
diff --git a/crates/ruff_linter/src/rules/pylint/rules/literal_membership.rs b/crates/ruff_linter/src/rules/pylint/rules/literal_membership.rs
index 7d0031ab0..0339c032c 100644
--- a/crates/ruff_linter/src/rules/pylint/rules/literal_membership.rs
+++ b/crates/ruff_linter/src/rules/pylint/rules/literal_membership.rs
@@ -31,6 +31,7 @@ use crate::checkers::ast::Checker;
 ///
 /// ## References
 /// - [What’s New In Python 3.2](https://docs.python.org/3/whatsnew/3.2.html#optimizations)
+/// - [How to benchmark it properly](https://github.com/astral-sh/ruff/issues/8758)
 #[violation]
 pub struct LiteralMembership;
```

---

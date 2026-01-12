```yaml
number: 18434
title: "SIM118 maybe shouldn't fire on `for key in dict.keys()`"
type: issue
state: open
author: zackw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-02T20:08:26Z
updated_at: 2025-06-03T13:10:54Z
url: https://github.com/astral-sh/ruff/issues/18434
synced_at: 2026-01-12T15:54:56Z
```

# SIM118 maybe shouldn't fire on `for key in dict.keys()`

---

_@zackw_

### Summary

[The documentation for SIM118](https://docs.astral.sh/ruff/rules/in-dict-keys/) makes it sound like it applies specifically to `if k in d.keys()` and similar, but it also fires on `for k in d.keys()`:

```
def f(d: dict):
    for k in d:
        print(k)

def g(d: dict):
    for k in d.keys():  # Use `key in dict` instead of `key in dict.keys()` (SIM118)
        print(k)
```

(playground: <https://play.ruff.rs/4f8a2dce-14a4-4de9-a41e-cbe523721349>)

Whenever the dictionary is being _iterated over_, rather than checking for membership of a single element, I think the case for insisting on bare `k in d` is much weaker and in fact I personally want the _exact opposite_ lint, because I have often made the mistake of writing `for k, v in d` when I meant to write `for k, v in dict.items()`.

Therefore I request that the effect of SIM118 be restricted to situations like `if k in d.keys()`, and that new lints be added for _both_ `for k in d.keys()` => `for k in d` _and_ `for k in d` => `for k in d.keys()`.

---

_Comment by @ntBre on 2025-06-02 20:46_

On the one hand, this looks very intentional in the current code:

https://github.com/astral-sh/ruff/blob/87a15f7e4cf328e6485cee98960d19f51a955ef4/crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs#L163-L172

but on the other hand, this appears to be a departure from the upstream rule:

```shell
$ cat <<'EOF' | uvx --with flake8-simplify flake8 --select SIM118 -
def f(d: dict):
    "" in d.keys()  # error
    for k in d.keys(): ...  # okay
EOF
stdin:2:5: SIM118 Use '"" in d' instead of '"" in d.keys()'
```

I personally prefer the explicit `d.keys()` call for loops too, but I'd be interested to see what others think.

---

_Label `rule` added by @ntBre on 2025-06-02 20:46_

---

_Label `needs-decision` added by @ntBre on 2025-06-02 20:46_

---

_Comment by @MrXerios on 2025-06-03 11:50_

Same here, I think ``d.keys()`` is much better since it's more explicit. Is it a performance issue ? ``for k in d :`` can't be much faster than ``for k in d.keys() :``, can it ?

---

_Comment by @zackw on 2025-06-03 13:10_

@MrXerios The bytecode compiler cannot eliminate the call to keys(), so `for x in d` will produce something like

```
...
              LOAD_FAST                0 (d)
              GET_ITER
      L1:     FOR_ITER                14 (to L2)
...
```

whereas `for x in d.keys()` will produce something like

```
...
              LOAD_FAST                0 (x)
              LOAD_ATTR                1 (keys + NULL|self)
              CALL                     0
              GET_ITER
      L1:     FOR_ITER                14 (to L2)
...
```

The loop body, however, is identical, so this difference should be in the noise for any substantial program.

The same difference is observable with `x in d[.keys()]` used as a boolean expression (just replace the GET_ITER/FOR_ITER pair with CONTAINS_OP) and in that case I _can_ imagine a program where the performance difference would be measurable, but it would be pretty contrived.

---

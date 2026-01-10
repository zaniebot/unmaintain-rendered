```yaml
number: 1546
title: Solid variable renaming
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-14T09:23:37Z
updated_at: 2025-12-31T16:38:42Z
url: https://github.com/astral-sh/ty/issues/1546
synced_at: 2026-01-10T01:56:40Z
```

# Solid variable renaming

---

_Issue opened by @MichaReiser on 2025-11-14 09:23_

We've gotten reports (without MRE's, unfortunately) that the rename feature often misses references. 

We should revisit the rename feature and stabilizie it once we have more confidence in it. 

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 09:23_

---

_Label `server` added by @MichaReiser on 2025-11-14 09:23_

---

_Comment by @sharkdp on 2025-12-01 13:05_

I ran into this yesterday with a much more complex example, and I only found out that ty's rename functionality broke my code several minutes later (and at that point it is difficult to undo). We should refuse to rename a variable if there is an existing variable of that name.

To reproduce, start with a snippet like the one below, and then rename `x` to `y`. The behavior of the script will change.
```py
def f():
    x = 1
    print("the value of x is:", x)
    y = "foo"
    print("x still has the value:", x)
```


---

_Comment by @Gankra on 2025-12-01 14:31_

Oh that's an interesting/good idea.

(Related but different: rust-analyzer will introduce shims to preserve the old name in some contexts. i.e. if you rename a struct field `x` to `y`, in places where you destructure it, it may change a simple `x` pattern to `y: x` so that the local you introduce in whatever function is still called `x`.)

---

_Comment by @MichaReiser on 2025-12-02 11:51_

Other things that come to mind that we should test:

* Renaming a definition from a third-party file should always fail (e.g. renaming a class defined in a third-party library or a method of a class from where it's used)
* Renaming a member should rename the member for all sub-classes
* Renaming named parameters should rename the call sites
* Renaming a named argument should probably not rename a named parameter?


---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:38_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:38_

---

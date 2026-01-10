```yaml
number: 21052
title: "[ty] Do not crash if a FileRoot is a symlink"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
base: main
head: gankra/rootcause
created_at: 2025-10-23T22:23:02Z
updated_at: 2025-11-12T20:47:47Z
url: https://github.com/astral-sh/ruff/pull/21052
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Do not crash if a FileRoot is a symlink

---

_Pull request opened by @Gankra on 2025-10-23 22:23_

This is a fix for the issue [described here](https://github.com/astral-sh/ruff/pull/21047#issuecomment-3437837018), where ty found my homebrew python site-packages, and then panicked because it was handling both symlink and non-symlink versions of the same directory (which, syntactically, are not subdirectories of eachother).

I am always suspicious of using canonicalize but it's not clear to me there's a way to avoid it here.

---

_Review requested from @carljm by @Gankra on 2025-10-23 22:23_

---

_Review requested from @MichaReiser by @Gankra on 2025-10-23 22:23_

---

_Review requested from @sharkdp by @Gankra on 2025-10-23 22:23_

---

_Review requested from @dcreager by @Gankra on 2025-10-23 22:23_

---

_Renamed from "Do not crash if a FileRoot is a symlink" to "[ty] Do not crash if a FileRoot is a symlink" by @Gankra on 2025-10-23 22:23_

---

_Label `ty` added by @Gankra on 2025-10-23 22:23_

---

_Label `server` added by @Gankra on 2025-10-23 22:23_

---

_@Gankra reviewed on 2025-10-23 22:24_

---

_Review comment by @Gankra on `crates/ruff_db/src/files.rs`:194 on 2025-10-23 22:24_

On paper the first `absolute` is redundant but I'm not sure if `db.system().current_directory()` is able to be not-the-process-level-current-directory that  canonicalize_path uses.

---

_Comment by @github-actions[bot] on 2025-10-23 22:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-10-23 22:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Frootcause?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21052 will **not alter performance**

<sub>Comparing <code>gankra/rootcause</code> (8e12163) with <code>main</code> (b93d8f2)</sub>



### Summary

`✅ 52` untouched  





---

_Review requested from @AlexWaygood by @Gankra on 2025-10-23 23:41_

---

_Comment by @github-actions[bot] on 2025-10-23 23:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-23 23:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @Gankra on `crates/ty_test/src/db.rs`:194 on 2025-10-23 23:45_

This is a "Hillarious" interaction between `strip_prefix` and `dunce`. 

`dunce` removes "unnecessary" UNC prefixes, but the import_basic mdtest actually passes in a really really really long path, which `dunce` then goes "ah UNC is needed". 

This results in `canonicalized.strip_prefix(os_system.current_directory())` failing.

If you then go "ah I will simply canonicalize `os_system.current_directory()`" **you still lose** because `dunce` is very helpful and removes the unnecessary UNC prefix from the much shorter cwd!

---

_@Gankra reviewed on 2025-10-23 23:45_

---

_Comment by @MichaReiser on 2025-10-24 06:03_

I'd very much prefer not to call `canonicalize` in `Files` and, instead, call `canonicalize` where we add the `FileRoot` (and where we look it up if that turns out to be necessary). It limits the scope of where we have an undesired `canonicalize` call and also allows us to better document why it is necessary in this specific instance. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-24 07:03_

---

_Comment by @Gankra on 2025-10-24 13:47_

> I'd very much prefer not to call canonicalize in Files and, instead, call canonicalize where we add the FileRoot (and where we look it up if that turns out to be necessary).

Do you mean you want this logic moved into the routine it's calling (FileRoot::try_add, FileRoot::at)? 
Or do you mean out to each caller of  `try_add_root` / `expect_root` / `root`?

---

_Comment by @MichaReiser on 2025-10-24 13:56_

Ideally, we'd canonicalize the path where we create it before adding it as a search path (or site package path or whatever it is). This should then also ensure that we only create paths that use the canonicalized representation. But I don't understand the specific problem enough to say whether that's possible

---

_Comment by @Gankra on 2025-10-30 15:29_

Bafflingly I can't reproduce this anymore so I'm just gonna close this

---

_Closed by @Gankra on 2025-10-30 15:29_

---

_Reopened by @Gankra on 2025-10-31 14:51_

---

_Comment by @Gankra on 2025-10-31 15:04_

Ok micha got a reproducer. If you go in vscode and via the pallete select "Python Interpretter: /opt/homebrew/bin/python3" the crash occurs:

```text
No root found for path '/opt/homebrew/Cellar/python@3.13/3.13.5/lib/python3.13/site-packages'. Known roots: FileRoots(
...
/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages
```

On my system the path we select is a fractal symlink:

1. `/opt/homebrew/bin/python3`
2. `/opt/homebrew/Cellar/python@3.13/3.13.5/bin/python3`
3. `/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/bin/python3`
4. `/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/bin/python3.13`

Path 4 is a real file. If you pass 1 or 2 to `ty check --python ...` everything works fine. If you pass 3 or 4 we have this panic.

The actual crash involves two other dirs in a symlink relationship:

5. `/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/lib/python3.13/site-packages`
6. `/opt/homebrew/Cellar/python@3.13/3.13.5/lib/python3.13/site-packages`

Path 6 is an actual directory. We also see that path 6 is, weirdly, what you would discover if you searched for site-packages from *path 2*, which is wild because path 2 was a bloody symlink!

So when handed path 3 or 4 as `--python` we find that we're in a *real* directory (so even if we canonicalized at that point it would do nothing). At this point we search for and discover site-packages at path 5, which *is* a symlink but we don't normalize that away.

Then at some later point we end up with a copy of the same path but with the symlink resolved (path 6), and we freak out because the two paths aren't lexically nested.

---

_Comment by @Gankra on 2025-10-31 15:23_

Ok so the "root cause" is this canonicalize (and the absence of a paired canonicalize for *adding* a root):

https://github.com/astral-sh/ruff/blob/b93d8f2b9fa834ab7c672d801c725c0031d9408e/crates/ty_python_semantic/src/module_resolver/resolver.rs#L451-L464

---

_Comment by @Gankra on 2025-10-31 16:41_

An unfortunate game of whackamole if you don't want to unconditionally canonicalize all paths.

---

_Comment by @Gankra on 2025-11-12 20:47_

Obsolete

---

_Closed by @Gankra on 2025-11-12 20:47_

---

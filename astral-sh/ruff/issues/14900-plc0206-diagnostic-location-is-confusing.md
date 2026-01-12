```yaml
number: 14900
title: PLC0206 diagnostic location is confusing
type: issue
state: closed
author: oz123
labels:
  - bug
  - diagnostics
assignees: []
created_at: 2024-12-10T22:15:08Z
updated_at: 2025-12-31T18:54:59Z
url: https://github.com/astral-sh/ruff/issues/14900
synced_at: 2026-01-12T15:54:54Z
```

# PLC0206 diagnostic location is confusing

---

_@oz123_

First it tells me no to use:
```
for key in dict:
      do_someting(key)
```
When I try `dict.keys` it still complains.
When I try `dict.item() and throw away value, it still complains:

```
oznt@dell-gentoo:/srv/oznt/Software/pypa/pipenv |pipenv-mRt4xz3R| [±|completely-remove-click-echo {2} U:3 ?:6 ✗|] $ruff check pipenv/routines/outdated.py 
pipenv/routines/outdated.py:50:23: PERF102 When using only the keys of a dict use the `keys()` method
   |
48 |     outdated = []
49 |     skipped = []
50 |     for package, _ in packages.items():
   |                       ^^^^^^^^^^^^^^ PERF102
51 |         norm_name = pep423_name(package)
52 |         if norm_name in updated_packages:
   |
   = help: Replace `.items()` with `.keys()`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
oznt@dell-gentoo:/srv/oznt/Software/pypa/pipenv |pipenv-mRt4xz3R| [±|completely-remove-click-echo {2} U:3 ?:6 ✗|] $vim pipenv/routines/outdated.py 
oznt@dell-gentoo:/srv/oznt/Software/pypa/pipenv |pipenv-mRt4xz3R| [±|completely-remove-click-echo {2} U:3 ?:6 ✗|] $vim pipenv/routines/outdated.py 
oznt@dell-gentoo:/srv/oznt/Software/pypa/pipenv |pipenv-mRt4xz3R| [±|completely-remove-click-echo {2} U:2 ?:6 ✗|] $ruff check pipenv/routines/outdated.py 
pipenv/routines/outdated.py:50:5: PLC0206 Extracting value from dictionary without calling `.items()`
   |
48 |       outdated = []
49 |       skipped = []
50 |       for package in packages.keys():
   |  _____^
51 | |         norm_name = pep423_name(package)
52 | |         if norm_name in updated_packages:
53 | |             version = packages[package]
54 | |             if isinstance(version, Mapping):
55 | |                 version = parse_version(version.get("version", "").replace("==", ""))
56 | |             else:
57 | |                 version = parse_version(version.replace("==", ""))
58 | |             if updated_packages[norm_name] != version:
59 | |                 outdated.append(
60 | |                     package_info(package, str(version), str(updated_packages[norm_name]))
61 | |                 )
62 | |             elif canonicalize_name(package) in outdated_packages:
63 | |                 skipped.append(outdated_packages[canonicalize_name(package)])
   | |_____________________________________________________________________________^ PLC0206
64 |       for package, old_version, new_version in skipped:
65 |           for category in project.get_package_categories():
   |

Found 1 error.

```

I believe PLC0206 should not be raised if I use `dict.keys()`

---

_Comment by @dylwil3 on 2024-12-10 23:32_

Thanks for submitting this!

So I think that the error message is pointing to the wrong spot (which is confusing...), PLC0206 is complaning about this:

```python
    if norm_name in updated_packages:
        version = packages[package]
```

So I think it would be compatible with both rules to do something more like:

```diff
diff --git a/tmp.py b/tmp2.py
index dfa6d6f..6975214 100644
--- a/tmp.py
+++ b/tmp2.py
@@ -1,9 +1,8 @@
 outdated = []
 skipped = []
-for package in packages.keys():
+for package, version in packages.items():
     norm_name = pep423_name(package)
     if norm_name in updated_packages:
-        version = packages[package]
         if isinstance(version, Mapping):
             version = parse_version(version.get("version", "").replace("==", ""))
         else:
```

But I can't quite tell from your code if you're trying to mutate the dictionary key at that point, in which case you'd have to do something else. (The above patch would also violate [redefined-loop-name (PLW2901)](https://docs.astral.sh/ruff/rules/redefined-loop-name/#redefined-loop-name-plw2901), so if that rule is also enabled you would have to name the loop variable something else).

---

_Comment by @dylwil3 on 2024-12-10 23:32_

I do think the error message being in the wrong spot is a bug though - we should fix that

---

_Label `bug` added by @dylwil3 on 2024-12-10 23:33_

---

_Label `diagnostics` added by @dylwil3 on 2024-12-10 23:33_

---

_Renamed from "PLC0206 vs PERF102 limbo ..." to "PLC0206 diagnostic location is confusing" by @dylwil3 on 2024-12-11 19:38_

---

_Comment by @dylwil3 on 2024-12-13 16:48_

I started looking into this and there's a bit of a design question: how should we handle multiple uses of the dictionary value inside the for-loop?

Some options:

1. Offer a separate diagnostic per violation, pointing to the location of the violation
2. Same as (1) but add a "parent" for each of these diagnostics at the start of the for-loop, so users can `# noqa` the whole block (I think that's how that would work?)
3. Overhaul the diagnostic infrastructure to allow displaying multiple ranges for a single diagnostic (I imagine this would be a last resort)
4. Keep it as-is
5. Something else

@AlexWaygood / @MichaReiser have y'all run into this issue before where you want to display multiple text ranges for a single diagnostic? Is there prior art?

---

_Comment by @MichaReiser on 2024-12-15 15:46_

Changing the location is, unfortunately, a breaking change because it breaks all `noqa` comments. We should avoid changing the location more than once.

To me it's not clear what the right location should be but I'm leaning towards:

* Flagging the dictionary expression in `in <dict>` because the rule name and its description are both about dictionary iteration (and not *use of dict index in dictionary iteration)
* Flagging the `in <dict>` allows suppressing all usages with a single noqa comment

However, I can see how this isn't useful. That's why I'm leaning towards renaming and redefining the rule to flag the usages instead. Requiring multiple `noqa` comments would then be okay in my view because the rule reports the usages and not the for loop itself. 


---

_Comment by @beauxq on 2025-11-22 18:11_

Note that this diagnostic location has the same problem as this issue: https://github.com/astral-sh/ruff/issues/15570

Marking an entire 100-line `for` loop with yellow squigglies is really noisy. It makes it hard to see other things that I'm looking for in the code.

Please steer away from marking anything that could be a large block of code.

---

_Comment by @ntBre on 2025-12-30 22:10_

I like the idea of flagging the `in <dict>` part as the primary diagnostic range. Now that we have sub-diagnostics, we could attach a sub-diagnostic/secondary annotation for each of the `dict[key]` uses.

---

_Closed by @ntBre on 2025-12-31 18:54_

---

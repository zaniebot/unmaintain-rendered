```yaml
number: 6514
title: "[Proposal] Mirror isort's float-to-top feature to autofix simple cases of E402"
type: issue
state: open
author: SummerStorm
labels:
  - fixes
assignees: []
created_at: 2023-08-12T00:14:00Z
updated_at: 2025-12-26T19:07:29Z
url: https://github.com/astral-sh/ruff/issues/6514
synced_at: 2026-01-10T11:09:48Z
```

# [Proposal] Mirror isort's float-to-top feature to autofix simple cases of E402

---

_Issue opened by @SummerStorm on 2023-08-12 00:14_

### Minimal reproducible example
```
a = 1
import os 
```
### Ruff output and Ruff version
```
$ ruff -V && ruff -v --ignore F401 --diff E402_violation.py
ruff 0.0.283
[2023-08-11][15:48:25][ruff_cli::resolve][DEBUG] Using Ruff default settings
[2023-08-11][15:48:25][ruff_cli::commands::run][DEBUG] Identified files to lint in: 1.5035ms
[2023-08-11][15:48:25][ruff_cli::diagnostics][DEBUG] Checking: /tmp/E402_violation.py
[2023-08-11][15:48:25][ruff_cli::commands::run][DEBUG] Checked 1 files in: 930.5µs
```

###  `isort` output (and expected Ruff output)
```
$ isort --float-to-top -diff E402_violation.py
import os

a = 1
```
`isort` has a useful [`float_to_top`](https://pycqa.github.io/isort/docs/configuration/options.html#float-to-top) config where all non-indented imports are "floated" to the top of the file. I find this quite useful as I can import anywhere within the file, and the import will automatically "float" to the correct location. I propose that we add a `float-to-top` flag to mirror this behavior:
```
[tool.ruff.isort] 
float-to-top = true 
```
By enabling this flag, the user is signaling to Ruff that it should autofix all such trivial cases of E402. While E402 is impossible to fix in general, I believe that non-indented cases of E402 are autofixable in the vast majority of projects. 
Here are two cases where re-positioning non-indented imports might break things:
```
import sys
sys.path.append(some_module_path)
import some_module
```
```
import os
os.environ['LIB_CAN_THROW_ERROR_ON_IMPORT'] = 2
import lib
os.environ['LIB_CAN_THROW_ERROR_ON_IMPORT'] = 0 
```


---

_Label `autofix` added by @charliermarsh on 2023-08-14 21:32_

---

_Comment by @charliermarsh on 2023-08-14 21:33_

We definitely _can_ fix this but for the reasons you describe it should probably be an opt-in fix once that feature ships.

---

_Comment by @SummerStorm on 2023-08-14 21:35_

> We definitely _can_ fix this but for the reasons you describe it should probably be an opt-in fix once that feature ships.

Yes, I agree. The user must opt-in via some new flag such as:
```
[tool.ruff.isort] 
float-to-top = true 
```


---

_Comment by @adamstirk-ct on 2024-04-30 18:03_

Is there any news on getting float-to-top added?

---

_Comment by @searayeah on 2025-03-07 21:27_

I frequently use `float-to-top` with nbQA on Jupyter notebooks. It automatically moves all imports from every cell to the first one, making the notebooks much cleaner. This feature would be very useful if added to ruff.

---

_Comment by @marciomazza on 2025-10-08 12:37_

This feature would be awesome.
I keep using isort just for that.

---

_Comment by @PeterJCLaw on 2025-11-03 20:54_

I've been looking at what's needed for this, mostly so far just familiarising myself with the `isort` equivalent code in `ruff`. I'd be up for trying to put together a PR if we can agree a spec.

Some questions:
1. how closely do we want to match `isort`'s `float-to-top` behaviour? Specifically: do we want to try to match things like `# isort:skip`, or are we happy with a first pass which doesn't? (I don't know how hard/easy that would be to support, just looking to set scope boundaries)
2. is there a good forum to get feedback on a proposed algorithm? I have some thoughts, though I'm fairly new to Rust so I a) don't have a good feel for what'd make good idiomatic Rust and b) would like to get some feedback on whether we want a "big" refactor here vs perhaps trying to do a smaller change (trade-off might be that future work is a little harder).

As food for thought on the latter, I see broadly these options :
- rework just [`organize_imports`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/isort/rules/organize_imports.rs#L85) and its caller so the loop currently in `check_imports` is pushed downwards, enabling all the work of pulling the imports to the top of the file (as well as all the general detection logic, fixing, etc.); this feels to me like it could make that function quite busy, which might be fine if more of it can be factored to utils (haven't looked hard at that yet)
- a bit more refactoring to a `Block` which separates out the list-of-statements-in-the-block (which ends up determining what `TextRange` is highlighted/replaced) from the list-of-imports-to-format, possibly making it a bit more mutable along the way (I haven't yet fully worked out the lifetime/scope of the `Block` instances, so not sure if this would have more side-effects)
- passing both the current `Block` and the list of all of them to `organize_imports`, with a flag for whether or not we're operating on the first one; I don't like this approach though as the coupling between the behaviours in the caller and the callee feel a bit "just so" and thus potentially brittle

---

_Comment by @PeterJCLaw on 2025-11-09 12:37_

> 1. how closely do we want to match `isort`'s `float-to-top` behaviour? Specifically: do we want to try to match things like `# isort:skip`, or are we happy with a first pass which doesn't? (I don't know how hard/easy that would be to support, just looking to set scope boundaries)

Realised that this is already supported (at least [according to the docs](https://docs.astral.sh/ruff/linter/#action-comments)), which I should have found sooner. So I think it'd just be a case of understanding how that works and is still respected.

I did however think of another question around how we want these fixes to be applied:

3. should this fix be considered ["safe"](https://docs.astral.sh/ruff/linter/#fix-safety) or not? My reading of the docs is that since this could more easily change the semantics around imports (for exactly the reasons described in the original post), it ought to be an unsafe fix. However if we're relying on the `float-to-top` config being present, that's arguably the user explicitly asking for it -- could we consider that as equivalent to configuring this fix as being "safe" just as the config for other unsafe fixes can be overridden?
If we do want to consider this aspect of the fixer for `I001` unsafe (even when explicitly enabled), that _may_ affect how we build it so I'd be interested to get opinions on it sooner.


---

_Comment by @ntBre on 2025-11-10 19:13_

@PeterJCLaw Thanks for looking into this! I wonder if an easier solution might be to make this an unsafe fix for `E402`? That seems like it could be enough opt-in and not require any new configuration.

---

_Comment by @PeterJCLaw on 2025-11-10 20:13_

> [@PeterJCLaw](https://github.com/PeterJCLaw) Thanks for looking into this! I wonder if an easier solution might be to make this an unsafe fix for `E402`? That seems like it could be enough opt-in and not require any new configuration.

Hrm, possibly. Two questions though:
- how would that interact with running the `isort` logic if the user has enabled it (i.e: how do we ensure that the move-to-top stuff happens before the other formatting, avoiding the need to double-run)
- would it still be possible to enable the fix if the user wants to enable `isort`'s `float-to-top` option, or would we just not support that option?


---

_Comment by @ntBre on 2025-11-10 20:22_

> * how would that interact with running the `isort` logic if the user has enabled it (i.e: how do we ensure that the move-to-top stuff happens before the other formatting, avoiding the need to double-run)

Ruff applies fixes in a loop until they stabilize, so enabling I001 and E402 should handle this automatically, I think.

> * would it still be possible to enable the fix if the user wants to enable `isort`'s `float-to-top` option, or would we just not support that option?

I was thinking we wouldn't add a new configuration option, you would just select E402 and accept its unsafe fix, if you want this behavior.

---

_Comment by @PeterJCLaw on 2025-11-10 21:05_

> Ruff applies fixes in a loop until they stabilize, so enabling I001 and E402 should handle this automatically, I think.

Ah interesting.

> I was thinking we wouldn't add a new configuration option, you would just select E402 and accept its unsafe fix, if you want this behavior.

I guess that could work. There is the small issue of whether people have used `# noqa: E402` vs `# isort: skip` around the imports they don't want to be moved, but that wouldn't be too hard for them to change.

The other consideration here is the organise-imports logic in editors -- I personally (and suspect others too) would want the float-to-top behaviour to happen when running that editor command. It looks like `organize_imports_edit` has a list of rules which it aims to fix though, so hooking that up shouldn't be too hard ... as long as that'd still obey people's choice of whether or not E402 should be being fixed or not (in general).

Thanks, I'll have a look at this as an option and see if I can work out what the various UX options/trade-offs are.

---

_Comment by @PeterJCLaw on 2025-12-26 19:07_

I've had a go at this and got something which seems to work: https://github.com/astral-sh/ruff/pull/22212. Still work-in-progress, but would be good to get a directional sense check before I get too far down the road of adding tests etc.

> I was thinking we wouldn't add a new configuration option, you would just select E402 and accept its unsafe fix, if you want this behavior.

This works nicely, thanks for suggesting.

> The other consideration here is the organise-imports logic in editors -- I personally (and suspect others too) would want the float-to-top behaviour to happen when running that editor command. It looks like `organize_imports_edit` has a list of rules which it aims to fix though, so hooking that up shouldn't be too hard ... as long as that'd still obey people's choice of whether or not E402 should be being fixed or not (in general).

I've got this connected up and it seems to work like we'd hope. If a project has selected E402's fix as "safe" then it gets applied, otherwise it doesn't.


---

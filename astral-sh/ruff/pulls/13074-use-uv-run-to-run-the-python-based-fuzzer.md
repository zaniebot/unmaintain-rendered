```yaml
number: 13074
title: "Use `uv run` to run the Python-based fuzzer"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - ci
assignees: []
base: main
head: scripts-uv
created_at: 2024-08-23T10:42:07Z
updated_at: 2024-11-26T12:06:32Z
url: https://github.com/astral-sh/ruff/pull/13074
synced_at: 2026-01-12T15:55:43Z
```

# Use `uv run` to run the Python-based fuzzer

---

_@AlexWaygood_

## Summary

This converts the `scripts/fuzz-parser` script into a script runnable via `uv run`.

Since this is a single-file script, I considered using [inline metadata](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) here; it seemed like it might be a good fit. However, it seems as though inline dependencies aren't recorded in `uv.lock` at all, which wouldn't be ideal since we use this script in CI as a regression test. I therefore added the dependencies of the script to the `pyproject.toml` in the Ruff repo root instead, which means that the dependencies are pinned in the project's `uv.lock` file.

Annoyingly, `uv run scripts/fuzz-parser.py` seems to build Ruff from source with this PR before it runs the script. I don't think there's any way currently to tell uv to _only_ install the dev dependencies, and not bother with installing the project itself, when running scripts? Building Ruff from source isn't necessary with this script if you pass the `--test-executable` flag. This might be a blocker for this PR? It means that the `uv run` invocation takes ages to get going.

I deliberately haven't tried to include any of the docs dependencies in this change, as @MichaReiser already tried in #12622, and the situation there is quite complicated with the two `requirements.txt` files.

Overall this is something I'd love to do as it seems like it _could_ be a really nice simplification... but I'm not sure it's a perfect fit right now?

## Test Plan

- `uv run scripts/fuzz-parser.py 0-100`
- `uv run scripts/generate_known_standard_library.py`
- `cd scripts/knot_benchmark && uv run benchmark`

---

_Label `internal` added by @AlexWaygood on 2024-08-23 10:42_

---

_Label `ci` added by @AlexWaygood on 2024-08-23 10:42_

---

_Review requested from @zanieb by @AlexWaygood on 2024-08-23 10:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-23 10:42_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-23 10:42_

---

_Comment by @codspeed-hq[bot] on 2024-08-23 10:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/scripts-uv)

### Merging #13074 will **not alter performance**

<sub>Comparing <code>scripts-uv</code> (d11d884) with <code>main</code> (c6023c0)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-23 11:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-08-23 11:13_

Haha, and here is another use case asking for virtual workspace. This will make @charliermarsh so happy

---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-23 11:14_

---

_Review request for @MichaReiser removed by @MichaReiser on 2024-08-23 11:14_

---

_Review request for @dhruvmanila removed by @dhruvmanila on 2024-08-23 11:40_

---

_Comment by @AlexWaygood on 2024-08-28 18:27_

Well... this works for the `fuzz-parser` script, but not for `generate_known_standard_library.py`. `uv` refuses to install a new enough version of `stdlibs`, because recent versions of `stdlibs` require Python 3.8+, but the `requires-python` field in our `pyproject.toml` states (correctly) that Ruff supports being installed on Python 3.7+, and it seems impossible to convince uv that it's okay to install a dev-dependency that's only available on newer Python versions if there's the `requires-python = ">= 3.7"` field there.

I think having a dev-dependency that's only available on newer Python versions than the one the package is declared as supporting is probably reasonable...? Hmm, but maybe if it's declared as a "virtual" workspace, uv assumes there _is_ no package, so therefore the `requires-python` field _must_ refer to whatever ad-hoc scripts you have floating around in your "virtual" workspace...

---

_Comment by @charliermarsh on 2024-08-28 18:30_

I kind of think these should just be PEP 723 scripts. Semantically, at least, that's what they _want_ to be, right?

---

_Comment by @AlexWaygood on 2024-08-28 18:31_

> I kind of think these should just be PEP 723 scripts. Semantically, at least, that's what they _want_ to be, right?

I tried that at first, but I don't get the transitive dependencies pinned in the lockfile (and updated by Renovate) if I have them as PEP 723 scripts, right? We run `fuzz-parser` in CI; I think we want the transitive deps pinned for that

---

_Comment by @charliermarsh on 2024-08-28 18:37_

Separately, why are the deps not in `scripts/pyproject.toml`?

---

_Comment by @AlexWaygood on 2024-08-28 18:40_

> Separately, why are the deps not in `scripts/pyproject.toml`?

Would I have to `cd` into `scripts` to run them then? `fuzz-parser` needs to be run from the workspace root. Or will `uv run` pick up the config file at `scripts/pyproject.toml` even if I run the scripts from the workspace root?

---

_Comment by @zanieb on 2024-08-28 18:46_

Related https://github.com/astral-sh/uv/issues/6733

---

_Comment by @charliermarsh on 2024-08-29 15:30_

Alternatively we could prioritize adding the lockfile to `PEP 723` scripts heh.

---

_Comment by @AlexWaygood on 2024-08-29 15:31_

> Alternatively we could prioritize adding the lockfile to `PEP 723` scripts heh.

Ah, that would be the ideal solution if it's something you're considering!

---

_Comment by @zanieb on 2024-08-29 16:30_

@AlexWaygood would you want it to be embedded in the script? It might be looong.

---

_Comment by @AlexWaygood on 2024-08-30 14:16_

> @AlexWaygood would you want it to be embedded in the script? It might be looong.

Hrm... no, probably not!

I think there's essentially two genres of single-file scripts:
1. One-off scripts that you write for a single purpose. You want these to be easily shareable, you don't want to have to faff around with a separate `requirements.txt` or lockfile for them, you want everything about the script to be contained within the single file.
2. "Persistent" scripts that are a single file but that you keep around indefinitely and run on a regular basis. These might be scripts that are e.g. run as part of CI for a larger project. You want these scripts to be reliable, and shareability isn't as important.

These scripts definitely feel like they fit into category (2) rather than category (1). It's not like it's ever going to make sense to try to run them outside this repository, so shareability and portability aren't really important; it's okay if the lockfile gets output to a separate file, IMO. I guess the easiest way of doing that implementation-wise would be a separate lockfile for each script, since you'll probably want a separate virtual environment for each script?

---

_Comment by @charliermarsh on 2024-08-30 14:23_

> "Persistent" scripts that are a single file but that you keep around indefinitely and run on a regular basis. These might be scripts that are e.g. run as part of CI for a larger project. You want these scripts to be reliable, and shareability isn't as important.

I think this should just be a project, personally.

---

_Closed by @AlexWaygood on 2024-11-14 17:01_

---

_Branch deleted on 2024-11-26 12:06_

---

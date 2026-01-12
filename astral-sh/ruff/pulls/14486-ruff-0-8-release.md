```yaml
number: 14486
title: Ruff 0.8 release
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: release-0.8
created_at: 2024-11-20T12:19:39Z
updated_at: 2024-11-22T07:45:21Z
url: https://github.com/astral-sh/ruff/pull/14486
synced_at: 2026-01-12T15:55:48Z
```

# Ruff 0.8 release

---

_@MichaReiser_

## Summary

Version bump and changelog for the Ruff 0.8 release. 

Below the list of contributors for the GitHub release entry

* [ ] Update breaking changes


### Contributors
- [@AlexWaygood](https://github.com/AlexWaygood)
- [@CarrotManMatt](https://github.com/CarrotManMatt)
- [@Daverball](https://github.com/Daverball)
- [@Glyphack](https://github.com/Glyphack)
- [@InSyncWithFoo](https://github.com/InSyncWithFoo)
- [@MichaReiser](https://github.com/MichaReiser)
- [@cake-monotone](https://github.com/cake-monotone)
- [@charliermarsh](https://github.com/charliermarsh)
- [@dhruvmanila](https://github.com/dhruvmanila)
- [@diceroll123](https://github.com/diceroll123)
- [@dylwil3](https://github.com/dylwil3)
- [@hauntsaninja](https://github.com/hauntsaninja)
- [@konstin](https://github.com/konstin)
- [@sbrugman](https://github.com/sbrugman)
- [@sharkdp](https://github.com/sharkdp)
- [@takaya0](https://github.com/takaya0)
- [@tjkuson](https://github.com/tjkuson)
- [@zanieb](https://github.com/zanieb)





---

_Label `release` added by @MichaReiser on 2024-11-20 12:20_

---

_@MichaReiser reviewed on 2024-11-20 12:20_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:11 on 2024-11-20 12:20_

Huh, why did it use my fork as remote

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2024-11-20 12:22_

probably https://github.com/zanieb/rooster/issues/42?

---

_@AlexWaygood reviewed on 2024-11-20 12:22_

---

_Comment by @github-actions[bot] on 2024-11-20 12:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2024-11-20 13:53_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2024-11-20 13:57_

```suggestion
- **Use XDG (i.e. `~/.local/bin`) instead of the Cargo home directory in the standalone installer**
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:17 on 2024-11-20 13:59_

```suggestion
    Previously, Ruff's installer used `$CARGO_HOME` or `~/.cargo/bin` for its target install directory. Now, Ruff will be installed into `$XDG_BIN_HOME`, `$XDG_DATA_HOME/../bin`, or `~/.local/bin` (in that order).
    
    This change is only relevant to users of the standalone Ruff installer (using the shell or PowerShell script). If you installed Ruff using `uv` or `pip`, you should be unaffected.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2024-11-20 14:00_

```suggestion
    Ruff now defaults to Python 3.9 instead of 3.8 if no explicit Python version is configured using [`ruff.target-version`](https://docs.astral.sh/ruff/settings/#target-version) or [`project.requires-python`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#python-requires) ([#13896](https://github.com/astral-sh/ruff/pull/13896))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2024-11-20 14:00_

```suggestion
    Ruff now uses a more recent version of the Unicode standard. In very rare cases, this can lead to formatting changes or new [`E501`](https://docs.astral.sh/ruff/rules/line-too-long/) violations if the computed line width differs.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2024-11-20 14:01_

```suggestion
The [`flake8-type-checking`](https://docs.astral.sh/ruff/rules/#flake8-type-checking-tch) rules have been remapped to new rule codes: `TCH` to `TC`.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:60 on 2024-11-20 14:02_

```suggestion
- [`ambiguous-variable-name`](https://docs.astral.sh/ruff/rules/ambiguous-variable-name/) (`E741`): Violations in stub files are now ignored. Stub authors typically don't control variable names.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:61 on 2024-11-20 14:03_

```suggestion
- [`invalid-pyproject-toml`](https://docs.astral.sh/ruff/rules/invalid-pyproject-toml/) (`RUF200`): Updated to reflect the provisionally accepted [PEP 639](https://peps.python.org/pep-0639/).
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:62 on 2024-11-20 14:03_

```suggestion
- [`printf-string-formatting`](https://docs.astral.sh/ruff/rules/printf-string-formatting/) (`UP031`): Report all `printf`-like usages even if no autofix is available
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:78 on 2024-11-20 14:04_

Could we possibly split this into two sections: new preview rules, and new features added to existing rules already in preview?

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:73 on 2024-11-20 14:05_

I keep debating in my head whether this deserves to be called out more prominently. On the one hand, it's quite a breaking change for people already opting into these rules. On the other hand, the rules are still in preview, so breaking changes are to be expected to some extent.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:85 on 2024-11-20 14:08_

```suggestion
- \[`flake8-datetimez`\] Exempt `min.time()` and `max.time()` for `datetime-min-max` (`DTZ901`) ([#14394](https://github.com/astral-sh/ruff/pull/14394))
- \[`flake8-pie`\] Mark `unnecessary-placeholder` fix as unsafe if the following statement is a string literal (`PIE790`) ([#14393](https://github.com/astral-sh/ruff/pull/14393))
- \[`flake8-pyi`\] Avoid panic when `redundant-numeric-union` is unfixable (`PYI041`) ([#14402](https://github.com/astral-sh/ruff/pull/14402))
- \[`flake8-type-checking`\] Correctly handle quotes in subscript expressions when generating autofixes ([#14371](https://github.com/astral-sh/ruff/pull/14371))
- \[`pylint`\] Improve autofix for `__contains__` in `unnecessary-dunder-call` (`PLC2801`) ([#14424](https://github.com/astral-sh/ruff/pull/14424))
```

---

_@AlexWaygood reviewed on 2024-11-20 14:08_

---

_Comment by @AlexWaygood on 2024-11-20 14:09_

Thank you!

---

_@MichaReiser reviewed on 2024-11-20 14:37_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:38 on 2024-11-20 14:37_

I agree that the list feels a bit overkill but it's consistent with previous changelogs. 

---

_Review comment by @MichaReiser on `CHANGELOG.md`:85 on 2024-11-20 14:38_

I don't think we ever included the rule names in other changelog entries (other than in the new or deprecated rule sections)

---

_@MichaReiser reviewed on 2024-11-20 14:38_

---

_@AlexWaygood reviewed on 2024-11-20 14:39_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2024-11-20 14:39_

Is stylistic consistency between changelogs that important here? I doubt anybody will go comparing the different changelogs.

Here specifically, there's no point in having a list with only one item in it.

---

_Review comment by @MichaReiser on `CHANGELOG.md`:78 on 2024-11-20 14:40_

We could, although it's not something we've done in the past. It's overall somewhat handwavy. Are preview bugfixes in the bugfix or preview section? 



---

_@MichaReiser reviewed on 2024-11-20 14:40_

---

_@MichaReiser reviewed on 2024-11-20 14:40_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:73 on 2024-11-20 14:40_

Yeah, I might call it out at the top. 

---

_@AlexWaygood reviewed on 2024-11-20 14:40_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:85 on 2024-11-20 14:40_

I think it will be more informative and helpful to users to include the rule names here. I don't see whether we've done it in the past as being that relevant

---

_@MichaReiser reviewed on 2024-11-20 14:41_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:38 on 2024-11-20 14:41_

I like the consistency because I commonly use previous changelogs as template for the next minor release. I also don't think it matters that much. It's a changelog. I wouldn't recommend this for the blog post

---

_@AlexWaygood reviewed on 2024-11-20 14:41_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:78 on 2024-11-20 14:41_

I'd just have two sections

```
### New preview rules

### Other preview changes
```

---

_@AlexWaygood reviewed on 2024-11-20 14:43_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:38 on 2024-11-20 14:43_

Interesting. I guess I weigh the usefulness as a template for the next version less strongly, as I usually find that I have to significantly edit the autogenerated changelog. But fair enough :-)

---

_@MichaReiser reviewed on 2024-11-20 14:45_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:78 on 2024-11-20 14:45_

That makes sense, but it only makes sense to me if we do this in all future releases. 

---

_@AlexWaygood reviewed on 2024-11-20 14:46_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:78 on 2024-11-20 14:46_

I'm okay with that, personally

---

_@MichaReiser reviewed on 2024-11-20 14:48_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:78 on 2024-11-20 14:48_

I know haha, I don't know of a good way to automate it. We're labeling our issues too inconsistently. 

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:78 on 2024-11-20 14:50_

I'm fine to leave it as you have it now :-) this probably comes down to me again feeling like this stage of the release process is already quite manual for me.

---

_@AlexWaygood reviewed on 2024-11-20 14:50_

---

_@AlexWaygood approved on 2024-11-20 14:50_

Thanks again!

---

_@calumy reviewed on 2024-11-20 14:51_

---

_Review comment by @calumy on `CHANGELOG.md`:31 on 2024-11-20 14:51_

This line is duplicated on line 33 but with the correct code (`PT005`)

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2024-11-20 14:51_

```suggestion
- [`syntax-error`](https://docs.astral.sh/ruff/rules/syntax-error/) (`E999`)
```

---

_@AlexWaygood reviewed on 2024-11-20 14:52_

---

_@AlexWaygood reviewed on 2024-11-20 14:52_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2024-11-20 14:52_

You had it right the first time in your now-deleted comment! `E999` and `PT005` are both removed in this release, but `PT005` is already listed lower down. This bullet should indeed cover `E999`. https://github.com/astral-sh/ruff/pull/14486#discussion_r1850459257

---

_@MichaReiser reviewed on 2024-11-20 14:58_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:85 on 2024-11-20 14:58_

It makes the change log entries unnecessarily long, in my view. We already include the rule code, which is the main identifier used by users. I agree that it's currently somewhat inconsistent with some sections using rule names and others not. Either way, I prefer keeping it as is for now.

---

_@AlexWaygood reviewed on 2024-11-20 15:00_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:85 on 2024-11-20 15:00_

OK!

---

_@MichaReiser reviewed on 2024-11-20 15:05_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:73 on 2024-11-20 15:05_

Actually, it's not really a significant change. Most users should be unaffected because it is only relevant for cases where the pydoclint rule is suppressed with a `noqa` comment, because the suppression now is invalid.

---

_@MichaReiser reviewed on 2024-11-20 15:07_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:73 on 2024-11-20 15:07_

Looking at the ecosystem changes, there's just one RUF100 violation because of the changed location. Seems fine

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:73 on 2024-11-20 15:15_

> Looking at the ecosystem changes, there's just one RUF100 violation because of the changed location. Seems fine

We don't know for sure that the ecosystem corpus is representative of our users as a whole there -- there might be lots of users `noqa`ing pydoclint rules who aren't included in the ecosystem corpus. I would probably still call it out at the top, out of an abundance of caution. But I agree it's not too bad.

---

_@AlexWaygood reviewed on 2024-11-20 15:15_

---

_@calumy reviewed on 2024-11-20 15:28_

---

_Review comment by @calumy on `CHANGELOG.md`:31 on 2024-11-20 15:28_

Oops, I got a bit confused! It's good to see that it is sorted now.

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-20 15:36_

---

_@AlexWaygood reviewed on 2024-11-20 17:30_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:63 on 2024-11-20 17:30_

Reading this through again, I think this might be in the wrong section. This behaviour wasn't ever in `preview` (IIUC), so I don't think it counts as a "stabilisation"?

---

_Review comment by @Daverball on `CHANGELOG.md`:40 on 2024-11-20 21:55_

```suggestion
- [`flake8-type-checking`](https://docs.astral.sh/ruff/rules/#flake8-type-checking-tc): `TCH` to `TC`
```
I assume the anchor in the docs is going to change once this is released.

---

_@Daverball reviewed on 2024-11-20 21:55_

---

_@MichaReiser reviewed on 2024-11-21 07:27_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:40 on 2024-11-21 07:27_

Good catch. We keep perma links for renamed rules but that probably doesn't apply to renamed categories.

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:13 on 2024-11-21 11:34_

```suggestion
    Ruff now assumes Python 3.9 by default instead of 3.8 if no explicit Python version is configured using [`ruff.target-version`](https://docs.astral.sh/ruff/settings/#target-version) or [`project.requires-python`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#python-requires) ([#13896](https://github.com/astral-sh/ruff/pull/13896))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2024-11-21 11:34_

```suggestion
    [`pydoclint`](https://docs.astral.sh/ruff/rules/#pydoclint-doc) diagnostics now point to the first-line of the problematic docstring. Previously, this was not the case.

    If you've opted into these preview rules but have them suppressed using
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:32 on 2024-11-21 11:35_

```suggestion
    Ruff now uses a new version of the [unicode-width](https://github.com/unicode-rs/unicode-width) Rust crate to calculate the line width. In very rare cases, this may lead to lines containing Unicode characters being reformatted, or being considered too long when they were not before ([`E501`](https://docs.astral.sh/ruff/rules/line-too-long/)).
```

---

_@AlexWaygood reviewed on 2024-11-21 11:36_

---

_Comment by @AlexWaygood on 2024-11-21 12:00_

I wonder if https://github.com/astral-sh/ruff/pull/14435 might be worth a mention in here somewhere? It's not breaking (it's the opposite of breaking), and I don't think it's important enough to be called out in the blog post, but it's a pretty useful feature that users might like to know about.

---

_Closed by @MichaReiser on 2024-11-21 12:51_

---

_Reopened by @MichaReiser on 2024-11-21 12:51_

---

_@MichaReiser reviewed on 2024-11-21 12:51_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:13 on 2024-11-21 12:51_

I prefer the simpler language of defaults

---

_@AlexWaygood reviewed on 2024-11-21 15:06_

---

_Review comment by @AlexWaygood on `metadata.json`:1 on 2024-11-21 15:06_

not sure you meant to add this

---

_@MichaReiser reviewed on 2024-11-21 15:15_

---

_Review comment by @MichaReiser on `metadata.json`:1 on 2024-11-21 15:15_

sure I did :rofl: 

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:22 on 2024-11-21 15:21_

Zanie correctly flagged in my blogpost that we typically don't style these with backticks

```suggestion
    This change is only relevant to users of the standalone Ruff installer (using the shell or PowerShell script). If you installed Ruff using uv or pip, you should be unaffected.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:28 on 2024-11-21 15:23_

```suggestion
    This change is only relevant to users of the standalone Ruff installer (using the shell or PowerShell script). If you installed Ruff using uv or pip, you should be unaffected.
```

---

_@AlexWaygood approved on 2024-11-21 15:23_

---

_@MichaReiser reviewed on 2024-11-21 15:37_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:28 on 2024-11-21 15:37_

I copied this from the changelog :laughing: 

---

_@AlexWaygood reviewed on 2024-11-21 15:38_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:28 on 2024-11-21 15:38_

this _is_ the changelog ;-) see my other comment at https://github.com/astral-sh/ruff/pull/14486#discussion_r1852343494

---

_@AlexWaygood approved on 2024-11-21 16:03_

---

_Merged by @MichaReiser on 2024-11-22 07:45_

---

_Closed by @MichaReiser on 2024-11-22 07:45_

---

_Branch deleted on 2024-11-22 07:45_

---

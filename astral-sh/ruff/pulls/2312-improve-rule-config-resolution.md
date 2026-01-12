```yaml
number: 2312
title: Improve rule config resolution
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: improve-rule-resolution
created_at: 2023-01-28T20:29:50Z
updated_at: 2023-01-30T21:27:00Z
url: https://github.com/astral-sh/ruff/pull/2312
synced_at: 2026-01-12T04:52:00Z
```

# Improve rule config resolution

---

_Pull request opened by @not-my-profile on 2023-01-28 20:29_

Ruff allows rules to be enabled with `select` and disabled with
`ignore`, where the more specific rule selector takes precedence,
for example:

    `--select ALL --ignore E501` selects all rules except E501
    `--ignore ALL --select E501` selects only E501

(If both selectors have the same specificity ignore selectors
take precedence.)

Ruff always had two quirks:

* If `pyproject.toml` specified `ignore = ["E501"]` then you could
  previously not override that with `--select E501` on the command-line
  (since the resolution didn't take into account that the select was
  specified after the ignore).

* If `pyproject.toml` specified `select = ["E501"]` then you could
  previously not override that with `--ignore E` on the command-line
  (since the resolution didn't take into account that the ignore was
  specified after the select).

Since d067efe2656dfa86c8ac2baa87f43f408e862a3a (#1245)
`extend-select` and `extend-ignore` always override
`select` and `ignore` and are applied iteratively in pairs,
which introduced another quirk:

* If some `pyproject.toml` file specified `extend-select`
  or `extend-ignore`, `select` and `ignore` became pretty much
  unreliable after that with no way of resetting that.

This commit fixes all of these quirks by making later configuration
sources take precedence over earlier configuration sources.

While this is a breaking change, it fixes the broken status quo.

Fixes #2300.

---

_Renamed from "Improve rule resolution" to "Improve rule config resolution" by @not-my-profile on 2023-01-28 20:30_

---

_Comment by @not-my-profile on 2023-01-28 20:44_

I think this makes much more sense and is much more user-friendly / understandable.

This obviously breaks existing configurations that relied on the previous weird behavior ... however I'd expect most ruff config files to not rely on this weird behavior ... but that's just a hunch.

How you want to go about this is very much your call. It would be possible to support both the old and the new config resolution at the same time and only use the new resolution based on some `tool.ruff.edition` setting but I am not really interested in implementing that.

---

_Comment by @charliermarsh on 2023-01-28 20:47_

I just have to take a look at some of the more complex setups in the wild and see how they'd be affected.

---

_@charliermarsh reviewed on 2023-01-28 21:36_

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:211 on 2023-01-28 21:36_

I think this is backwards, since we want our selections to take precedence over those that we're merging in (and so, they have to come later).

---

_@charliermarsh reviewed on 2023-01-28 21:37_

---

_Review comment by @charliermarsh on `src/settings/configuration.rs`:211 on 2023-01-28 21:37_

If I invert this, the Dagster repo (probably the most involved deployment) works as-is (no breaking change).

They have this config:

```toml
[tool.ruff]

# Extend root configuration.
extend = "../../pyproject.toml"

# Match black. Note that this also checks comment line length, but black does not format comments.
line-length = 88

# Use extend-ignore so that we ignore all the same codes ignored in root.
extend-ignore = [

  # (Unused import): When the same symbol is imported in multiple blocks, the
  # last import takes precedence for python. This causes Ruff to think an
  # import in an earlier block is unused and report F401.
  "F401",

  # (Redefinition): This happens frequently in docs_snippets when we import the same symbol in multiple
  # snippets within the same file.
  "F811",

  # (local variable assigned but never used): This happens a lot in docs snippets for didactic
  # purposes.
  "F841",

]
```

So, I guess that could now use `ignore` instead of `extend-ignore`, but it works either way.

---

_Comment by @charliermarsh on 2023-01-28 21:41_

> How you want to go about this is very much your call. It would be possible to support both the old and the new config resolution at the same time and only use the new resolution based on some tool.ruff.edition setting but I am not really interested in implementing that.

Don't worry, I definitely don't want to support both.

---

_@charliermarsh reviewed on 2023-01-28 21:45_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-28 21:45_

Do you mind adding some comments throughout this method? Or explaining the logic in more detail here? I'm tracing it but having trouble following.

As a specific example: if I have `select = ["F"]` in my `pyproject.toml`, and I do `--select A`, then I end up with two rule selections:

```rust
[RuleSelection { select: Some([Prefix { prefix: F, redirected_from: None }]), ignore: Some([Prefix { prefix: A, redirected_from: None }]), extend_select: None, extend_ignore: None, fixable: None, unfixable: None }, RuleSelection { select: Some([Prefix { prefix: A, redirected_from: None }]), ignore: None, extend_select: None, extend_ignore: None, fixable: None, unfixable: None }]
```

But, how do the `F` rules not end up in the select set?


---

_@charliermarsh reviewed on 2023-01-28 21:51_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-28 21:51_

Oh, I see -- do we, in effect, only use the last `RuleSelection` that contains a non-empty `select`? The rest gets discarded?

---

_@charliermarsh reviewed on 2023-01-28 21:53_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-28 21:53_

If that's the correct interpretation how would I do the following: create a `pyproject.toml` that enables all the `F` rules, then extend it from another `pyproject.toml` to also include the `E` rules? I think I know the answer, but it would be helpful to hear it.

---

_@charliermarsh reviewed on 2023-01-28 21:55_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-28 21:55_

Separate question: under this scheme, is there _ever_ any difference between specifying `ignore` and `extend-ignore`?


---

_@not-my-profile reviewed on 2023-01-29 03:19_

---

_Review comment by @not-my-profile on `src/settings/configuration.rs`:211 on 2023-01-29 03:19_

Ah yes ... very good catch! Thanks :) Fixed via force push.

---

_Review comment by @not-my-profile on `src/settings/configuration.rs`:211 on 2023-01-29 03:20_

Nice that my assumption was correct in that it likely doesn't break real world configs :)

---

_@not-my-profile reviewed on 2023-01-29 03:20_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:245 on 2023-01-29 03:49_

I added some comments :)

> if I have select = ["F"] in my pyproject.toml, and I do --select A, [..] But, how do the F rules not end up in the select set?

If `selection.select.is_some()` then the `select_set` is overridden.

> do we, in effect, only use the last RuleSelection that contains a non-empty select? The rest gets discarded?

No, multiple RuleSelections can all contribute to `select_set` if they only use `extend_select` / `ignore` but not `select`.

> how would I do the following: create a pyproject.toml that enables all the F rules, then extend it from another pyproject.toml to also include the E rules?

`select` can be extended via `extend-select`.

> Separate question: under this scheme, is there ever any difference between specifying ignore and extend-ignore?

Very good catch! I didn't even notice that but indeed we can deprecate `extend-ignore` since it now can always be replaced with `ignore`. The reasoning behind that is that now `ignore`/`extend-ignore` can only ever deselect already selected rules ... it's impossible for an ignore option to take precedence over a latter select option. Since the "ignore set" isn't something that is kept track of anymore ... the function only keeps track of the effectively selected rules, `extend-ignore` doesn't fulfill any purpose anymore.

---

_@not-my-profile reviewed on 2023-01-29 03:49_

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-29 04:02_

---

_@charliermarsh reviewed on 2023-01-29 18:49_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-29 18:49_

Cool, I _think_ this makes sense. I'd like to try adding a section in the README to explain this before we merge. If I do that, do you mind reviewing for clarity and correctness?

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:245 on 2023-01-29 20:29_

But of course :)

While I don't think this will affect most ruff configurations this technically still is a breaking change, so we probably should also document it in `BREAKING_CHANGES.md`?

---

_@not-my-profile reviewed on 2023-01-29 20:29_

---

_Comment by @not-my-profile on 2023-01-29 20:30_

(force pushed a small refactor that removed `extend_ignore` from the new `RuleSelection` struct and removed some needless `Option` wrapping around the fields that don't require it)

---

_@charliermarsh reviewed on 2023-01-29 20:36_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-29 20:36_

Definitely, will do. Mechanically, probably easiest for me to open those changes as a separate PR on `main` and then just rebase that PR after this merges...?

---

_@not-my-profile reviewed on 2023-01-29 20:40_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:245 on 2023-01-29 20:40_

You have write permissions on my branch so you could just push your commits there :)

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-29 22:37_

Ok, I went ahead and pushed a first-pass in the `README.md` and `BREAKING_CHANGES.md`. Feedback welcome.

---

_@charliermarsh reviewed on 2023-01-29 22:38_

---

_@not-my-profile reviewed on 2023-01-30 04:20_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:245 on 2023-01-30 04:20_

Thank you so much, I think you described it very well :)

---

_Comment by @not-my-profile on 2023-01-30 04:21_

(force pushed to fix the CI failure, which was caused by the README now including the `--extend-ignore` setting which this PR hides from the help output)

I think we should also mention in the new *Rule resolution* section in the README and the BREAKING_CHANGES.md entry that `extend-ignore` is now deprecated since it behaves exactly like `ignore`.

(This is now already mentioned in the documentation of the `extend-ignore` setting but I think it would make sense to repeat it ... note that I have also updated the CLI help to hide the `--extend-ignore` flag.)

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:245 on 2023-01-30 04:30_

Great. I'll try to get this out tomorrow!

---

_@charliermarsh reviewed on 2023-01-30 04:30_

---

_@charliermarsh reviewed on 2023-01-30 13:08_

---

_Review comment by @charliermarsh on `README.md`:437 on 2023-01-30 13:08_

I do _kind_ of wonder if we should just leave this visible in the CLI to avoid confusion (due to the asymmetry with `--select`). What do you think? Bad idea?

---

_@not-my-profile reviewed on 2023-01-30 17:33_

---

_Review comment by @not-my-profile on `README.md`:437 on 2023-01-30 17:33_

If we leave it I think we should add `DEPRECATED` to the description ... however I don't think that would really reduce the confusion since we'd have to explain why it's deprecated ... which would probably be too verbose. I consider `--extend-ignore` to effectively be an alias for `--ignore` ... and we currently don't document e.g. that `ruff clean` has `ruff --clean` as an alias in the CLI help.

I don't have a strong opinion about this, if you think it should be listed we can add it back.

Note that I have also removed `extend-ignore` from the JSON schema ... which could also be confusing for users but I think it's worth it to stop it from being autocompleted for users writing new configs.

---

_Review comment by @not-my-profile on `README.md`:437 on 2023-01-30 20:59_

I think I prefer to hide it. I have updated the message to mention that it's deprecated since it still shows up in the clap shell completion.

---

_@not-my-profile reviewed on 2023-01-30 20:59_

---

_Merged by @charliermarsh on 2023-01-30 21:26_

---

_Closed by @charliermarsh on 2023-01-30 21:27_

---

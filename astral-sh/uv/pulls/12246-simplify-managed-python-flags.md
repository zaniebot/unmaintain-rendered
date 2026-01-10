```yaml
number: 12246
title: Simplify managed Python flags
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/managed-python-flags
created_at: 2025-03-17T19:53:10Z
updated_at: 2025-04-28T15:45:38Z
url: https://github.com/astral-sh/uv/pull/12246
synced_at: 2026-01-10T11:10:39Z
```

# Simplify managed Python flags

---

_Pull request opened by @jtfmumm on 2025-03-17 19:53_

Currently, for users to specify at the command line whether to use uv-managed or system Python interpreters, they use the `--python-preference` parameter, which takes four possible values. This is more complex than necessary since the normal case is to either say "only managed" or "not managed". This PR hides the old `--python-preference` parameter from help and documentation and adds two new flags: `--managed-python` and `--no-managed-python` to capture the "only managed" and "not managed" cases.

I have successfully tested this locally but currently cannot add snapshot tests because of problems with distinguishing managed vs. system interpreters in CI (and non-determinism when run on different developers' machines). The `--python-preference` test in `tool-install.rs` is currently ignored for this reason. See #5144 and #7473.  


---

_Review requested from @zanieb by @zanieb on 2025-03-17 19:56_

---

_@zanieb reviewed on 2025-03-17 20:21_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:161 on 2025-03-17 20:21_

I would re-phrase (I think we use "Whether" when we only show one of the options)

```suggestion
    /// Disable use of uv-managed Python distributions.
```
I think we could say a bit more here, too, like

> Instead, uv will search for a suitable Python installation on the system.




---

_@zanieb reviewed on 2025-03-17 20:22_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:147 on 2025-03-17 20:22_

Similar to https://github.com/astral-sh/uv/pull/12246/files#r1999551644

```suggestion
    /// Only use uv-managed Python distributions.
```

(Sort of on the fence about "installations" vs "distributions" here but don't want users to be confused about whether we are "installing" here)



---

_@zanieb reviewed on 2025-03-17 20:23_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:151 on 2025-03-17 20:23_

```suggestion
    /// installed. This option disables use of system Python versions.```

---

_@zanieb reviewed on 2025-03-17 20:23_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:147 on 2025-03-17 20:23_

I guess "versions" is actually the most consistent with other language?

---

_@zanieb reviewed on 2025-03-17 20:23_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:161 on 2025-03-17 20:23_

See https://github.com/astral-sh/uv/pull/12246/files#r1999556537 — maybe we want to say "Python versions".

---

_@zanieb reviewed on 2025-03-17 20:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:166 on 2025-03-17 20:24_

I think we prefer `overrides_with` for this. 

---

_@zanieb reviewed on 2025-03-17 20:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:166 on 2025-03-17 20:24_

(Though `conflicts_with` makes sense for `python_preference`, I think)

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:147 on 2025-03-17 20:27_

I wonder if we should also mention that this is equivalent to the `python-preference = "only-managed"` setting? I'm not sure. We might want to reference `python-preference` though unless we intend to later add a `managed-python = true | false` option to the settings to replace `python-preference`?

---

_@zanieb reviewed on 2025-03-17 20:27_

---

_@zanieb reviewed on 2025-03-17 20:28_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:283 on 2025-03-17 20:28_

We didn't remove this option from the settings reference too, did you mean to remove this link?


---

_@zanieb reviewed on 2025-03-17 20:30_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:281 on 2025-03-17 20:30_

I wonder if we should reframe similar to above? Like, rename to "Disabling managed Python versions"?

We should definitely describe the `--managed-python` and `--no-managed-python` flags here though.

---

_@zanieb reviewed on 2025-03-17 20:31_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:147 on 2025-03-17 20:31_

I also think a `UV_MANAGED_PYTHON=1|0` variable might makes sense for these new options. It seems easier to use than `UV_PYTHON_PREFERENCE`.

---

_@jtfmumm reviewed on 2025-03-18 09:35_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:283 on 2025-03-18 09:35_

I was trying to split the difference in terms of what we're emphasizing, but I've restored the link.

---

_@jtfmumm reviewed on 2025-03-18 09:37_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:281 on 2025-03-18 09:37_

I've added the flag descriptions. 

I wonder if we should keep the `python-preference` section in here at all. It's a little confusing to have them both. If we remove it, we could rename the section.

---

_@jtfmumm reviewed on 2025-03-18 09:39_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:147 on 2025-03-18 09:39_

I'm hesitant to reference `python-preference` because the user might then try to learn two different ways to do the same thing (which might defeat part of the purpose of simplifying configuration here).

---

_@jtfmumm reviewed on 2025-03-18 09:41_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:147 on 2025-03-18 09:41_

It would be nice to use the same language consistently. I'm fine with "versions". There are a number of other points in help/docs where we say "Python installations" or "Python distributions". And we say "Python installations" in output (e.g., "Searching for Python installations"). 

I could open a separate issue for unifying this language and just update these ones to "versions" for now

---

_@jtfmumm reviewed on 2025-03-18 09:52_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:147 on 2025-03-18 09:52_

I considered an env var, but the problem is we have a third option as a default. 

We could use `UV_MANAGED_PYTHON` to mean 1 == required, 0 == not required (i.e., managed then system). But then we don't have "forbidden" represented. I didn't want another multi-value env var if we still have `UV_PYTHON_PREFERENCE`. But it also seemed a little awkward to add a `UV_NO_MANAGED_PYTHON` env var. 

What do you think?

---

_@zanieb reviewed on 2025-03-18 13:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:147 on 2025-03-18 13:14_

> It would be nice to use the same language consistently.

Yeah, but there are some places where it might be clearer to refer to installations or distributions. I think we should probably use "versions" when it's not confusing. It'd be good to audit other usages too, I'm sure there are more cases where we could switch to "versions".

I think I'd prefer updating the ones here then opening an issue to standardize the language.

> I'm hesitant to reference python-preference because the user might then try to learn two different ways to do the same thing 

Since there's not a configuration file setting for `managed-python`, I think it makes sense to explain how it's related? I wouldn't suggest the `--python-preference` CLI option.

> I considered an env var, but the problem is we have a third option as a default.

Thinking out loud here...

I assumed `UV_MANAGED_PYTHON=1` means `--managed-python` and `UV_MANAGED_PYTHON=0` means `--no-managed-python` while unset means default.

Looking at some of our existing patterns, we do use `UV_NO_...` sometimes. I think most of them don't have an equivalent "yes" flag though. I might be okay with `UV_MANAGED_PYTHON=1` means `--managed-python` and `UV_NO_MANAGED_PYTHON=1` means `--no-managed-python`. I think `UV_MANAGED_PYTHON=0` not working would be surprising. That's how other boolean flags work today though, e.g.:

```
❯ UV_SYSTEM_PYTHON=0 uv python find
/opt/homebrew/opt/python@3.13/bin/python3.13
```

but as you said, that's for an option that's truly binary.

I think I've loosely talked myself into using `UV_NO_MANAGED_PYTHON` / `UV_MANAGED_PYTHON` :)


 

---

_@zanieb reviewed on 2025-03-18 13:30_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:281 on 2025-03-18 13:30_

I posted a draft of what I would write at https://github.com/astral-sh/uv/pull/12279

---

_@jtfmumm reviewed on 2025-03-18 13:40_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:147 on 2025-03-18 13:40_

Yeah if `UV_MANAGED_PYTHON` is the only env var, I do think `UV_MANAGED_PYTHON=0` feels like "don't use managed Python". But I'd find the 0/1/unset options to be surprising. I guess I'd assume the default would effectively be 0. 

But if we have two separate env vars `UV_MANAGED_PYTHON` and `UV_NO_MANAGED_PYTHON` this is probably less surprising, since it would mean 1 == use the flag, 0 == don't use the flag.

---

_@zanieb reviewed on 2025-03-18 13:42_

---

_Review comment by @zanieb on `docs/concepts/python-versions.md`:281 on 2025-03-18 13:42_

(feel free to merge it or crib what you want)

---

_@jtfmumm reviewed on 2025-03-18 13:44_

---

_Review comment by @jtfmumm on `docs/concepts/python-versions.md`:281 on 2025-03-18 13:44_

I like your proposal, though I still worry about it being confusing to read through both in sequence. Instead of 4 options to learn, it seems like there are 6 options to learn. But I'm also ok with merging your changes.

---

_Comment by @zanieb on 2025-03-18 16:39_

> I like your proposal, though I still worry about it being confusing to read through both in sequence. Instead of 4 options to learn, it seems like there are 6 options to learn. But I'm also ok with merging your changes.

I think there's probably a long-term improvement we can make here, like... introducing a `managed-python` setting (as you proposed) and phasing out the `python-preference` entirely. I'm just not certain what that should look like yet. I'm hoping feedback on this incremental improvement can help guide a future decision. 

---

_@zanieb approved on 2025-03-18 16:40_

I don't see environment variables as blocking (we often add them in follow-ups), but I do think we should add them — people often use `UV_PYTHON_PREFERENCE` in Docker containers and I'd like to offer this simpler option instead. (Speaking of that, we might need to look at updating the `uv-docker-example` and the Docker integration guide)

---

_Comment by @jtfmumm on 2025-03-18 17:10_

I've added the env vars.

---

_Merged by @jtfmumm on 2025-03-18 17:13_

---

_Closed by @jtfmumm on 2025-03-18 17:13_

---

_Branch deleted on 2025-03-18 17:13_

---

_Comment by @pfmoore on 2025-03-21 17:36_

So the only way of getting `--python-preference=system` is now to use a hidden option, is that correct? There's no intention in the longer term to *remove* the `--python-preference=system` option, is there?

---

_Comment by @zanieb on 2025-03-21 17:38_

> There's no intention in the longer term to remove the --python-preference=system option, is there?

There's no plan to currently. It was considered, but I think it's valuable to advanced users and we need to retain it for backwards compatibility anyway.

> So the only way of getting --python-preference=system is now to use a hidden option, is that correct?

Yeah, or to use the non-hidden configuration file setting.

---

_Comment by @pfmoore on 2025-03-21 17:47_

OK. Please don't remove this option until there is better support for managing "mixed" environments with both system and uv-managed interpreters (see the discussion under https://github.com/astral-sh/uv/issues/12122 for context).

Currently, preferring the system Python is the only viable way of having a standard Python install while using uv-managed Pythons for temporary testing, unless you're willing to forego things like `uv venv` or `uv run` when you want to use your system Python.

---

_Comment by @zanieb on 2025-03-21 17:50_

We intend to fix those other problems you're having regardless, but yeah.

---

_Comment by @mikenerone on 2025-04-28 15:42_

Even if the older preference option remains (stealthed), it seems like an inconsistency that the option is hidden in docs and help output, but the environment variable is [still included](https://docs.astral.sh/uv/configuration/environment/#uv_python_preference).

---

_Comment by @zanieb on 2025-04-28 15:45_

The environment variable matches the persistent setting at https://docs.astral.sh/uv/reference/settings/#python-preference — I think it seems reasonable to keep?

---

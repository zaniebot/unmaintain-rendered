---
number: 18349
title: Warn about error being related to a deprecated rule
type: issue
state: closed
author: Andrej730
labels:
  - cli
  - help wanted
assignees: []
created_at: 2025-05-28T09:48:35Z
updated_at: 2025-09-08T16:10:40Z
url: https://github.com/astral-sh/ruff/issues/18349
synced_at: 2026-01-07T13:12:16-06:00
---

# Warn about error being related to a deprecated rule

---

_Issue opened by @Andrej730 on 2025-05-28 09:48_

### Summary

My case - was trying out Ruff and enabled `UP`. Going through the log of found errors, I've found that [UP038](https://docs.astral.sh/ruff/rules/non-pep604-isinstance/) from my point of view seems unreasonable, checking the documentation I've found that it's actually deprecated and will be removed soon.

A suggestion - `ruff check` to warn that found error is related to a deprecated rule, so it will be possible to find deprecated rules just by using `ruff`, without checking documentation. This would improve rule deprecation discoverability.

(I'm hope I'm not missing some option that already does that, but couldn't find anything related in https://docs.astral.sh/ruff/settings/)

```python
a = 25
print(isinstance(25, (str, int)))
# ruff check L:\1_test.py --select UP --target-version py311
# UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
#   |
# 2 | a = 25
# 3 | print(isinstance(25, (str, int)))
#   |       ^^^^^^^^^^^^^^^^^^^^^^^^^^ UP038
#   |
#   = help: Convert to `X | Y`
# 
# Found 1 error.
# No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_Comment by @MichaReiser on 2025-05-28 10:41_

This should already be the case:

```shell
❯ uvx ruff@latest check --select=UP038 
Installed 1 package in 2ms
warning: Rule `UP038` is deprecated and will be removed in a future release.
All checks passed!
```

What version of Ruff are you using?

---

_Label `question` added by @MichaReiser on 2025-05-28 10:41_

---

_Comment by @Andrej730 on 2025-05-28 11:11_

ruff version:
```
PS L:\> uvx ruff@latest --version                                                               
Installed 1 package in 7ms
ruff 0.11.11
PS L:\> uvx ruff@latest check L:\1_test.py --select UP --target-version py311
Installed 1 package in 8ms
1_test.py:2:7: UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
  |
1 | a = 25
2 | print(isinstance(25, (str, int)))
  |       ^^^^^^^^^^^^^^^^^^^^^^^^^^ UP038
  |
  = help: Convert to `X | Y`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

Oh, I see, it seems warning only occurs if you run deprecated rule directly.
```
PS L:\> uvx ruff@latest check L:\1_test.py --select UP038 --target-version py311
Installed 1 package in 8ms
warning: Rule `UP038` is deprecated and will be removed in a future release.
1_test.py:2:7: UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
  |
1 | a = 25
2 | print(isinstance(25, (str, int)))
  |       ^^^^^^^^^^^^^^^^^^^^^^^^^^ UP038
  |
  = help: Convert to `X | Y`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_Comment by @MichaReiser on 2025-05-28 16:27_

Yeah, I'm somewhat surprise that we include deprecated rules in cases where there's no direct match

This was implemented in https://github.com/astral-sh/ruff/pull/9689 but I couldn't find much rational to why we include deprecated rules when they're not explicitly selected.

---

_Comment by @MichaReiser on 2025-05-28 16:34_

It seems I raised this concern on an earlier PR https://github.com/astral-sh/ruff/pull/10497#issuecomment-2188376434

I'd be interested to hear from more folks on what they think (also @zanieb as the author of our rule deprecation infrastructure)

---

_Comment by @Andrej730 on 2025-05-28 18:10_

> but I couldn't find much rational to why we include deprecated rules when they're not explicitly selected.

I had the same thought meeting this, but it does make some sense - e.g. user is using `UP` and relying on all it's rules and maybe they find deprecated rule useful. So from their POV they might never know that rule is no longer working, if it will be just removed from `UP`, because it's just got deprecated.

But it has the downside that when someone is just starting to use Ruff, like me, and when trying to select a group of rules to try out, they'll need to get some of those that are reasonably deprecated and they'll need to start excluding stuff, even though you just started and never met those rules before. No idea if there's some middle ground to satisfy both cases.

Personally, I'd prefer getting all the rules when selecting the group, including the deprecated ones, but Ruff somehow communicating that the rule that just got triggered is deprecated and error should be taken with a grain of salt. But that's just me, generally new users would expect deprecated rules to be just excluded for a smoother starting experience.

---

_Comment by @MichaReiser on 2025-06-02 09:49_

> I had the same thought meeting this, but it does make some sense - e.g. user is using UP and relying on all it's rules and maybe they find deprecated rule useful. So from their POV they might never know that rule is no longer working, if it will be just removed from UP, because it's just got deprecated.

I can follow this reasoning. But the rule gets "removed" once we promote it from deprecated to removed. That means we only defer the point where users lose access to a rule. 

Given that we have a high bar for deprecating rules (they have to be harmful), I think it would be a better outcome to exclude them when they're deprecated. It avoids confusion for new users but also existing users that just haven't run into whatever rough edges the rule has (and is the reason why it gets removed). 

> Personally, I'd prefer getting all the rules when selecting the group, including the deprecated ones, but Ruff somehow communicating that the rule that just got triggered is deprecated and error should be taken with a grain of salt.

I think this would be annoying for users. Every user then starts seeing a deprecated warning and they then need to figure out how they can silence it. I'm not even sure if adding the rule to `ignore is sufficient to silence it. 

CC: @ntBre  @dylwil3 wdyt? this might be a smaller change that we could ship as part of the minor

---

_Comment by @ntBre on 2025-06-02 12:38_

I think it makes sense to exclude the deprecated rules from selection with their group. It's also nice that we already have a warning for the direct match case, which the group exclusion would push users towards, if they _really_ want the rule.

---

_Comment by @dylwil3 on 2025-06-02 13:47_

Agreed - deprecated rules should only be allowed to be deliberately, individually selected

---

_Label `cli` added by @MichaReiser on 2025-06-02 13:54_

---

_Label `help wanted` added by @MichaReiser on 2025-06-02 13:54_

---

_Label `question` removed by @MichaReiser on 2025-06-02 13:54_

---

_Added to milestone `v0.12` by @MichaReiser on 2025-06-02 13:54_

---

_Comment by @zanieb on 2025-06-02 18:51_

I'm supportive of this change, but then we need to defer deprecations to breaking releases and I think I originally was optimizing for being able to perform deprecations in arbitrary releases (that's my general pattern, introducing warnings as early as possible).

If the pitch is to remove selection of rules on deprecation in non-breaking releases, I'm strongly opposed.

Another option is that we add `SoftDeprecated` and a `HardDeprecated` types, with these two different behaviors. I don't really think that's merited, but figure I'd throw it out there.

---

_Comment by @MichaReiser on 2025-06-03 06:37_

> I'm supportive of this change, but then we need to defer deprecations to breaking releases and I think I originally was optimizing for being able to perform deprecations in arbitrary releases (that's my general pattern, introducing warnings as early as possible).

Oh, that makes sense. I don't think we ever did this because we considered breaking for users that explicitly opt in to the rule (you see a warning and there's nothing you can do about it). But it's a good call out that we should also update the versioning policy if we make this change

---

_Removed from milestone `v0.12` by @ntBre on 2025-06-10 20:38_

---

_Added to milestone `v0.13` by @ntBre on 2025-06-10 20:38_

---

_Comment by @LoicRiegel on 2025-08-23 19:56_

Hi, what's the status of this?

I found myself in the same situation as @Andrej730, which led me here.

The issue is marked with "help-wanted", so does this mean that nobody is working on it? I've never contributed to ruff and I have limited time, but if the effort is reasonnable and you provide me with some guidance, I can work on it

---

_Comment by @ntBre on 2025-08-25 16:45_

Yeah, I don't think anyone has started working on this yet. From the thread here, it sounds like the changes that need to be done are:
- Requiring explicit selection of deprecated rules
  That should be a fairly simple change in this area, where #10497 also modified:
  https://github.com/astral-sh/ruff/blob/db423ee9789b3a045266665198c4b25f31a23b6f/crates/ruff_linter/src/rule_selector.rs#L213-L220
- Updating our versioning policy to make "A rule is deprecated" a minor version change
  https://github.com/astral-sh/ruff/blob/67200c966852fdc038be9c17a7578f80f5ddcd9d/docs/versioning.md?plain=1#L14-L22
  It looks like we don't currently have this listed anywhere in the versioning policy

With the first of these, we'll probably also want a CLI test in this file: [`lint.rs`](https://github.com/astral-sh/ruff/blob/67200c966852fdc038be9c17a7578f80f5ddcd9d/crates/ruff/tests/lint.rs). Although we might already have this covered in the tests around here:

https://github.com/astral-sh/ruff/blob/67200c966852fdc038be9c17a7578f80f5ddcd9d/crates/ruff/tests/integration_test.rs#L1472-L1481


---

_Referenced in [astral-sh/ruff#20167](../../astral-sh/ruff/pulls/20167.md) on 2025-08-30 14:45_

---

_Closed by @ntBre on 2025-09-08 16:10_

---

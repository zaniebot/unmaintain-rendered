```yaml
number: 9872
title: "v0.1.6: Overriding sub-setting category nulls other inherited settings (#4348)"
type: issue
state: open
author: LTourinho
labels:
  - configuration
assignees: []
created_at: 2024-02-07T12:02:38Z
updated_at: 2024-12-09T11:10:04Z
url: https://github.com/astral-sh/ruff/issues/9872
synced_at: 2026-01-10T11:09:52Z
```

# v0.1.6: Overriding sub-setting category nulls other inherited settings (#4348)

---

_Issue opened by @LTourinho on 2024-02-07 12:02_

Very similar to the issue described in https://github.com/astral-sh/ruff/issues/4348. I have a root dir with a file that looks a bit like:

proj/.ruff.toml:
>[lint.isort]
section-order = ["standard-library", "common_pkgs", "third-party", "same_subcomp"]
[lint.isort.sections]
"common_pkgs" = ["numpy", "behave", <other examples>]
"same_subcomp"= []

proj/subdir/.ruff.toml:
>extend = "../.ruff.toml"
[lint.isort.sections]
"same_subcomp" = ["\<pattern\>"]

Using a command such as ruff proj/subdir/<some_file> returns me an error like:
>warning: `section-order` contains unknown section: `UserDefined("common_pkgs")`

Which means that isort.sections is being completely overwritten, when I expected to only overwrite the value of "same_subcomp". I can also confirm that the sorting of imports is not behaving as expected, as it will put numpy in the middle of the third party, when I want it to have its own block.

If this is the intended behavior, which I assumed it wasn't given issue #4348, is there a simple way I can achieve *my* intended behavior without copying the definition of common_pkgs to the .ruff.toml in every subdir?

Thanks in advance.


---

_Renamed from "At version 0.1.6, I am experiencing the same bug as shown in https://github.com/astral-sh/ruff/issues/4348" to "v0.1.6: Overriding sub-setting category nulls other inherited settings (#4348)" by @zanieb on 2024-02-07 14:38_

---

_Label `bug` added by @zanieb on 2024-02-07 14:39_

---

_Comment by @charliermarsh on 2024-02-11 03:15_

Yeah, I think this should probably be considered a bug.

---

_Comment by @LTourinho on 2024-02-21 09:04_

Hi, I see that the PR is approved and checks are green. What would be the ETA on this bugfix delivery?

---

_Comment by @charliermarsh on 2024-02-26 15:55_

@LTourinho -- It's technically a breaking change, so it will go out in v0.3.

---

_Comment by @charliermarsh on 2024-03-01 02:05_

@LTourinho -- Sorry for the delay, we decided to defer this to v0.4 because we need to figure out a way to allow extended configs to _unset_ these.

---

_Comment by @LTourinho on 2024-03-01 08:42_

> @LTourinho -- Sorry for the delay, we decided to defer this to v0.4 because we need to figure out a way to allow extended configs to _unset_ these.

No problem. Is the suggestion of setting to an "empty" object too bad in that regard? (The comment I made on the PR)

---

_Comment by @LTourinho on 2024-03-08 10:59_

Hi @charliermarsh, as the PR was closed, what are the plans on fixing this issue?

---

_Comment by @MichaReiser on 2024-03-08 11:44_

> Hi @charliermarsh, as the PR was closed, what are the plans on fixing this issue?

Sorry that was not intentional. I cleaned up my stale branches and thought it's safe to delete any branch starting with `charliemarsh`. I restored the branch and reopened the PR.

---

_Comment by @charliermarsh on 2024-03-08 15:18_

Thanks Micha, I meant to ask about that too :)

---

_Comment by @LTourinho on 2024-04-29 09:27_

Hi guys, is there a reason this was postponed to 0.5? No agreement on how to unset values?

---

_Comment by @MichaReiser on 2024-04-29 14:01_

> Hi guys, is there a reason this was postponed to 0.5? No agreement on how to unset values?

There was no specific reason other than time constraints. We didn't want to block the 0.4 release by making this decision.

---

_Comment by @charliermarsh on 2024-04-29 14:02_

0.4 was focused on the parser, we punted other breaking changes to 0.5 which we expect to release in the next few weeks.

---

_Comment by @MichaReiser on 2024-06-25 08:31_

To confirm the behavior change. We'll now start merging the following options:

* `per_file_ignores`
* `extend-per-file-ignores` (Do we still need the `extend`?) 
* `flake8_import_convention_options.aliases`
* `flake8_import_convention_options.extend_aliases`
* `flake8_import_convention_options.banned_aliases`
* `flake8_import_convention_options.banned_from`
* `flake8_tidy_imports.banned_api`
* `isort.sections`

I'm not sure whether the new behavior is correct for all settings, or at least it becomes less clear why we would still need the `extend-` options when "extending" becomes the new default for tables. 

The new `CombineOptions` allows us to use different merging behaviors for different settings, but I'm worried this will be confusing. 

This makes me hesitant to merge this change. @charliermarsh what are your thoughts on this?

---

_Comment by @LTourinho on 2024-07-04 14:32_

Hi, I see that the previous solution pull request was closed. Is there no alternatives? Is this being picked up in any way? 

---

_Comment by @MichaReiser on 2024-07-04 14:36_

Unfortunately, I know of no alternative. We aren't actively working on this right now but are open to ideas. As little as I like that, a short-term solution could be an `extend` option. [This comment](https://github.com/astral-sh/ruff/pull/9927#issuecomment-2191086650) explains why the other PR was closed)

---

_Comment by @LTourinho on 2024-07-04 15:17_

Could there be a different keyword? E.g.: merge-into = "../.base_ruff.toml"

Then the breaking functionality would only happen to those who explicitly use the new keyword.

---

_Comment by @MichaReiser on 2024-07-05 08:05_

> Could there be a different keyword? E.g.: merge-into = "../.base_ruff.toml"

I would prefer not to but I won't rule it out. Either way, deciding on adding a different extension strategy isn't something we want to do lightly because it adds a lot of complexity and also makes it harder for users to understand what's happening. 

---

_Comment by @KotlinIsland on 2024-07-11 04:50_

- Related (duplicate?) #7696

---

_Label `configuration` added by @MichaReiser on 2024-12-09 11:10_

---

_Label `bug` removed by @MichaReiser on 2024-12-09 11:10_

---

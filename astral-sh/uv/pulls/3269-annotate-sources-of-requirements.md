```yaml
number: 3269
title: Annotate sources of requirements
type: pull_request
state: merged
author: palfrey
labels:
  - enhancement
assignees: []
merged: true
base: main
head: annotate-requirements-sources
created_at: 2024-04-25T20:29:00Z
updated_at: 2024-05-09T13:35:33Z
url: https://github.com/astral-sh/uv/pull/3269
synced_at: 2026-01-10T14:37:54Z
```

# Annotate sources of requirements

---

_Pull request opened by @palfrey on 2024-04-25 20:29_

## Summary

Fixes https://github.com/astral-sh/uv/issues/1343. This is kinda a first draft at the moment, but does at least mostly work locally (barring some bits of the test suite that seem to not work for me in general).

## Test Plan

Mostly running the existing tests and checking the revised output is sane

## Outstanding issues

Most of these come down to "AFAIK, the existing tools don't support these patterns, but `uv` does" and so I'm not sure there's an existing good answer here! Most of the answers so far are "whatever was easiest to build"

- [x] ~~Is "-r pyproject.toml" correct? Should it show something else or get skipped entirely~~ No it wasn't. Fixed in 3044fa8b86791369a3c468ced585753428e70b1b
- [ ] If the requirements file is stdin, that just gets skipped. Should it be recorded?
- [ ] Overrides get shown as "--override<override.txt>". Correct?
- [x] ~~Some of the tests (e.g. `dependency_excludes_non_contiguous_range_of_compatible_versions`) make assumptions about the order of package versions being outputted, which this PR breaks. I'm not sure if the text is fairly arbitrary and can be replaced or whether the behaviour needs fixing?~~ - fixed by removing the custom pubgrub PartialEq/Hash
- [ ] Are all the `TrackedFromStr` et al changes needed, or is there an easier way? I don't think so, I think it's necessary to track these sort of things fairly comprehensively to make this feature work, and this sort of invasive change feels necessary, but happy to be proved wrong there :)
- [x] ~~If you have a requirement coming in from two or more different requirements files only one turns up. I've got a closed-source example for this (can go into more detail if needed), mostly consisting of a complicated set of common deps creating a larger set. It's a rarer case, but worth considering.~~ 042432b200395d498b4517121a9fb25f1bbda8ca
- [ ] Doesn't add annotations for `setup.py` yet
    - This is pretty hard, as the correct location to insert the path is `crates/pypi-types/src/metadata.rs`'s `parse_pkg_info`, which as it's based off a source distribution has entirely thrown away such matters as "where did this package requirement get built from". Could add "`built package name`" as a dep, but that's a little odd.

---

_@palfrey reviewed on 2024-04-25 20:29_

---

_Review comment by @palfrey on `crates/pep440-rs/src/tracked_from_str.rs`:4 on 2024-04-25 20:29_

I am not entirely sure this is the right package, but AFAIK, it's the only crate that everything that needs it can depend on....

---

_@palfrey reviewed on 2024-04-25 20:32_

---

_Review comment by @palfrey on `crates/uv-resolver/src/pubgrub/package.rs`:16 on 2024-04-25 20:32_

If we use the default ones, two requirements with different sources get flagged as different packages, and then you get duplicates in the output. Replacing them with hand-tooled options isn't the best of plans, but it is one that works and I don't have a better idea so far.

---

_Comment by @ThiefMaster on 2024-04-25 21:57_

I just tested it with the requirements files of https://github.com/indico/indico/, and for `requirements.{in,txt}` it works great - no changes beyond the comments on top! :)

For `requirements.dev.{in,txt}`, however, I noticed one bug: Instead of `-c requirements.txt` it uses `-r requirements.txt` in the source comments. That aside, it seems to do everything fine there as well.

---

_Comment by @zanieb on 2024-04-25 22:45_

Exciting! I'm not a good review for this one, but I wonder if it could be used to address https://github.com/astral-sh/uv/issues/1854 next?

---

_@charliermarsh reviewed on 2024-04-25 23:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/package.rs`:64 on 2024-04-25 23:06_

Could we instead omit this here, and re-connect the requirements to their paths when we build the `ResolutionGraph`?

---

_@charliermarsh reviewed on 2024-04-25 23:45_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/package.rs`:64 on 2024-04-25 23:45_

I think that could also enable correct annotation for (e.g.) `-c requirements.txt`, since then you could identify requirements that came from constraints.

---

_Comment by @palfrey on 2024-04-26 15:46_

> I just tested it with the requirements files of https://github.com/indico/indico/, and for `requirements.{in,txt}` it works great - no changes beyond the comments on top! :)
> 
> For `requirements.dev.{in,txt}`, however, I noticed one bug: Instead of `-c requirements.txt` it uses `-r requirements.txt` in the source comments. That aside, it seems to do everything fine there as well.

9655e9139d7ea2ea02d979fcc255d45542aa287f should fix that.

---

_@palfrey reviewed on 2024-04-26 20:12_

---

_Review comment by @palfrey on `crates/uv-resolver/src/pubgrub/package.rs`:64 on 2024-04-26 20:12_

I started writing "here's why this doesn't work" and then found out in the course of writing it, that yes, it can be done! A tool I really want but can't seem to find with a bit of googling is some sort of code visualisation thing for Rust that'd let me visually see all of this v.s. the "lets cram it all into my head" approach I've been using, which clearly isn't the best option.

Here's my notes:

The only code that actually notices -r/-c requirements is `parse_entry` in the `requirements-txt` crate, which then only gets used by `parse`/`parse_inner` in `RequirementsTxt`. This results in path data in `RequirementEntry` and source values in the `Requirement`'s that are the `constraints` in the `RequirementsTxt` struct.

That in turn gets called by `RequirementsSpecification` `from_source`/`from_sources`, which similarly builds `requirements`/`constraints` with path/source data.

`pip_compile` in the `uv` crate then calls all that, feeds it into the pubgrub bits for resolution, and then feeds the results into `DisplayResolutionGraph` for display.

So, yeah. I could take the data from `RequirementsSpecification`, fish out a package -> sources hashmap and skip adding it to Pubgrub entirely... let me have a look at that.

---

_@palfrey reviewed on 2024-04-26 21:29_

---

_Review comment by @palfrey on `crates/uv-resolver/src/pubgrub/package.rs`:64 on 2024-04-26 21:29_

Fixed! Also resolved some test failures :)

---

_Comment by @palfrey on 2024-04-26 21:31_

@charliermarsh I'm seeing some test failures on Windows, and they appear to be related to path redactions in insta. There's some nice work for doing that cross-platform in `crates/uv/tests/common/mod.rs`, but I'm needing it for things like `crates/requirements-txt/src/lib.rs` and that would be a circular dependency. Any thoughts on how to approach sorting those out? I don't have an easy way to do Windows work sadly, so this might be a bit fiddly!

---

_Comment by @ThiefMaster on 2024-04-27 12:23_

> https://github.com/astral-sh/uv/commit/9655e9139d7ea2ea02d979fcc255d45542aa287f should fix that.

Indeed, the output is exactly what I expect now!

<details>

```diff
[adrian@claptrap:~/dev/indico/src:master *$]> git diff
diff --git a/docs/requirements.txt b/docs/requirements.txt
index af095861bf..e16ac62be7 100644
--- a/docs/requirements.txt
+++ b/docs/requirements.txt
@@ -1,9 +1,5 @@
-#
-# This file is autogenerated by pip-compile with Python 3.12
-# by the following command:
-#
-#    pip-compile --strip-extras docs/requirements.in
-#
+# This file was autogenerated by uv via the following command:
+#    uv pip compile -o docs/requirements.txt docs/requirements.in
 alabaster==0.7.16
     # via sphinx
 babel==2.14.0
diff --git a/requirements.dev.txt b/requirements.dev.txt
index 6146f0535c..3fdd46a5bd 100644
--- a/requirements.dev.txt
+++ b/requirements.dev.txt
@@ -1,9 +1,5 @@
-#
-# This file is autogenerated by pip-compile with Python 3.12
-# by the following command:
-#
-#    pip-compile --strip-extras requirements.dev.in
-#
+# This file was autogenerated by uv via the following command:
+#    uv pip compile -o requirements.dev.txt requirements.dev.in
 aiosmtpd==1.4.5
     # via pytest-localserver
 alabaster==0.7.16
@@ -125,6 +121,8 @@ parso==0.8.4
     #   pyquotes
 pep8-naming==0.13.3
     # via -r requirements.dev.in
+pip==24.0
+    # via pip-tools
 pip-tools==7.4.1
     # via -r requirements.dev.in
 plantweb==1.2.1
@@ -190,6 +188,10 @@ ruff==0.4.1
     # via -r requirements.dev.in
 schemainspect==3.1.1663587362
     # via migra
+setuptools==69.5.1
+    # via
+    #   flake8-quotes
+    #   pip-tools
 six==1.16.0
     # via
     #   -c requirements.txt
@@ -267,7 +269,3 @@ werkzeug==3.0.2
     #   pytest-localserver
 wheel==0.43.0
     # via pip-tools
-
-# The following packages are considered to be unsafe in a requirements file:
-# pip
-# setuptools
diff --git a/requirements.txt b/requirements.txt
index 32521e6a04..a8ec374820 100644
--- a/requirements.txt
+++ b/requirements.txt
@@ -1,9 +1,5 @@
-#
-# This file is autogenerated by pip-compile with Python 3.12
-# by the following command:
-#
-#    pip-compile --strip-extras
-#
+# This file was autogenerated by uv via the following command:
+#    uv pip compile -o requirements.txt requirements.in
 alembic==1.13.1
     # via
     #   -r requirements.in
```

</details>

---

_Comment by @jardarbo on 2024-04-29 17:39_

@palfrey Very nice of you to look at this!

Testing your version with the following `pyproject.toml`:
```bash
$ cat pyproject.toml
[project]
name = "uv_test"
dynamic = ["version"]
dependencies = ["click"]
```
Running `uv pip compile` with your version yields the following:
```bash
$ uv pip compile pyproject.toml
Resolved 1 package in 4ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml
click==8.1.7
    # via -r pyproject.toml
```
Running `pip-compile` (https://github.com/jazzband/pip-tools) yields:
```bash
$ pip-compile pyproject.toml
#
# This file is autogenerated by pip-compile with Python 3.12
# by the following command:
#
#    pip-compile pyproject.toml
#
click==8.1.7
    # via uv_test (pyproject.toml)
```

The difference is `# via -r pyproject.toml` vs. `# via uv_test (pyproject.toml)`.

I don't know if you think we should match the output of `pip-compile`, but it makes it easier to mix the usage of `uv pip compile` and `pip-compile`.

Running this on Ubuntu on Windows 11 WSL.

---

_Comment by @ThiefMaster on 2024-04-29 17:48_

+1 on matching the `pip-compile` output, it makes transitioning from pip-tools to uv much more straightforward (as you immediately see that nothing changed), and `-r pyproject.toml` seems weird anyway (unless `pip install -r pyproject.toml` is valid which I'd find surprising)

---

_Comment by @palfrey on 2024-04-29 22:23_

> The difference is `# via -r pyproject.toml` vs. `# via uv_test (pyproject.toml)`.
> 
> I don't know if you think we should match the output of `pip-compile`, but it makes it easier to mix the usage of `uv pip compile` and `pip-compile`.

I didn't investigate this path much, but had seen `-r pyproject.toml` and wasn't sure offhand how this was meant to be handled. Now I know, 3044fa8b86791369a3c468ced585753428e70b1b fixes this.


---

_Comment by @jardarbo on 2024-04-30 04:41_

> Now I know, https://github.com/astral-sh/uv/commit/3044fa8b86791369a3c468ced585753428e70b1b fixes this.

Wow, this was quick @palfrey, thanks! This does indeed make output same as `pip-compile`.

<details>

```bash
$ uv pip compile pyproject.toml
Resolved 1 package in 4ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml
click==8.1.7
    # via uv-test (pyproject.toml)
```

</details>

---

_Comment by @palfrey on 2024-05-05 22:00_

@charliermarsh I've just updated to latest main. I'd regard this as done, except for the Windows test failures, which I'm not sure how to fix ([see previous comment on this](https://github.com/astral-sh/uv/pull/3269#issuecomment-2080143202)). However, that was _painful_ to update, so a review of this very soon would be appreciated!

If anyone else has bright ideas about how to fix the Windows issue, I'm all ears.

---

_Comment by @charliermarsh on 2024-05-05 22:37_

Thank you @palfrey and sorry for the painful rebase. I'd been holding off on reviewing this until the `sources` PR merged, since I knew it would lead to changes here, but I should've said so earlier. I will own reviewing now.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-05 22:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-05 22:37_

---

_Label `enhancement` added by @charliermarsh on 2024-05-05 22:37_

---

_Comment by @palfrey on 2024-05-05 22:46_

> If anyone else has bright ideas about how to fix the Windows issue, I'm all ears.

`safe_filter_path` fixes the regex issues (mostly by copy/paste from `crates/uv/tests/common/mod.rs`) but there's still some issues, both with a lack of replacement in some cases, and a difference in output on Windows v.s. Linux that I can't easily resolve without a local Windows dev box.

---

_Marked ready for review by @palfrey on 2024-05-05 22:47_

---

_Comment by @palfrey on 2024-05-08 14:54_

> I will own reviewing now.

How's that going? I'm seeing a new fun set of conflicts, but didn't want to resolve those until the next bit of reviewing was done ideally.

---

_Comment by @charliermarsh on 2024-05-08 14:56_

Don't worry about resolving any conflicts for now. Worst case I will help resolve them since it's on me to review.

---

_Comment by @charliermarsh on 2024-05-08 14:56_

I will add this to my list for today.

---

_@charliermarsh reviewed on 2024-05-08 20:13_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/lenient_requirement.rs`:176 on 2024-05-08 20:13_

Do you mind if I remove all these impls for `LenientVersionSpecifiers`, `LenientRequirement`, etc.? My read is that the path is always set to `None` as-is.

---

_Review comment by @charliermarsh on `crates/pypi-types/src/lenient_requirement.rs`:176 on 2024-05-08 20:13_

(I did it but as a standalone commit, can revert if we decide against.)

---

_@charliermarsh reviewed on 2024-05-08 20:13_

---

_@charliermarsh reviewed on 2024-05-08 20:21_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/tracked_from_str.rs`:4 on 2024-05-08 20:21_

I want to experiment with a slightly different approach here. Instead of a trait, could we just always use `None` for the source, and then add a method to set the source _after_ parsing? Since the parsing itself doesn't depend on the source at all, right?

---

_@charliermarsh reviewed on 2024-05-08 20:21_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/tracked_from_str.rs`:4 on 2024-05-08 20:21_

(I can give it a try.)

---

_Review comment by @charliermarsh on `crates/distribution-types/src/annotation.rs`:7 on 2024-05-08 20:56_

I renamed this because we have some other "source" concepts and I wanted to disambiguate it.

---

_@charliermarsh reviewed on 2024-05-08 20:56_

---

_@charliermarsh reviewed on 2024-05-08 20:56_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/annotation.rs`:13 on 2024-05-08 20:56_

I made these all `PathBuf`, so that we convert at the very end (when we display) rather than eagerly.

---

_@charliermarsh reviewed on 2024-05-08 20:57_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/annotation.rs`:11 on 2024-05-08 20:57_

This is now optional.

---

_@charliermarsh reviewed on 2024-05-08 21:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:548 on 2024-05-08 21:10_

I changed this to use `BTreeSet` inside so that we don't need to call `.sorted` on the string representations. Instead, the values will already be sorted based on the sort order of `SourceAnnotation`.

---

_@charliermarsh approved on 2024-05-08 21:10_

---

_Comment by @charliermarsh on 2024-05-08 21:12_

@palfrey - I made a few changes and left comments for each one to explain the motivation. This LGTM but I'll hold off on merging to give you time to chime in, agree / disagree, etc.

---

_@palfrey reviewed on 2024-05-08 21:28_

---

_Review comment by @palfrey on `crates/pypi-types/src/lenient_requirement.rs`:176 on 2024-05-08 21:28_

I added all of those in earlier on when I was just trying to make sure the source data didn't get lost at any point so added it everywhere. I think you're right, and if the tests pass with them removed, all good.

---

_@palfrey reviewed on 2024-05-08 21:31_

---

_Review comment by @palfrey on `crates/distribution-types/src/annotation.rs`:13 on 2024-05-08 21:31_

Ok. I'm fine with that, but I'm not sure how needed that is, as AFAIK at this point their only use is as something we'll print, so they could be eagerly converted. This works though.

---

_Comment by @palfrey on 2024-05-08 21:40_

> @palfrey - I made a few changes and left comments for each one to explain the motivation. This LGTM but I'll hold off on merging to give you time to chime in, agree / disagree, etc.

Happy with the changes so far. I'm seeing one snapshot fix on ubuntu/macos, but then the windows snapshots appear to be missing `[TEMP_DIR]` bits vs. what the other platforms results were? Also some fun like the extra `tqdm` req on only windows in `tool_uv_sources`!

---

_Comment by @charliermarsh on 2024-05-08 21:46_

Interesting, I can take a look at those... I switched to `.user_display()` for `Path` which attempts to relativize it, if it's in the current directory.

---

_Comment by @charliermarsh on 2024-05-08 21:49_

I have a Windows machine so I can debug on that.

---

_Comment by @palfrey on 2024-05-08 21:57_

BTW, it doesn't seem to add sources for editables e.g. the `poetry_editable = { path = "../poetry_editable", editable = true }`  in `tool_uv_sources` just comes out as `-e ../poetry_editable` with no source. ~~This might well be like the `setup.py` scenario I listed in the issues in the description, and may well be effectively unsolvable (or at least worth pushing further down the line post this PRs merge)~~

Nope :) Got a local fix, pushing shortly...

---

_@palfrey reviewed on 2024-05-08 22:40_

---

_Review comment by @palfrey on `crates/pep440-rs/src/tracked_from_str.rs`:4 on 2024-05-08 22:40_

Sure. If you can make it work, all good!

---

_Comment by @palfrey on 2024-05-08 22:43_

eb3ea9856ddfceb2f41c4f775a5e5d9bef6a1b5c fixes editables. I needed to change the `sources` key to `String` to cope with both package names and editables urls, which I'm a little uncertain about.

Just going to fix that clippy issue, then get some sleep 💤. Thanks for the review and work here!

---

_Merged by @charliermarsh on 2024-05-09 03:19_

---

_Closed by @charliermarsh on 2024-05-09 03:19_

---

_Branch deleted on 2024-05-09 07:15_

---

_Comment by @palfrey on 2024-05-09 11:32_

🥳 

---

_Comment by @zanieb on 2024-05-09 13:35_

Thanks for the contribution!

---

```yaml
number: 15272
title: "fix: refresh activation scripts from upstream virtualenv"
type: pull_request
state: merged
author: 2bndy5
labels:
  - bug
assignees: []
merged: true
base: main
head: activations-scripts-from-upstream-virtualenv
created_at: 2025-08-14T08:53:59Z
updated_at: 2025-09-05T21:19:18Z
url: https://github.com/astral-sh/uv/pull/15272
synced_at: 2026-01-10T06:44:33Z
```

# fix: refresh activation scripts from upstream virtualenv

---

_Pull request opened by @2bndy5 on 2025-08-14 08:53_

## Summary
This refreshes the venv activation scripts from upstream `virtualenv` project.
This was largely triggered by a problem in the activate.nu script (for nushell):
- #14888 
- #14914 
- #14917 

I was careful to respect the git history going back to astral-sh/uv#3376 (the last time this was done).
Actually I looked at the complete history from back when this `uv-virtualenv` crate was named after a Pokémon (⁉️), but I found nothing (about activation scripts) from back then that hasn't been overwritten since.

### Some post-processing was involved

- Retained license info at top of scripts
- Retained template vars (eg `{{ BIN_PATH }}`) to assure current support toward relocatable venv
- Retained deviation from upstream in astral-sh/uv#5640. This seems to be the only deviation that isn't in sync with upstream.

### Notable changes from upstream

- (omitted due to undesirable complexity) pypa/virtualenv#2928 and its follow-up pypa/virtualenv#2940
- pypa/virtualenv#2910 (what prompted astral-sh/uv#14917 from astral-sh/uv#14888)

## Test Plan

There was a request in #14917 to add unit tests to detect breakage or errors.
I have added a CI job that runs the nushell activation script.
But I think it is better to have the CI test all/most supported shells.
See also #15294 

I have tested this locally using

- [x] nushell (v0.106.1)
- [x] cmd.exe (Microsoft Windows [Version 10.0.26100.4946])
- [x] bash in WSL (GNU bash, version 5.1.16(1)-release (x86_64-pc-linux-gnu))
- [x] pwsh (PSVersion 5.1.26100.4768)

---

_@zanieb reviewed on 2025-08-14 12:07_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1014 on 2025-08-14 12:07_

We try to avoid third-party actions, is there a simple way to install this manually? This isn't a critical area of CI, so it's probably okay if not, but worth checking.

---

_@2bndy5 reviewed on 2025-08-14 12:15_

---

_Review comment by @2bndy5 on `.github/workflows/ci.yml`:1014 on 2025-08-14 12:15_

The best alternative would be to download the release straight from nushell's GitHub release assets, which is exactly what this action does (I've inspected the source myself). At that point it would be like implementing `cargo-binstall`.

Since this is only testing on `x86_64-unknown-linux-gnu` target, I could instead do a simple `curl` with the nushell version interpolated with the URL.

I was also wondering if you'd rather test against latest nushell instead of pinning to v0.106.1?

---

_@zanieb reviewed on 2025-08-14 12:24_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:1014 on 2025-08-14 12:24_

Yeah I would just curl unless there are complications there.

I guess in an ideal world we'd test against a pinned version and it'd be bumped by Renovate? It seems like a bit of a pain though. I'm okay with trying latest — that's the easiest way to get updates about breakage even if it can be a little disruptive.

---

_Comment by @zanieb on 2025-08-14 12:36_

Since you mentioned it in the issue, we _could_ do another testing strategy within our test suite, e.g., we'd add a new `test-nushell` feature then assume nushell is available and add a snapshot test for the activation script that's gated by the feature. Then, we'd install nushell in the Linux cargo test run. I don't really have a strong preference for now, but that does make the test more robust. Given you have this CI setup working, it's probably best to just open an issue to track afterwards.

---

_Label `bug` added by @zanieb on 2025-08-14 13:13_

---

_Assigned to @zanieb by @zanieb on 2025-08-14 13:13_

---

_Comment by @2bndy5 on 2025-08-14 21:50_

I would actually leverage `cargo-nextest`'s DSL and profiles to filter tests instead of conditionally compiling certain tests.

**No one** should be running `cargo test` on this project. It didn't take me long to stumble across calls to ~`env::remove_var()`~ or `env::set_var()` - both of which are unsound in async contexts (eg. parallel running tests) and require `unsafe` blocks in rust 2024 edition.

---

_Comment by @charliermarsh on 2025-08-14 21:57_

Are you referring to `uv`? Which calls are you referring to? I don't see any calls to `env::remove_var`, and we only have a single call to `env::set_var`.


---

_Comment by @2bndy5 on 2025-08-14 22:54_

My bad, I mis-read some function calls in `uv-virtualenv::virtualenv`. Since you mentioned it, I did a search on the code base: No calls to `env::remove_var()`, but there are currently 3 calls to `env::set_var()` (1 in `uv::self::main()` and 2 in `uv_trampoline::bounce::make_child_cmdline()`), all of which are wrapped in `unsafe` blocks. Regardless, use of `cargo-nextest` ensures safety obligations are met about these invocations.

---

_@2bndy5 reviewed on 2025-08-14 23:56_

---

_Review comment by @2bndy5 on `.github/workflows/ci.yml`:1014 on 2025-08-14 23:56_

I ended up using gh-cli (present on every github-hosted runner) to get the latest nushell tag and download the x86_64-unknown-linux-gnu release asset. FYI, I added the `github.token` to the step's env to avoid hitting the GitHub REST API rate limit.

Personally, I'm not a fan of bash and all the quirks of `curl`. This is why I use nushell as my daily driver (though not as a login shell).

---

_Comment by @zanieb on 2025-08-15 00:58_

The trampoline is a distinct binary — it's never run in the same process as a test. Similarly, most of uv's tests are integration tests that spawn a subprocess and, for the ones that don't, we're just setting a variable to the path of the uv binary, which is always safe when running concurrent instances of the same uv binary.

---

_Comment by @2bndy5 on 2025-08-15 01:18_

@zanieb What are the requirements of the requested test(s)? I added a nushell activation test. Do you want more tests for all other supported shell(s)? I'm of the impression that nushell needed a test to detect breakages because nushell is still in v0.x. I have only ever used bash and pwsh (with much reluctance).

---

_Comment by @zanieb on 2025-08-15 01:27_

It'd be ideal to have test coverage for them all, but I don't think it's necessary to do here! Especially if you're reluctant :) We can open an issue to track it.

---

_Marked ready for review by @2bndy5 on 2025-08-15 01:46_

---

_Comment by @2bndy5 on 2025-08-15 01:47_

I opened #15294 to summarize the effort towards properly testing venv activation scripts

---

_Comment by @2bndy5 on 2025-08-15 02:49_

oops! Looks like further tweaks are needed for activate.ps1 as pwsh errors with:
```pwsh
__TCL_LIBRARY__ : The term '__TCL_LIBRARY__' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the 
name, or if a path was included, verify that the path is correct and try again.
At C:\dev\uv\.venv\Scripts\activate.ps1:86 char:5
+ if (__TCL_LIBRARY__ -ne "") {
+     ~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (__TCL_LIBRARY__:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

__TK_LIBRARY__ : The term '__TK_LIBRARY__' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, 
or if a path was included, verify that the path is correct and try again.
At C:\dev\uv\.venv\Scripts\activate.ps1:93 char:5
+ if (__TK_LIBRARY__ -ne "") {
+     ~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (__TK_LIBRARY__:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

---

_Review comment by @2bndy5 on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 08:07_

This relates to https://github.com/pypa/virtualenv/pull/2928. See [upstream's approach](https://github.com/pypa/virtualenv/blob/dad9369e97f5aef7e33777b18dcdb51b1fdac7bd/src/virtualenv/discovery/py_info.py#L140-L190) which uses TCL interpreter inside the python runtime to get adequate paths/values for these variables.

Since loading the TCL interpreter is not an option here, there's an unaddressed complexity here:
- the TCL lib location varies based on distribution (unix vs windows vs standalone built by astral-sh)
- `uv_pypi_types::scheme::Scheme` has no field to identify the location of the TCL library (if existent)

Instead, I just hard-coded these values to be blank strings. This preserves previous behavior and still keeps the activation scripts in sync with upstream.

I have to admit, I was not previously aware of the dynamic nature of these activation scripts when I took on this task.

---

_@2bndy5 reviewed on 2025-08-15 08:07_

---

_@zanieb reviewed on 2025-08-15 14:21_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 14:21_

Terrifying... will look into that.

---

_@zanieb reviewed on 2025-08-15 15:48_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 15:48_

I think we might just want to back these changes out and consider them separately. TCL / TK are notoriously complicated and cause a lot of problems so I don't want to unset any user-provided variables.

cc @geofft for when you're back

---

_@geofft reviewed on 2025-08-15 16:38_

---

_Review comment by @geofft on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 16:38_

Not looking super closely but I suspect I agree, we should not be setting `TCL_LIBRARY` or `TK_LIBRARY`. If this is being copied from upstream virtualenv than I am a little skeptical that that logic is right and would like to look at the PR.

For a system Python, the locations hard-coded in libtcl and libtk should work. For python-build-standalone, there is logic to make setting them unnecessary. So in both cases setting them would at most introduce some risk of making things go wrong.

Also an incredibly drive-by comment that I can expand on more next week, sorry - I'm not even sure we should be trying to sync with virtualenv as opposed to stdlib venv.

---

_@2bndy5 reviewed on 2025-08-15 20:34_

---

_Review comment by @2bndy5 on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 20:34_

> I'm not even sure we should be trying to sync with virtualenv as opposed to stdlib venv.

I'm under the impression that stdlib venv is an minimal fork of virtualenv. I appreciate syncing with virtualenv because it supports nushell, where stdlib venv does not.

If we decide to roll back the TCL/TK related changes from upstream, then it would certainly cause further deviation from upstream. Such deviations should be marked with comments (or a section in the crate's README), so the next contributor wouldn't have to dig through the git histories of both uv-virtualenv and virtualenv. It would be nicer to have .patch files that apply to upstream scripts, but that isn't very feasible.

---

_@zanieb reviewed on 2025-08-15 20:42_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 20:42_

> I'm under the impression that stdlib venv is an minimal fork of virtualenv. I appreciate syncing with virtualenv because it supports nushell, where stdlib venv does not.

Yeah I think this is fine for now. @geofft let's consider anything there separately from this contribution.

> If we decide to roll back the TCL/TK related changes from upstream, then it would certainly cause further deviation from upstream. 

I think we need to roll them back. I don't mind leaving a comment about it.

---

_@2bndy5 reviewed on 2025-08-15 21:58_

---

_Review comment by @2bndy5 on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-15 21:58_

I'll roll back the TCL/TK stuff. Upon further thought, I think a README section might be better than uv-specific comments in each script:
- comment syntax is different for each script (some of which I'm unfamiliar with)
- uv-specific comments in scripts can be easily overwritten with uninformed copy-n-paste; and easily overlooked if the git diff of index vs worktree is not reviewed before committing.
- A README can point to specific parts of the source (in uv-virtualenv and upstream virtualenv) where deviation shall be maintained; referring to the placeholder identities (ie `{{ BIN_PATH }}` vs `__BIN_PATH__`) used in the (template) activation scripts.

---

_@2bndy5 reviewed on 2025-08-16 01:03_

---

_Review comment by @2bndy5 on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-16 01:03_

I rolled back TCL/TK changes and documented major deviations in README.

FWIW, I have spent way more time on this patch than I initially thought I would. Even if the README section is not kept up to date, it still would help prevent future contributors having to research the entire git histories of both projects (wrt activation scripts).

---

_@zanieb reviewed on 2025-08-16 12:29_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:474 on 2025-08-16 12:29_

I appreciate that you're taking the time to improve the situation for the future! It often ends up being that way with tasks here, since we're widely used and generally adverse to breakage :)

---

_@zanieb approved on 2025-09-05 21:12_

Thank you so much!

---

_Merged by @zanieb on 2025-09-05 21:12_

---

_Closed by @zanieb on 2025-09-05 21:12_

---

_Branch deleted on 2025-09-05 21:19_

---

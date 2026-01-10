```yaml
number: 12049
title: "Respect `--project` during virtual environment discovery"
type: pull_request
state: closed
author: thejchap
labels: []
assignees: []
base: main
head: bug/python-find-project
created_at: 2025-03-07T15:48:23Z
updated_at: 2025-08-04T11:39:28Z
url: https://github.com/astral-sh/uv/pull/12049
synced_at: 2026-01-10T06:44:32Z
```

# Respect `--project` during virtual environment discovery

---

_Pull request opened by @thejchap on 2025-03-07 15:48_

## Summary
Resolves #11990

- Updates a couple discovery-related functions to accept a directory argument, used as the root directory when searching for a virtual environment
- Updates `uv python find` to pass the `--project` flag through to start the search for a python executable from that project, regardless of current directory

## Test Plan
- Snapshot tests
- E2E (below)

### Before
```sh
$ cargo run python find --directory ~/src --project example
/Users/justinchapman/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
```

### After
```sh
$ cargo run python find --directory ~/src --project example
/Users/justinchapman/src/example/.venv/bin/python3
```

---

_Renamed from "respect --project during uv python find (#11990)" to "[WIP] Respect --project during `uv python find` (#11990)" by @thejchap on 2025-03-07 15:48_

---

_Renamed from "[WIP] Respect --project during `uv python find` (#11990)" to "Respect --project during `uv python find` (#11990)" by @thejchap on 2025-03-13 04:39_

---

_Marked ready for review by @thejchap on 2025-03-13 04:39_

---

_Review requested from @zanieb by @konstin on 2025-03-14 20:42_

---

_Assigned to @zanieb by @zanieb on 2025-03-18 23:00_

---

_Comment by @zanieb on 2025-03-26 19:44_

(Sorry for the delay, I'm traveling a lot this month)

---

_Comment by @thejchap on 2025-04-04 06:45_

@zanieb anything i can do to help on this one?

---

_@zanieb reviewed on 2025-04-04 13:46_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:537 on 2025-04-04 13:46_

Why is this unix only?

---

_@zanieb reviewed on 2025-04-04 13:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:537 on 2025-04-04 13:47_

If it's for the snapshot path, you can do

```
    context
        .with_filtered_python_names()
        .with_filtered_virtualenv_bin()
        .with_filtered_exe_suffix();
```

---

_@zanieb reviewed on 2025-04-04 13:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:636 on 2025-04-04 13:49_

You can use `.assert().success();` instead to avoid an empty snapshot.

We should test `python find --project` _before_ creating the virtual environment. In that case, we should ensure we're respecting the `requires-python` range from the child `pyproject.toml`.

Then, we should test with a virtual environment.

Then, we should also test we respect the `--project` root for `.python-version` file discovery too.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-04 13:53_

Should these be updated to respect `--project` / `--directory` too? Is there a good reason to do that separately?

---

_@zanieb reviewed on 2025-04-04 13:53_

---

_@thejchap reviewed on 2025-04-08 23:51_

---

_Review comment by @thejchap on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-08 23:51_

~@zanieb was wondering the same thing (in [this comment](https://github.com/astral-sh/uv/issues/11990#issuecomment-2719956384) :) - just started to feel like blast radius was snowballing a bit - i'll dig a little more~ i misread this and got it conflated with something else

---

_Review comment by @thejchap on `crates/uv/tests/it/python_find.rs`:636 on 2025-04-09 02:44_

@zanieb i think i covered all the test cases - lmk if i misinterpreted something. also was getting a diff between windows and unix on [this assertion](https://github.com/astral-sh/uv/actions/runs/14347400247/job/40219696086?pr=12049#step:10:2515) which i mitigated with `.with_filtered_python_sources()` as i don't think that particular diff matters for this test - but lmk if thats incorrect

<img width="874" alt="Screenshot 2025-04-08 at 22 41 10" src="https://github.com/user-attachments/assets/41568fde-27c1-4166-aa61-72a4f98df7bf" />

---

_@thejchap reviewed on 2025-04-09 02:44_

---

_@thejchap reviewed on 2025-04-16 03:16_

---

_Review comment by @thejchap on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-16 03:16_

@zanieb i am thinking it might be good to refactor that particular callsite separately (if for no other reason than to prevent this pr from getting too unwieldy) - i believe the only caller of `PythonInstallation::find_best` is `pip_compile` ([pointer](https://github.com/thejchap/uv/blob/bug/python-find-project/crates/uv/src/commands/pip/compile.rs#L267-L267)) - i can add a separate issue for that if it would be helpful

---

_@zanieb reviewed on 2025-04-16 15:29_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:96 on 2025-04-16 15:29_

I'm surprised this uses `current_dir()` still, `find_or_download` is used in a lot of places.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-16 15:32_

I think https://github.com/astral-sh/uv/pull/12049/files#r2047193355 is more concerning to me

I do worry that if we change the behavior everywhere but `pip compile` that would be really confusing to users? I don't think we can merge this incrementally.

It might be worth writing up the difference here between:

- Respect `--project` during `uv python find`

and the more broad change:

- Use `--project` and `--directory` as the root for virtual environment discovery

Is this pull request trying to do one or the other? I'm not sure it makes sense to do the first without the second. That we don't do the second one seems like a bug.

I do understand the concern about the increase in scope. I could probably take it over if you prefer? If we do the latter change we probably want to land it in 0.7.0 (#12843)

---

_@zanieb reviewed on 2025-04-16 15:32_

---

_@thejchap reviewed on 2025-04-17 04:04_

---

_Review comment by @thejchap on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-17 04:04_

@zanieb this is helpful thanks - just to make sure im following the potential problem with changing behavior incrementally, it would look something like:

- user runs `uv python find --project sub-project` to debug which python is being used to run a script in a project, it tells them `sub-project/.venv/bin/python3`
- user runs `uv run my-script --project sub-project`, and it actually gets run via another interpreter, like `~/.local/share/uv/python/cpython-3.13.0`

also, with the `--directory` flag, is it a known issue that venv discovery isn't happening correctly already with that? it looks to me like that just [changes the working directory](https://github.com/thejchap/uv/blob/bug/python-find-project/crates/uv/src/lib.rs#L62) - so even though right now discovery is just looking at cwd that should work already? or are you saying that this, combined with the `--project` flag not getting propagated through to discovery results in weird behavior because `--project` isn't overriding `--directory` when it should be

i'll think a little on scope change if i think i'll have bandwidth to finish out by Tues

---

_@thejchap reviewed on 2025-04-19 04:45_

---

_Review comment by @thejchap on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-19 04:45_

@zanieb `find_or_download` now takes a path argument and callsites have been updated to pass `--project` through where relevant

don't think ill have any more bandwidth before tuesday (ie to add tests) - but if this doesn't make it into `0.7.0` i'm happy to continue working on it later next week and the following

---

_Review comment by @thejchap on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-25 13:53_

going to get back to this in the next day or two and will add some tests

---

_@thejchap reviewed on 2025-04-25 13:53_

---

_@zanieb reviewed on 2025-04-25 14:15_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1192 on 2025-04-25 14:15_

> it looks to me like that just [changes the working directory](https://github.com/thejchap/uv/blob/bug/python-find-project/crates/uv/src/lib.rs#L62) - so even though right now discovery is just looking at cwd that should work already?

That makes sense to me — I would believe that's already working as expected.

> user runs uv run my-script --project sub-project, and it actually gets run via another interpreter, like ~/.local/share/uv/python/cpython-3.13.0

Yeah, this is what I'm worried about. `uv python find --project ...` should return the same interpreter that would be used in `uv run --project ...`.

---

_Comment by @thejchap on 2025-05-11 18:30_

@zanieb sorry for the delay on this one - decided to work on `ty` a bit

what would you recommend as a reliable way to test which python interpreter was used during `pip compile`? i looked in a couple test suites and wasn't able to find any obvious examples - unless i'm thinking about testing wrong here

what i did initially was set the test `.pip_compile()` to verbose via `.arg("-v")` then checked for a debug message, but that seems like it could be brittle - also appears the test context filters don't get applied to debug messages so was also running into issues with the windows tests

any code pointers/examples/advice here would be helpful, thanks!

---

_Comment by @zanieb on 2025-05-17 01:54_

> sorry for the delay on this one - decided to work on ty a bit

No problem! I've been slow to reply anyway :)

> what would you recommend as a reliable way to test which python interpreter was used during pip compile? i looked in a couple test suites and wasn't able to find any obvious examples - unless i'm thinking about testing wrong here

I would do like

```
anyio==3.0.0; python_version >= "3.12"
anyio==4.0.0; python_version < "3.12"
```

Then the test should toggle the compiled version

```
❯ echo 'anyio==3.0.0; python_version >= "3.12"\nanyio==4.0.0; python_version < "3.12"' | uv pip compile - -p 3.12
Resolved 3 packages in 5ms
# This file was autogenerated by uv via the following command:
#    uv pip compile - -p 3.12
anyio==3.0.0
idna==3.10
    # via anyio
sniffio==1.3.1
    # via anyio
❯ echo 'anyio==3.0.0; python_version >= "3.12"\nanyio==4.0.0; python_version < "3.12"' | uv pip compile - -p 3.11
Resolved 3 packages in 6ms
# This file was autogenerated by uv via the following command:
#    uv pip compile - -p 3.11
anyio==4.0.0
idna==3.10
    # via anyio
sniffio==1.3.1
    # via anyio
```

---

_Comment by @zanieb on 2025-05-17 01:54_

(I'd maybe swap so the "newer" anyio matches the "newer" Python)

---

_Renamed from "Respect --project during `uv python find` (#11990)" to "Respect `--project` during virtual environment discovery" by @zanieb on 2025-05-27 17:11_

---

_@zanieb reviewed on 2025-05-27 17:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:278 on 2025-05-27 17:16_

Why not use `project_dir` here?

---

_@zanieb reviewed on 2025-05-27 17:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:238 on 2025-05-27 17:17_

We should probably move this so it comes before the printer. Stylistically, that's always been the last argument? I don't know how much it matters.

---

_@zanieb reviewed on 2025-05-27 17:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:128 on 2025-05-27 17:17_

This command does support the `--project` flag, I guess we should respect it? I'm not sure it matters here but it could bite someone later?

---

_Comment by @thejchap on 2025-05-27 20:19_

@zanieb still working my way through all the commands- sorry should have moved this back to draft

---

_Converted to draft by @thejchap on 2025-05-27 20:25_

---

_Comment by @codspeed-hq[bot] on 2025-05-31 15:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/thejchap%3Abug%2Fpython-find-project)

### Merging #12049 will **improve performances by 15.79%**

<sub>Comparing <code>thejchap:bug/python-find-project</code> (5e5207c) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_Comment by @thejchap on 2025-05-31 15:35_

@zanieb i should probably hand this one off - i don't think the scope increase is a good match for my availability anymore (each time i've picked it up i've spend most of the time i had available resolving merge conflicts :)

i've been going through command by command and passing `project_dir` through and adding tests, if i had to guess i'd say i'm probably halfway through?

one thing i noticed that i probably would have done next is updated [TestContext::with_filtered_virtualenv_bin](https://github.com/thejchap/uv/blob/5e5207c09fb1a71c7481a08af137fddf1e3ba41d/crates/uv/tests/it/common/mod.rs#L220) to also work for filtering out venvs in a different project directory (or added a new helper) - didn't yet have a nice way to test this

---

_Marked ready for review by @thejchap on 2025-05-31 15:35_

---

_Comment by @zanieb on 2025-06-02 15:59_

Thanks for the update!

---

_Comment by @thejchap on 2025-06-14 21:12_

@zanieb was thinking about this a little more - could it make sense to pass project dir to the remaining few commands and merge this, then add an issue to continue improving test coverage when passing in an explicit project arg?

Test coverage is the main thing that's been slow going because of the # of commands the change is touching, but seems like there could still be some value in the additional coverage and the behavior change as it is currently?

---

_Closed by @thejchap on 2025-08-04 11:39_

---

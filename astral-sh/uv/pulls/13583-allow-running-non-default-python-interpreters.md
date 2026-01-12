```yaml
number: 13583
title: allow running non-default Python interpreters directly via uvx
type: pull_request
state: merged
author: oconnor663
labels:
  - enhancement
assignees: []
merged: true
base: main
head: jack/uvx_pypy
created_at: 2025-05-21T20:15:56Z
updated_at: 2025-05-30T16:12:41Z
url: https://github.com/astral-sh/uv/pull/13583
synced_at: 2026-01-12T16:10:45Z
```

# allow running non-default Python interpreters directly via uvx

---

_@oconnor663_

Previously if you wanted to run e.g. PyPy via `uvx`, you had to spell it like `uvx -p pypy python`. Now we reuse the `PythonRequest::parse` machinery to handle the executable, so all of the following examples work:

- `uvx python`
- `uvx python3.8`
- ~~`uvx 38`~~
- `uvx pypy`
- `uvx pypy@3.8`
- `uvx graalpy`
- `uvx graalpy@3.8`

The `python` (and on Windows only, `pythonw`) special cases are retained, which normally aren't allowed values of `-p`/`--python`.

Closes https://github.com/astral-sh/uv/issues/13536.

---

The initial version of this PR has 3 failing snapshot tests. The common issue is that this `@` syntax used to work:

```
$ uvx python@3.8
Python 3.8.20 (default, Oct  2 2024, 16:34:12) 
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

But now it does this:

```
$ uvx python@3.8
× No solution found when resolving tool dependencies:
  ╰─▶ Because python was not found in the package registry and you require python==3.8, we can conclude that your requirements are unsatisfiable.
```

The underlying reason for the change is that we [used to parse the executable with `Target::parse`](https://github.com/astral-sh/uv/blob/38884da9b902e6dcd553beb4b79aeef7d066c8d2/crates/uv/src/commands/tool/mod.rs#L36), but in this PR [we parse the executable with `PythonRequest::parse`](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L49). The latter doesn't recognize `python@3.8` at all, and it returns the catch-all `PythonRequest::ExecutableName` variant, which [we take to mean that the executable is a package and not an interpreter](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L71).

There are several different ways we could handle this, and I'm not sure what's best. Reviewer feedback needed :)

- ~~We could break back compat and stop supporting `uvx python@3.8`. Note that either way, `uvx python3.8`, `uvx python38`, and even `uvx 38` all work under this PR. (The last one there might be even more flexible than we want? That's another potential reviewer question.) I'm not sure how widely the current syntax is used, but I'd guess it's more widely used than `uvx -p python3.8 python`?~~
- We could expand [the special case in `ToolRequest::parse`](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L42-L46) (previously `is_python`) to support `python@3.8` in addition to bare `python`. (**Update:** We ended up expanding `ToolRequest::parse` and refactoring some of the `PythonRequest::parse` machinery to minimize duplication.)
- ~~We could expand `PythonRequest::parse` to support `python@3.8`, possibly by adding `"python"` to `ImplementationName::long_names` as an aslias for `"cpython"`. This would allow e.g. `uvx --python python@3.8 ruff`, which isn't currently allowed in `main` or in this PR, and which might not be desirable?~~

---

_Review requested from @Gankra by @oconnor663 on 2025-05-21 20:15_

---

_Review requested from @zanieb by @oconnor663 on 2025-05-21 20:15_

---

_Comment by @Gankra on 2025-05-21 20:49_

> We could expand [the special case in ToolRequest::parse](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L42-L46) (previously is_python) to support python@3.8 in addition to bare python.

After some checking this *feels* like the best option of the 3 you laid out. I don't want to break compat without a really good reason, and PythonRequest::parse is called pervasively throughout our CLI, so changing that would have widespread ramifications.

---

_Review comment by @Gankra on `crates/uv/src/commands/tool/mod.rs`:72 on 2025-05-21 20:50_

extremely small nit: ` => {}` is vaguely more idiomatic

---

_@Gankra reviewed on 2025-05-21 20:53_

The implementation seems reasonable. In an ideal world we could fail one of the parsers and simply fallback to the other to preserve the old behaviour, but I guess the parsers are too flexible for that.

---

_Comment by @zanieb on 2025-05-21 21:23_

> > We could expand [the special case in ToolRequest::parse](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L42-L46) (previously is_python) to support python@3.8 in addition to bare python.
> 
> After some checking this _feels_ like the best option of the 3 you laid out. I don't want to break compat without a really good reason, and PythonRequest::parse is called pervasively throughout our CLI, so changing that would have widespread ramifications.

I actually just suggested that we want consistent Python parsing throughout and `python` is a reasonable alias for `cpython`, so `uv run -p python@3.8` seems okay and I think the last option is what I'd lean towards unless there are other tool-specific special cases we need to implement anyway.

---

_@zanieb reviewed on 2025-05-21 21:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:52 on 2025-05-21 21:26_

I don't think a bare version (e.g., 312 or 3.12) should work, but I do think `python3` and `python3.12` should work.

---

_Comment by @oconnor663 on 2025-05-21 21:26_

> > We could expand [the special case in ToolRequest::parse](https://github.com/astral-sh/uv/blob/7850a6ef14c188e9bc78d778c4a3c9b441f9e6a6/crates/uv/src/commands/tool/mod.rs#L42-L46) (previously is_python) to support python@3.8 in addition to bare python.
> 
> After some checking this _feels_ like the best option of the 3 you laid out. I don't want to break compat without a really good reason, and PythonRequest::parse is called pervasively throughout our CLI, so changing that would have widespread ramifications.

I chatted with @zanieb just now, and they were leaning towards the third option of allowing e.g. `--python python@3.8` everywhere (maybe by adding it to `long_names` and calling it an alias for `"cpython"`), which would fix my broken tests as a side effect. At the same time, `uvx 38` seems like too much to allow, because we'd be implicitly shadowing a large class of possible package names. I'm going to noodle on whether I can factor out what we want from `PythonRequest::parse`, and/or whether we might want something like `uv run`'s  `--script` flag that callers with "interesting" internal package names can use to prevent shadowing. (The natural choice might be `--package` but alas that's already taken.)

---

_@zanieb reviewed on 2025-05-21 21:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:52 on 2025-05-21 21:28_

This suggests that maybe `PythonRequest` isn't quite right since it drops that information? You could add variants like `Interpreter` (i.e., bare `python`) and `InterpreterVersion` (i.e., `python3.13`) (names questionable), but that's a fair amount of work since you'd need to handle them everywhere.

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:61 on 2025-05-21 21:29_

I don't think you need to say this last part, this is sort of implied by the choice to enumerate the variants and we frequently use this pattern here.

---

_@zanieb reviewed on 2025-05-21 21:29_

---

_@zanieb reviewed on 2025-05-21 21:33_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:90 on 2025-05-21 21:33_

I'm loosely opposed to changing this, because we use this argument name all over the place. We do use `python_request` in some places? which you could use since you'll just shadow it later? I don't have strong feelings. It seems like this does improve readability here, but may be confusing if you're coming from elsewhere.

---

_@zanieb reviewed on 2025-05-21 21:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:700 on 2025-05-21 21:35_

Nit: Generally we don't shorten variable names, I'd just use `tool_request` or `tool_python_request`.

---

_Comment by @zanieb on 2025-05-21 21:43_

On

> whether we might want something like uv run's --script flag that callers with "interesting" internal package names can use to prevent shadowing.

I mostly brought that up to explain our general thinking and existing patterns for that problem. I probably wouldn't pursue adding that here.

---

_Label `enhancement` added by @zanieb on 2025-05-21 21:44_

---

_Assigned to @zanieb by @zanieb on 2025-05-21 21:44_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-05-22 17:01_

---

_Unassigned @zanieb by @oconnor663 on 2025-05-22 17:01_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1435 on 2025-05-22 22:17_

Drive by: this is what was intended right?

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1591 on 2025-05-22 22:21_

As written this allows `--python @3.8`. Do we want to disallow that?

---

_Comment by @oconnor663 on 2025-05-22 22:45_

Second pass: I've factored out an alternate constructor for `PythonRequest` called `try_parse_tool_executable`. This shares parsing logic for cases that are (now) common between `--python` and `uvx` (`python3.8`, `python@3.8`, `pypy38`) but allows for cases that are unique to `--python` (`38`, `cp@38`) or to `uvx` (`python`, `pythonw` on Windows). If the differences get any more complicated than this, it might be better to give up the sharing and just copy-paste the parser, but as-is it doesn't feel too bad. The `match` statement where I was seeing which variant of `PythonRequest` we got is thankfully gone.

There is one remaining failing snapshot test: `tool_run_python_from`

https://github.com/astral-sh/uv/blob/30be27beb1c93147d00a819b70fc729673bbe1af/crates/uv/tests/it/tool_run.rs#L1956-L2004

This tests cases like `uvx --from python@3.8 python`. That's similar to `uvx --python python3.8 python` (which is still supported and now allows the `@` syntax too), but it actually ignores the executable argument entirely. For example with the currently released version:

```
$ uvx --from python@38 blargblargblarg
Python 3.8.20 (default, Oct  2 2024, 16:34:12) 
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

I'm not sure what to do with this. We could keep supporting it, but it seems kind of bizarre. Does anyone use this? (If it was important enough to be in the snapshot tests I fear the answer is yes.) Feedback needed.

**Update:** Zanie clarified that `--from python` is important for use cases like `uvx --from python bash`, which runs `bash` from the `PATH` with Python available. I added support below.

---

_@oconnor663 reviewed on 2025-05-22 22:45_

---

_Assigned to @zanieb by @oconnor663 on 2025-05-22 22:48_

---

_Unassigned @oconnor663 by @oconnor663 on 2025-05-22 22:48_

---

_Comment by @Gankra on 2025-05-23 12:21_

> I'm not sure what to do with this. We could keep supporting it, but it seems kind of bizarre. Does anyone use this? (If it was important enough to be in the snapshot tests I fear the answer is yes.) Feedback needed.

This was added in https://github.com/astral-sh/uv/pull/11076 but the PR/issue focuses on non-from usage. That said, zanie added explicit snapshots for it, and one error for "hey what are you doing", so pretty clear they were going eyes wide-open into it.

The fact that the second argument gets ignored seems like a bug though... at very least we should probably error if it's not `python`?

---

_@Gankra reviewed on 2025-05-23 12:23_

---

_Review comment by @Gankra on `crates/uv-python/src/discovery.rs`:1591 on 2025-05-23 12:23_

Geez we have so many random sugars, I feel loathe to add another one.
(If we decide to allow this we definitely need to snapshot test it... actually maybe add a snapshot test regardless)

---

_@oconnor663 reviewed on 2025-05-24 06:56_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1591 on 2025-05-24 06:56_

forbidden (and tested) in 448534ee50ecfe26151303180e8fe1c98a2d697c

---

_@zanieb reviewed on 2025-05-27 16:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:1946 on 2025-05-27 16:37_

```suggestion
    // Bare versions don't work either. Again we interpret them as package names.
```
?

---

_@zanieb reviewed on 2025-05-27 16:39_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1485 on 2025-05-27 16:39_

The newline is necessary in some markdown renderers, let's include it for consistency
```suggestion
    ///
    /// - This only supports long names, including e.g. `pypy39` but **not** `pp39` or `39`.
```

---

_@zanieb reviewed on 2025-05-27 16:41_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1549 on 2025-05-27 16:41_

Should use `///`

Can you also add a top-level sentence on what the method does?

---

_@zanieb reviewed on 2025-05-27 16:41_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1507 on 2025-05-27 16:41_

Same as https://github.com/astral-sh/uv/pull/13583/files#r2109677726

---

_@zanieb reviewed on 2025-05-27 16:45_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1488 on 2025-05-27 16:45_

Maybe `try_from_executable_name`?

---

_@zanieb reviewed on 2025-05-27 16:46_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1395 on 2025-05-27 16:46_

Note this name changed, and should use `[...]` for a proper link.

---

_@oconnor663 reviewed on 2025-05-27 23:02_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1488 on 2025-05-27 23:02_

I'm also just about to push a commit that adds back `uvx --from python[...]` support, and this function gets used in that case too, where its argument isn't really an "executable" or a "command". I'm going to go with `try_from_tool_name`. Let me know what you think.

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:1409 on 2025-05-28 09:27_

nit: We don't block-align comments, can you move those to the line above?

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:2462 on 2025-05-28 10:25_

no need to go down to bytes, we can handle this on the char-level:
```suggestion
        if s.chars().all(char::is_alphabetic) {
```

---

_Review comment by @konstin on `crates/uv/tests/it/tool_run.rs`:1943 on 2025-05-28 10:29_

Does this snapshot break when someone publishes `cp311` to PyPI?

---

_Review comment by @konstin on `crates/uv/src/commands/tool/mod.rs`:48 on 2025-05-28 10:32_

Does uvx behave differently with `pythonw`, as uvx is already a terminal command? 

---

_@konstin reviewed on 2025-05-28 10:32_

Just some drive-by comments

---

_@zanieb reviewed on 2025-05-28 13:16_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1488 on 2025-05-28 13:16_

That seems sensible.

---

_@zanieb reviewed on 2025-05-28 13:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:1943 on 2025-05-28 13:17_

Yes, but we should squat or advocate for those names to be banned.

---

_@oconnor663 reviewed on 2025-05-28 15:07_

---

_Review comment by @oconnor663 on `crates/uv/src/commands/tool/mod.rs`:48 on 2025-05-28 15:07_

I think both in `main` and in this PR we parse `pythonw` as an abstract `PythonRequest::Version`, so we probably never shell out to `pythonw.exe` at all. But I'm not sure that's what you're asking?

---

_@zanieb reviewed on 2025-05-28 15:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/mod.rs`:48 on 2025-05-28 15:18_

`pythonw` exists to avoid launching a terminal window on Windows, whereas, `python` always creates one in the background. 

@konstin it probably won't, but `uvw tool run pythonw` will be a thing after #11786 

---

_@oconnor663 reviewed on 2025-05-28 15:19_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/tool_run.rs`:1943 on 2025-05-28 15:19_

This is my first time using snapshot testing, so a broad philosophy question: How do we feel about "brittleness" in snapshots? Is it just as bad as brittleness in regular tests, or is it more acceptable?

---

_@oconnor663 reviewed on 2025-05-28 15:21_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1409 on 2025-05-28 15:21_

3b9358e402e6de305920b470a427e7d99776fd3a

---

_@zanieb reviewed on 2025-05-28 19:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:1943 on 2025-05-28 19:25_

It's just as bad, really. We try to make them reproducible wherever possible. That's mostly w.r.t. the external world though, internal changes causing snapshot changes are fine.

However, I think this specific case is fine.

---

_@zanieb reviewed on 2025-05-28 19:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2048 on 2025-05-28 19:26_

This seems like a regression, and would be confusing since we do support, e.g., `ruff@latest`.

---

_@zanieb reviewed on 2025-05-28 19:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2113 on 2025-05-28 19:28_

Separate, but we should open an issue to follow-up on this. `uvx -p >=3.12 python` is valid and probably a better recommendation? Also I'm not sure why we wouldn't support `python>=3.12` here.

---

_@zanieb approved on 2025-05-28 19:28_

(Presuming this regression is fixed https://github.com/astral-sh/uv/pull/13583/files#r2112604209)

---

_Review comment by @oconnor663 on `crates/uv/tests/it/tool_run.rs`:2113 on 2025-05-29 03:40_

I'm not sure exactly how, but I'd gotten the impression that we didn't want to support this. If we do, I think I can just remove this check:

https://github.com/astral-sh/uv/blob/3b9358e402e6de305920b470a427e7d99776fd3a/crates/uv/src/commands/tool/run.rs#L763-L771

---

_@oconnor663 reviewed on 2025-05-29 03:40_

---

_@oconnor663 reviewed on 2025-05-29 03:54_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/tool_run.rs`:2048 on 2025-05-29 03:54_

Could you say more here? To me it looks like something that was an error before is still an error, with a different message. `PythonRequest` doesn't currently have a `Latest` variant, so that would need to be implemented if we wanted to support it in this PR.

---

_@oconnor663 reviewed on 2025-05-29 03:55_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/tool_run.rs`:2113 on 2025-05-29 03:55_

5ac5190b049e222733d1f1993e054518faec9a1c

---

_@zanieb reviewed on 2025-05-29 12:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2048 on 2025-05-29 12:15_

The change in message here is a regression because it moves from a clear error message to a misleading one. Previously, we explained that `latest` was not supported for `python` requests. Here, we say `latest` is an invalid version request, but... that's not true in the general case because we support it for non-python requests, like `ruff@latest`.

---

_@oconnor663 reviewed on 2025-05-29 15:14_

---

_Review comment by @oconnor663 on `crates/uv/tests/it/tool_run.rs`:2048 on 2025-05-29 15:14_

753329f9146b0f5dbf28dccceca33659e42d5049

---

_Comment by @oconnor663 on 2025-05-29 15:19_

Also, @Gankra @konstin any more comments?

---

_Assigned to @Gankra by @oconnor663 on 2025-05-29 15:19_

---

_Assigned to @konstin by @oconnor663 on 2025-05-29 15:19_

---

_Unassigned @zanieb by @oconnor663 on 2025-05-29 15:19_

---

_Comment by @konstin on 2025-05-30 08:16_

I'll leave the design review to zanie, code LGTM.

---

_Merged by @oconnor663 on 2025-05-30 16:12_

---

_Closed by @oconnor663 on 2025-05-30 16:12_

---

_Branch deleted on 2025-05-30 16:12_

---

```yaml
number: 3266
title: Rewrite Python interpreter discovery
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/interp-request-ii
created_at: 2024-04-25T18:01:13Z
updated_at: 2024-05-21T19:37:25Z
url: https://github.com/astral-sh/uv/pull/3266
synced_at: 2026-01-12T16:05:32Z
```

# Rewrite Python interpreter discovery

---

_@zanieb_

Updates our Python interpreter discovery to conform to the rules described in #2386, please see that issue for a full description of the behavior. Briefly, we now will search for interpreters that satisfy a requested version without stopping at the first Python executable. Additionally, if retrieving information about an interpreter fails we will continue to search for a working interpreter. We also add the plumbing necessary to request Python implementations other than CPython, though we do not add support for other implementations at this time.

A major internal goal of this work is to prepare for user-facing managed toolchains i.e. fetching a requested version during `uv run`. These APIs are not introduced, but there is some managed toolchain handling as required for our test suite.

Some noteworthy implementation changes:

- The `uv_interpreter::find_python` module has been removed in favor of a `uv_interpreter::discovery` module. 
- There are new types to help structure interpreter requests and track sources
- Executable discovery is implemented as a big lazy iterator and is a central authority for source precedence
- `uv_interpreter::Error` variants were split into scoped types in each module
- There's much more unit test coverage, but not for Windows yet

Remaining work:

- [x] Write new test cases
- [x] Determine correct behavior around executables in the current directory
- _Future_: Combine `PythonVersion` and `VersionRequest`
- _Future_: Consider splitting `ManagedToolchain` into local and remote variants
- _Future_: Add Windows unit test coverage
- _Future_: Explore behavior around implementation precedence (i.e. CPython over PyPy)

Refactors split into:

- #3329 
- #3330 
- #3331
- #3332

Closes #2386
 

---

_Comment by @codspeed-hq[bot] on 2024-04-30 17:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/interp-request-ii)

### Merging #3266 will **not alter performance**

<sub>Comparing <code>zb/interp-request-ii</code> (1b212ab) with <code>main</code> (dfd6ccf)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:37 on 2024-05-02 08:35_

Can you add "in PATH" to the docstring? This got me confused initially

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:192 on 2024-05-02 08:40_

Error handling with results in iterators is ugly, probably needs a `impl Iterator<Item = Result<(InterpreterSource, PathBuf)>, _> + 'a` return type or something (see e.g. https://doc.rust-lang.org/std/fs/struct.ReadDir.html#impl-Iterator-for-ReadDir or https://docs.rs/walkdir/latest/walkdir/struct.WalkDir.html#impl-IntoIterator-for-WalkDir)

---

_@konstin reviewed on 2024-05-02 09:00_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:37 on 2024-05-02 12:39_

Yes, but I intentionally removed it previously because the request should not need to know _how_ we find the interpreter to fulfill the given executable name.

---

_@zanieb reviewed on 2024-05-02 12:39_

---

_@zanieb reviewed on 2024-05-02 12:40_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:192 on 2024-05-02 12:40_

Yeah I've been dreading making this have error handling. I'm kind of wondering if we want it at all? Like should we consider just logging errors and continue searching for an interpreter we can use?

---

_@zanieb reviewed on 2024-05-02 17:43_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:192 on 2024-05-02 17:43_

Done!

---

_Review comment by @konstin on `crates/uv-interpreter/src/managed/find.rs`:96 on 2024-05-07 09:22_

nit: Can you add a comment about sorting, esp. about how we order cpython-3.8 < cpython-3.9 < cpython-3.10?



---

_Review comment by @konstin on `crates/uv-interpreter/src/managed/find.rs`:70 on 2024-05-07 09:27_

I'd merge these into a single invalid toolchain name error that we return

---

_Review comment by @konstin on `crates/uv-interpreter/src/managed/find.rs`:21 on 2024-05-07 09:32_

I'd return `Toolchain` from `toolchain_directories`

---

_Review comment by @konstin on `crates/uv-interpreter/src/lib.rs`:10 on 2024-05-07 09:33_

The crate doc comment above needs updating

---

_Review comment by @konstin on `crates/uv/src/commands/pip_compile.rs`:183 on 2024-05-07 09:36_

It's asymmetric that `find_interpreter` gets the sources passed, while `find_best_interpreter` gets the system preference passed.

---

_Review comment by @konstin on `crates/uv/src/commands/pip_check.rs`:32 on 2024-05-07 09:37_

This is a much better abstraction

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:115 on 2024-05-07 09:39_

This is type is `pub`, but not importable (it's not reexported in lib.rs)

---

_Review comment by @konstin on `crates/uv/tests/pip_compile_scenarios.rs`:37 on 2024-05-07 09:41_

```suggestion
.env("UV_TEST_PYTHON_PATH", &python_path)
```

The function takes two `AsRef<OsStr>`, it clones internally anyway

---

_Review comment by @konstin on `crates/uv-interpreter/src/managed/find.rs`:19 on 2024-05-07 09:44_

I think this can be `pub(crate)`

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-07 09:49_

These aliases feel odd, i think i'd go for another variant in the `Error` enum and matching in downstream code instead

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:182 on 2024-05-07 09:54_

nit: Inline iter and dedent one

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:191 on 2024-05-07 10:02_

Really missing `yield` in rust here, chaining different iterators with results is so much nesting.

An alternative to chaining is boxing, not sure if it's better:

```rust
    let mut iter: Box<dyn Iterator<Item = _>> = Box::new(iter::empty());
    // (1) The active environment
    if sources.contains(InterpreterSource::ActiveEnvironment) {
        let active_venv = virtualenv_from_env()
            .into_iter()
            .map(virtualenv_python_executable)
            .map(|path| Ok((InterpreterSource::ActiveEnvironment, path)));
        iter = Box::new(iter.chain(active_venv));
    }
    // (2) A discovered environment
    if sources.contains(InterpreterSource::DiscoveredEnvironment) {
        let discovered_venv = virtualenv_from_working_dir()
            .map(|path| {
                path.map(virtualenv_python_executable)
                    .map(|path| (InterpreterSource::DiscoveredEnvironment, path))
                    .into_iter()
            })
            .map_err(Error::from);
        iter = Box::new(iter.chain(iter::once(discovered_venv).flatten_ok()));
    }
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/discovery.rs`:312 on 2024-05-07 10:03_

```suggestion
/// Lazily iterate over all discoverable Python interpreters.
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/managed/find.rs`:13 on 2024-05-07 10:04_

Do the `expect`s in `TOOLCHAIN_DIRECTORY` below hold up for non-test code? 

---

_Review comment by @konstin on `crates/uv-interpreter/src/lib.rs`:20 on 2024-05-07 10:07_

This feels like it could be private (with fetch-python moving a bit?)

---

_@konstin approved on 2024-05-07 10:10_

Left some nitpicky rust comments

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-07 14:38_

I really want to separate "There was an error during discovery" from "We did not find an interpreter". They mean very different things and I think callers should be encouraged to handle them separately. I could remove the aliases or change it to be a custom enum instead of a result — I explored both of these options previously though and found this the clearest.

---

_@zanieb reviewed on 2024-05-07 14:38_

---

_@zanieb reviewed on 2024-05-07 14:42_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/lib.rs`:20 on 2024-05-07 14:42_

Do you mind if I tackle this separately? I'm trying not to touch the toolchain management stuff here and expect to revisit it when we add real support for it.

---

_@konstin reviewed on 2024-05-07 14:43_

---

_Review comment by @konstin on `crates/uv-interpreter/src/lib.rs`:20 on 2024-05-07 14:43_

Yeah that's a follow-up PR

---

_Review comment by @zanieb on `crates/uv-interpreter/src/managed/find.rs`:13 on 2024-05-07 14:43_

I'll double check. It's still not intended to be used in production though i.e. it's powered by `UV_BOOTSTRAP_DIR`

---

_@zanieb reviewed on 2024-05-07 14:43_

---

_@zanieb reviewed on 2024-05-07 14:44_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile_scenarios.rs`:37 on 2024-05-07 14:44_

Ah thanks this was just added during debugging and I missed reverting it.

---

_@zanieb reviewed on 2024-05-08 13:00_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/managed/find.rs`:21 on 2024-05-08 13:00_

I don't know if it will be costly to construct `Toolchain` in the future, going to wait until I work on that code more.

---

_@charliermarsh reviewed on 2024-05-08 19:20_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 19:20_

What are the aliases doing exactly? Am I correct that `Ok(FoundInterpreter(result))` could be replaced with `Ok(InterpreterResult::Ok(result))`? So it's just syntactic sugar?

---

_@zanieb reviewed on 2024-05-08 19:25_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 19:25_

Yep just sugar to clarify the meaning, I'm okay with the latter (and had it earlier) but chose the current sugar because I think "Found" is clearer than "Ok". I especially don't like `Ok(Ok(...))` and `Ok(Err(...))`

---

_@zanieb reviewed on 2024-05-08 19:27_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 19:27_

I tried to create a more explicit `InterpreterResult::Found` alias but you can't do that since `Result` is external.

---

_@charliermarsh reviewed on 2024-05-08 19:27_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 19:27_

I would personally find `Ok(Ok(...))` and `Ok(Err(...))` to be clearer, since I don't have to guess at the meaning or look back at this definition.

---

_@zanieb reviewed on 2024-05-08 19:35_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 19:35_

Either way it's trade-offs at where you have to look when, but two votes is enough I'll just remove the aliases.

---

_@charliermarsh reviewed on 2024-05-08 20:07_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:77 on 2024-05-08 20:07_

I think it's fine in principle, I just haven't seen it before so I'm finding the unfamiliarity of it to be not worth it (but this is highly personal, others may disagree, maybe it is a common pattern outside of our crates).

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/lib.rs`:19 on 2024-05-13 00:32_

Nit: `Python` should be capitalized here

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:70 on 2024-05-13 00:33_

Today, doesn't `--today` _ignore_ (e.g.) discovering a `.venv` in the current directory? `Preferred` makes it sound like that's still allowed as a fallback. Is it a behavior change?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:67 on 2024-05-13 00:33_

Nit: `vritual`

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:319 on 2024-05-13 00:35_

Nit: needless `let`, can we just return `python_executables` directly?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:809 on 2024-05-13 00:37_

So we _do_ allow (e.g.) `--python foo` where `foo` is an arbitrary executable in `PATH`? I think the post said we wouldn't do this: https://github.com/astral-sh/uv/issues/2386#issuecomment-1992133003. Although I think we _should_ support it and don't agree with the linked comment.

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:269 on 2024-05-13 00:39_

Nit: Do you need to collect here? You immediately `into_iter`, can you just continue the chain?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:451 on 2024-05-13 00:40_

Confirming the logic here -- we take the first interpreter that matches the query, right? So e.g. if the user requested `3.9`, and the first `3.9` we saw was `3.9.0`, we'd accept that (even if we might have `3.9.12` later in `PATH`) -- is that correct? (IMO that is the correct behavior but confirming.)

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:70 on 2024-05-13 00:42_

Looking at the code, I think it _does_ ignore the `.venv` in the current directory. So maybe this should be called `Required`...?

---

_@charliermarsh reviewed on 2024-05-13 00:42_

---

_@charliermarsh reviewed on 2024-05-13 00:44_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:64 on 2024-05-13 00:44_

Can you make these rustdocs? (I.e., use triple slashes.)

---

_@charliermarsh reviewed on 2024-05-13 00:44_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:91 on 2024-05-13 00:44_

Nit: `requesed`

---

_@charliermarsh reviewed on 2024-05-13 00:45_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 00:45_

Nit: this lifetime can be removed ("elided")

---

_@charliermarsh reviewed on 2024-05-13 00:47_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:894 on 2024-05-13 00:47_

I think I would find `from_sources` or similar to be more idiomatic.

---

_@charliermarsh reviewed on 2024-05-13 00:51_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:402 on 2024-05-13 00:51_

I'd personally find these a bit easier to read as a `match`, I think.

---

_@charliermarsh reviewed on 2024-05-13 00:52_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:395 on 2024-05-13 00:52_

What's the motivation for `try_exists` over `exists`? Just wondering.

---

_@charliermarsh reviewed on 2024-05-13 00:56_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:268 on 2024-05-13 00:56_

It seems like `which_in_global` does accept multiple directories, but I'm guessing this gives us slightly different ordering / preference resolution?

---

_@charliermarsh reviewed on 2024-05-13 00:58_

This generally looks good to me -- I don't have any major structural feedback. I'm mostly curious as to how we're going to test this and gain confidence in its correctness prior to merging and shipping out to users, since it's such a substantial and user-facing refactor.

---

_@charliermarsh reviewed on 2024-05-13 01:00_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:800 on 2024-05-13 01:00_

Should we not check if `value_as_path.is_file()`? What if you provide `foo` and `foo` is in your current working directory? IMO that should resolve to the local executable, but would it as-written?


---

_@charliermarsh reviewed on 2024-05-13 01:01_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:775 on 2024-05-13 01:01_

Honestly I think the path checks should be first. If you have `python3` in the current working directory, I'd expect `./python3` and `python3` to work the same (as they would if you ran them directly from the shell).

---

_@zanieb reviewed on 2024-05-13 14:29_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:70 on 2024-05-13 14:29_

Yeah you are correct

---

_@zanieb reviewed on 2024-05-13 14:31_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:809 on 2024-05-13 14:31_

Yeah I forgot I said we shouldn't do this in that latter comment. I think it's okay? I'll consider this a bit more.. it seems nice not to have a breaking change

---

_@zanieb reviewed on 2024-05-13 14:33_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:269 on 2024-05-13 14:33_

```
error[E0515]: cannot return value referencing local variable `search_path`
   --> crates/uv-interpreter/src/discovery.rs:268:5
    |
268 |       env::split_paths(&search_path)
    |       ^                ------------ `search_path` is borrowed here
    |  _____|
    | |
269 | |         .into_iter()
270 | |         .filter(|dir| dir.is_dir())
271 | |         .flat_map(move |dir| {
...   |
302 | |                 )
303 | |         })
    | |__________^ returns a value referencing data owned by the current function
    |
    = help: use `.collect()` to allocate the iterator
```

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:395 on 2024-05-13 14:36_

Just because the Rust docs suggest that `exists` is error prone and `try_exists` is better to use. I think if we encounter a permissions error on a parent directory it'll fail instead of saying `false`.

---

_@zanieb reviewed on 2024-05-13 14:36_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:268 on 2024-05-13 14:37_

Yeah we need to check each directory separately since we are checking for multiple names per directory, whereas `which_in_global` would resolve a single name.

---

_@zanieb reviewed on 2024-05-13 14:37_

---

_@zanieb reviewed on 2024-05-13 14:39_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 14:39_

With an unnamed lifetime

```
error[E0106]: missing lifetime specifier
   --> crates/uv-interpreter/src/discovery.rs:177:74
    |
175 |     version: Option<&VersionRequest>,
    |              -----------------------
176 |     sources: &SourceSelector,
    |              ---------------
177 | ) -> impl Iterator<Item = Result<(InterpreterSource, PathBuf), Error>> + '_ {
    |                                                                          ^^ expected named lifetime parameter
    |
    = help: this function's return type contains a borrowed value, but the signature does not say whether it is borrowed from `version` or `sources`
help: consider introducing a named lifetime parameter
    |
174 ~ fn python_executables<'a>(
175 ~     version: Option<&'a VersionRequest>,
176 ~     sources: &'a SourceSelector,
177 ~ ) -> impl Iterator<Item = Result<(InterpreterSource, PathBuf), Error>> + 'a {
```

Without a lifetime

```

error[E0700]: hidden type for `impl std::iter::Iterator<Item = std::result::Result<(discovery::InterpreterSource, std::path::PathBuf), discovery::Error>>` captures lifetime that does not appear in bounds

help: to declare that `impl std::iter::Iterator<Item = std::result::Result<(discovery::InterpreterSource, std::path::PathBuf), discovery::Error>>` captures `'_`, you can introduce a named lifetime parameter `'a`
    |
174 ~ fn python_executables<'a>(
175 ~     version: Option<&'a VersionRequest>,
176 ~     sources: &'a SourceSelector,
177 ~ ) -> impl Iterator<Item = Result<(InterpreterSource, PathBuf), Error>> + 'a  {
```

---

_@zanieb reviewed on 2024-05-13 14:40_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:451 on 2024-05-13 14:40_

Yep!

---

_@charliermarsh reviewed on 2024-05-13 14:46_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 14:46_

You can omit it from `sources` specifically (is what I was trying to say).

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 14:46_

Like `sources: &SourceSelector` while leaving the rest of the signature intact. It's not a necessary constraint. (At least, it wasn't when I tried, I could be wrong or misremembering now.)

---

_@charliermarsh reviewed on 2024-05-13 14:47_

---

_@zanieb reviewed on 2024-05-13 14:47_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 14:47_

Oh okay thanks for clarifying!

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:180 on 2024-05-13 14:47_

(and you are correct)

---

_@zanieb reviewed on 2024-05-13 14:48_

---

_@zanieb reviewed on 2024-05-14 03:36_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:775 on 2024-05-14 03:36_

I don't think that's how shells usually work though:

```
❯ touch foo
❯ chmod +x foo
❯ foo
zsh: command not found: foo
❯ ./foo
❯ bash
bash-5.2$ foo
bash: foo: command not found
bash-5.2$ ./foo
```


---

_@zanieb reviewed on 2024-05-14 17:31_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:800 on 2024-05-14 17:31_

I think it should not actually, per the comment at https://github.com/astral-sh/uv/pull/3266#discussion_r1599331047

---

_Comment by @zanieb on 2024-05-20 16:17_

Example test setup

```
gh pr checkout 3266
cargo build
alias uv=$(pwd)/target/debug/uv
uv version
```

I'll do some local QA and share helpful commands here

---

_Comment by @charliermarsh on 2024-05-20 20:26_

Not sure if intentional, but I have `pypy` (which is PyPy at Python 2.7) and `pypy39` (which is PyPy at Python 3.9) in path, and `cargo run venv --python pypy` fails because it tries to use the one called `pypy`:

```
puffin on  zb/interp-request-ii [$] is 📦 v0.1.44 via 🐍 v3.11.8 via 🦀 v1.78.0 took 2s
❯ cargo run venv --python pypy
   Compiling uv v0.1.44 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.75s
     Running `target/debug/uv venv --python pypy`
  × Can't use Python at `/opt/homebrew/bin/pypy`
  ╰─▶ Python executable does not support `-I` flag. Please use Python 3.8 or newer.

puffin on  zb/interp-request-ii [$] is 📦 v0.1.44 via 🐍 v3.11.8 via 🦀 v1.78.0 took 2s
❯ cargo run venv --python pypy3.9
   Compiling uv v0.1.44 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.02s
     Running `target/debug/uv venv --python pypy3.9`
Using Python 3.9.18 interpreter at: /opt/homebrew/bin/pypy3.9
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

Should it resolve to the `pypy3.9`?

`cargo run venv --python pypy3.9` does work as expected.


---

_Comment by @charliermarsh on 2024-05-20 20:30_

Otherwise, my own smoke-testing works as expected.

---

_Comment by @charliermarsh on 2024-05-20 20:32_

I would personally expect this to work -- I copied a Python to `foo` locally, and `--python ./foo` discovers it but `--python foo` does not:

```
puffin on  zb/interp-request-ii [$?]
❯ cargo run venv --python foo
   Compiling uv v0.1.44 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.87s
     Running `target/debug/uv venv --python foo`
  × Requested Python executable `foo` not found in PATH

puffin on  zb/interp-request-ii [$?]
❯ cargo run venv --python ./foo
   Compiling uv v0.1.44 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.72s
     Running `target/debug/uv venv --python ./foo`
Using Python 3.9.18 interpreter at: foo
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

I think we fixed this in the prior implementation and that it was considered a bug.


---

_@charliermarsh reviewed on 2024-05-20 22:56_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:800 on 2024-05-20 22:56_

At the very least, doesn't this need to respect both kinds of separators, per https://github.com/astral-sh/uv/issues/3062 ?

---

_@zanieb reviewed on 2024-05-20 22:59_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:800 on 2024-05-20 22:59_

Ah interesting, yeah I guess we need to support Unix separators on Windows

---

_Comment by @zanieb on 2024-05-20 23:55_

I think your `pypy` example fails because we don't have support for detecting that as an implementation name yet (that will happen in a follow up pull request). Here, `cpython` is the only implementation name variant (just to reduce scope).

---

_@zanieb reviewed on 2024-05-21 14:54_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:775 on 2024-05-21 14:54_

I changed this behavior to match Charlie's expectation.

---

_@zanieb reviewed on 2024-05-21 14:54_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:800 on 2024-05-21 14:54_

I fixed this.

---

_Marked ready for review by @zanieb on 2024-05-21 16:32_

---

_Merged by @zanieb on 2024-05-21 19:37_

---

_Closed by @zanieb on 2024-05-21 19:37_

---

_Branch deleted on 2024-05-21 19:37_

---

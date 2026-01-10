```yaml
number: 15068
title: "Use `.rcdata` to store trampoline type + path to python binary"
type: pull_request
state: merged
author: paveldikov
labels:
  - no-build
assignees: []
merged: true
base: main
head: trampoline
created_at: 2025-08-04T22:42:08Z
updated_at: 2025-11-09T18:48:21Z
url: https://github.com/astral-sh/uv/pull/15068
synced_at: 2026-01-10T06:28:12Z
```

# Use `.rcdata` to store trampoline type + path to python binary

---

_Pull request opened by @paveldikov on 2025-08-04 22:42_

`.rsrc` is the idiomatic way of storing metadata and non-code resources in PE
binaries. This should make the resulting binaries more robust as they are no longer
dependent on the exact location of a certain magic number.

Addresses: #15022

## Test Plan

Existing integration test for `uv-trampoline-builder` + addition to ensure robustness
to code signing.

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:162 on 2025-08-04 22:50_

remark: Sloppy though it may be, I am quite pleased with the overall simplicity of this. LoC for `bounce.rs` went down (590 -> 488) and there appears to be no significant change in compiled file size.

---

_@paveldikov reviewed on 2025-08-04 22:50_

---

_Review comment by @T-256 on `crates/uv-trampoline-builder/src/lib.rs`:529 on 2025-08-04 23:02_

we have `cfg(windows)` already in top `mod tests` at line 374.

though I recommend extract this executable signing into new function, so it can be used in `console_script_launcher` too.

---

_Comment by @zanieb on 2025-08-04 23:11_

I'm not super qualified to review this idea, cc @samypr100 & @konstin 

---

_Review comment by @T-256 on `crates/uv-trampoline/src/bounce.rs`:55 on 2025-08-04 23:17_

nit:
```suggestion
            _ => None,
```

---

_Review comment by @T-256 on `crates/uv-trampoline/src/bounce.rs`:67 on 2025-08-04 23:19_

you could move `10` as const at module's top and document that it comes from `object::pe`.

---

_Review comment by @T-256 on `crates/uv-trampoline/Cargo.toml`:51 on 2025-08-04 23:30_

if it's intentional to not use `dunce::canonicalize` then we can drop it from deps.

---

_@T-256 reviewed on 2025-08-04 23:45_

LGTM, it's not necessary to done before merging, but can you confirm that it can bring back python3.7 support? (i.e. `uvx -p 3.7 ruff` most work)

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:40 on 2025-08-05 01:26_

```suggestion
const RESOURCE_TRAMPOLINE_KIND: &str = "UV_TRAMPOLINE_KIND\0";
const RESOURCE_PYTHON_PATH: &str  = "UV_TRAMPOLINE_PYTHON_PATH\0";
```

nit: use a namespace for the resource ids

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:91 on 2025-08-05 01:28_

```suggestion
    let trampoline_kind = load_resource(RESOURCE_TRAMPOLINE_KIND)
        .and_then(|data| TrampolineKind::from_resource(&data))
        .unwrap_or_else(|| error_and_exit("Failed to load trampoline kind from resources"));
```

nit: type vs kind naming change, opt to keep it consistent

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:114 on 2025-08-05 01:32_

```suggestion
    match trampoline_kind {
```

nit: naming consistency

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:118 on 2025-08-05 01:36_

Was dunce removal intentional? iirc we expect path to always be absolute

---

_Label `no-build` added by @zanieb on 2025-08-05 01:54_

---

_Comment by @paveldikov on 2025-08-05 01:56_

Added an even sloppier (but working) refactor of `uv-trampoline-builder`. Needs oodles of polish and review.

It is now verifiably robust to code-signing, so that solves my original problem.

I have not verified the py3.7 case, though a similar integration test can be added probably very easily? @T-256

Also note that the Win32 APIs are all really rather `unsafe`. I'm not sure what the right thing to do here is tbh.

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:48 on 2025-08-05 02:07_

From a historical perspective, I believe the choice to append the script as an archive to the executable was taken from [distlib](https://github.com/pypa/distlib/tree/master/PC)

---

_@samypr100 reviewed on 2025-08-05 02:08_

---

_@samypr100 reviewed on 2025-08-05 02:09_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:529 on 2025-08-05 02:09_

agreed

---

_Comment by @samypr100 on 2025-08-05 02:18_

I don't see any immediate issues with using rcdata for the trampoline kind and runnable path, it's mainly a different way to accomplish the same thing. What I'm not very sure of is using it for the script contents. From a code signing perspective is the goal to sign the launcher regardless of what it may end up executing?

---

_@samypr100 reviewed on 2025-08-05 02:19_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:162 on 2025-08-05 02:19_

It is indeed nice, lots of spooky code removed 😄 although we may get it back in the trampoline builder

---

_@paveldikov reviewed on 2025-08-05 10:16_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:529 on 2025-08-05 10:16_

done

---

_@paveldikov reviewed on 2025-08-05 10:16_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:40 on 2025-08-05 10:16_

done

---

_@paveldikov reviewed on 2025-08-05 10:16_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:67 on 2025-08-05 10:16_

> done

---

_@paveldikov reviewed on 2025-08-05 10:17_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:91 on 2025-08-05 10:17_

done (reverted to 'kind')

---

_@paveldikov reviewed on 2025-08-05 10:17_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:114 on 2025-08-05 10:17_

done (reverted to 'kind')

---

_@paveldikov reviewed on 2025-08-05 10:18_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:48 on 2025-08-05 10:18_

Added a nugget on the `zipimport` mechanism in the README. (which already talks about the `distlib` DNA)

---

_@paveldikov reviewed on 2025-08-05 10:18_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:55 on 2025-08-05 10:18_

done

---

_Comment by @paveldikov on 2025-08-05 10:53_

> What I'm not very sure of is using it for the script contents. From a code signing perspective is the goal to sign the launcher regardless of what it may end up executing?

The goal is to be able to sign a (finished) entrypoint executable.

---

_@paveldikov reviewed on 2025-08-05 11:07_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:118 on 2025-08-05 11:07_

Restored dunce. Path may be relative if the trampoline is supposed to be relocatable.

---

_Comment by @paveldikov on 2025-08-05 11:49_

Getting there...
* `uv-trampoline` is pretty much ready for full review
    - [x] Still need to compile binaries for all platforms, not just x86-64 (is this something I need to do by hand? or there's a job somewhere that compiles 'blessed' copies of these?)
* `uv-trampoline-builder` is refactored, but still has a bunch of outstanding things (I might need help with some of them)
    - [x] Figure out the `TODO`'d code path with `is_gui` (smells wrong, but I couldn't figure out how the previous version avoided having to know this. Does it end up being a don't-care value? It's only used in one other place -- in `uv run`.)
    - [x] There's a dozen warnings that need sorted
    - [x] The integration test seems to be flaky on my personal PC. There's a 1 in 3 chance of success, ENOENT or failure to `try_from_path()`. Not sure why...
    - [x] For some reason the PowerShell snippet complains about missing `Cert:` drive when run on CI

If anyone is able to give me a hand on any of these, it would be amazing! If not I will try to keep chipping away over the next few days

---

_Marked ready for review by @paveldikov on 2025-08-05 11:49_

---

_Comment by @konstin on 2025-08-05 13:47_

>  Still need to compile binaries for all platforms, not just x86-64 (is this something I need to do by hand? or there's a job somewhere that compiles 'blessed' copies of these?)

You can build the binaries on your machine for testing and CI (there are cross-compile instructions in the readme), before merging the PR a maintainer will build the binaries on their machine and push them to this branch.

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:68 on 2025-08-06 00:11_

Note, for safety we should use the *W variants (e.g. LoadLibraryExW) as the uv binaries don't have a manifest that sets the activeCodePage to UTF-8 unlike the trampoline which does set an active code page.

---

_@samypr100 reviewed on 2025-08-06 00:11_

---

_@samypr100 reviewed on 2025-08-06 00:53_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 00:53_

I'm not sure gui script metadata is propagated in this code path currently. Seems like a gap (likely out of scope to fix in this PR), but would need to dig into the code to see if that's the case

---

_@zanieb reviewed on 2025-08-06 20:52_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 20:52_

I'm not sure either. Isn't the existing code just copying the existing launcher payload and therefore retaining the content without knowledge of what it is?

---

_@paveldikov reviewed on 2025-08-06 20:54_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:68 on 2025-08-06 20:54_

Done. There is some residual usage of the *A variants which I have not touched.

Also thank you for indirectly prompting me to dig further find out that the `PCSTR`/`PCWSTR` types require explicit null-termination in order to interoperate with Rust. I wasted a perfectly good day of my life trying to figure out why 50% of my test runs were failing with 'file not found' :-)

---

_@paveldikov reviewed on 2025-08-06 22:03_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 22:03_

It _says_ it's doing so but I can't for the life of me figure out how it's doing that. Could it be that it's actually using `Launcher.payload` to also include the PE binary and not just the zip'd segment? I have just re-added a `.script_data` field in the struct, but maybe I am inadvertently altering its meaning?

---

_@T-256 reviewed on 2025-08-06 22:05_

---

_Review comment by @T-256 on `crates/uv-trampoline-builder/Cargo.toml`:41 on 2025-08-06 22:05_

IIUC, these should be workspace dependencies.

---

_@zanieb reviewed on 2025-08-06 22:06_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 22:06_

yeah payload there is not just the zip'd content

---

_@zanieb reviewed on 2025-08-06 22:07_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 22:07_

I recently added that functionality in https://github.com/astral-sh/uv/pull/14790 where I needed to rewrite the Python interpreter path but not change anything else.

---

_Comment by @paveldikov on 2025-08-06 22:20_

Close...

- [X] Committed binaries for x86-64 and i686
- [x] ...I can't seem to get it to x-compile for aarch64 though, hence the failing CI run for aarch64.
- A bunch of `unsafe` Win32 API usage.
    - [x] Question to answer: I could try my hand at refactoring it to use the `winsafe` crate, but current precedent in the codebase seems to steer me away from that. What little Win32 API usage exists right now is `unsafe` too. Opinions welcome!
- [X] Code-signing int test is pretty solid now
- [X] Tests passing on Windows! (almost -- 3 test cases failed with what looks like an unrelated transient.)
- [x] Tests passing on non-Windows.
    - [x] fixed compile on non-Windows. Some serious `cfg` hole-poking which is not pretty, but it wasn't pretty to begin with, so it's consistent. Some fundamental limitations in cargo workspaces to contend with.
- [x] `write_to_file()` `is_gui` mystery solved

---

_@paveldikov reviewed on 2025-08-06 22:28_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 22:28_

Maybe I'll just add another flag as a PE resource in the binaries (ergo `Launcher.is_gui`), so that the resulting binary is clearly identifiable as a console/gui launcher. Then one can reconstruct a binary from pure metadata?

---

_@samypr100 reviewed on 2025-08-06 22:59_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 22:59_

If we end up adding a resource for that a more technically correct name would likely be `subsystem` with two possible values (`console`, or `windows`) although generally I'm not sure if it's needed as we'd be able to tell between python.exe vs pythonw.exe usage? I thought we checked for pythonw usage somewhere 😅 

---

_@samypr100 reviewed on 2025-08-06 23:04_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-06 23:04_

If the previous code just copied the entrypoints without regenerating the entire trampoline, I think it would still behave correctly

---

_Comment by @samypr100 on 2025-08-07 00:07_

> * I seem to have broken compile on non-Windows as the addition of Win32 API calls now makes `uv-trampoline-builder` not compile on non-Windows at all. I kinda don't know what the right way of going about this is.

A first step towards unblocking you may be adding `#[cfg(windows)]`, `if cfg!(windows)`, or `[target.'cfg(target_os = "windows")'.dependencies]` at places where there's references to trampoline builder.

---

_@samypr100 reviewed on 2025-08-07 00:23_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-07 00:23_

Sorry looking at the code in main now. From what I gather looking at `copy_entrypoint`, I think what's missing there is a `let is_gui = launcher.python_path.ends_with("pythonw.exe");` and maybe the signature of `launcher.write_to_file(&mut file)?;` can be updated to pass the is_gui flag or `python_executable.to_path_buf()` replacing python with pythonw when is_gui is true.

I think that could be easily fixed in a separate PR (to avoid increasing scope on this one)? Thoughts @zanieb ?

---

_@paveldikov reviewed on 2025-08-07 08:04_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/Cargo.toml`:41 on 2025-08-07 08:04_

Done

---

_@samypr100 reviewed on 2025-08-07 13:30_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-07 13:30_

Started a PR in https://github.com/astral-sh/uv/pull/15134

---

_Assigned to @zanieb by @konstin on 2025-08-07 14:29_

---

_@samypr100 reviewed on 2025-08-07 15:47_

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-07 15:47_

@paveldikov we should be able to fix this now

---

_@paveldikov reviewed on 2025-08-07 19:21_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:112 on 2025-08-07 19:21_

Thanks! I have rebased on top of yours & I have also decided to use the `is_gui` value so I can also use the proper base binary (console/window) when re-building the entrypoint.

---

_Comment by @paveldikov on 2025-08-07 19:34_

Ok I'm feeling pretty good about this now. A **big** thank-you in particular to @samypr100 for helping with this -- you could tell I was a wee bit out of my depth...

Outstanding:
- The aarch64 trampoline test is failing because the binary blob is not the correct one. I just can't get it to x-compile on my home PC setup, and attempts to remedy this by installing further SDKs have just maxed out my hard drive. _I guess I'll just wait for a maintainer to build a 'blessed' binary since that's part of the process anyway?_
- Unless asked otherwise, I assume there is little appetite to refactor Win32 API usage to `winsafe`, so leaving the `unsafe`ty as it is.

---

_Review comment by @T-256 on `crates/uv-trampoline-builder/Cargo.toml`:32 on 2025-08-07 21:36_

`workspace = true`?

---

_Review comment by @T-256 on `crates/uv-trampoline/README.md`:101 on 2025-08-07 21:38_

IMO, it's better to put a reference link to supported formats by `zipimport` (it seems that also accepts .exe files)

---

_Comment by @samypr100 on 2025-08-07 22:53_

> Unless asked otherwise, I assume there is little appetite to refactor Win32 API usage to winsafe, so leaving the unsafety as it is.

I believe so, iirc winsafe usage was removed a while ago in preference of normalizing towards a unified window crate

> The aarch64 trampoline test is failing because the binary blob is not the correct one. I just can't get it to x-compile on my home PC setup, and attempts to remedy this by installing further SDKs have just maxed out my hard drive. I guess I'll just wait for a maintainer to build a 'blessed' binary since that's part of the process anyway?

If you have windows, easiest is via WSL and cargo-xwin. I'd also be happy to add one for you as well but I don't have push access to your branch 😅 


---

_Review requested from @zanieb by @paveldikov on 2025-08-08 18:44_

---

_@T-256 approved on 2025-08-09 13:41_

It looks good now,
in replace of `dunce` crate, we can consider implement our own (small) function as said in [here](https://users.rust-lang.org/t/things-in-std-with-strong-alternatives/83998/10) :

> The ultimate reason is that DOS-style paths (like C:\path) in modern Windows are emulated on top of NT paths (e.g. \Device\HarddiskVolume3\path). This emulation is lossy because not all NT paths can be represented as DOS-style paths and DOS-style paths do lossy things like trim trailing spaces and dots (this made more slightly more sense in an old dos shell). In short, canonicalize needs to be able to return a correct path even if it's not representable as a normal DOS-style path. Removing path length limits (well up to i16::MAX u16s) is definitely a useful bonus though.
>
> So, yeah, canonical uses \\?\ as a shorthand for "this is an NT path pretending to be a DOS path, please don't do anything weird with it". This makes converting NT to DOS simpler and also makes it easy to turn it back into a proper NT path. (side note: the only change is that \\?\ is converted to \??\ which is a special NT directory full of symlinks like C: which points to whatever the real path for the C: drive is).

there is already Q&A in stackoverflow for this at [here](https://stackoverflow.com/questions/50322817/how-do-i-remove-the-prefix-from-a-canonical-windows-path), I tested below on Windows machine:

```rs
use std::path::{Component, Path, PathBuf, Prefix};

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let path = Path::new(r"\\?\C:\projects\3rdparty\rust");

    // Should remove the “\\?” prefix from the canonical path
    // in order to avoid CMD bailing with “UNC paths are not supported”.
    let head = path.components().next().ok_or("empty path?")?;
    let disk;
    let head = if let Component::Prefix(prefix) = head
        && let Prefix::VerbatimDisk(d) = prefix.kind() {
        disk = format!("{}:", d as char);
        Path::new(&disk).components().next().ok_or("empty path?")?
    } else {
        head
    };

    let path = std::iter::once(head)
        .chain(path.components().skip(1))
        .collect::<PathBuf>();

    println!("{:?}", head);  // Prefix(PrefixComponent { raw: "C:", parsed: Disk(67) })
    println!("{:?}", path);  // "C:\\projects\\3rdparty\\rust"

    Ok(())
}
```
and here is output on play.rust-lang.org (non-Windows):
```
Normal("\\\\?\\C:\\projects\\3rdparty\\rust")
"\\\\?\\C:\\projects\\3rdparty\\rust"
```

---

_Comment by @samypr100 on 2025-08-10 02:34_

> in replace of dunce crate, we can consider implement our own (small) function as said in [here](https://users.rust-lang.org/t/things-in-std-with-strong-alternatives/83998/10)

It's a good idea to consider. Although the scope of this PR is already big, so I'd propose we investigate this replacement as part of a different PR to ease on the reviewers. It's not clear to me immediately if there will be any substantial space/bloat savings.

---

_Review comment by @paveldikov on `crates/uv-trampoline/README.md`:101 on 2025-08-11 15:32_

It doesn't seem to be explicitly documented in such terms @ https://docs.python.org/3/library/zipimport.html, though.

If you read the src, however, you can make a fairly reasonable inference that:
* https://github.com/python/cpython/blob/3.13/Lib/zipimport.py#L372-L422: the nature of the algorithm suggests that zip archive content may be found at any arbitrary location inside the file being imported (as long as it is contiguous)
* https://github.com/python/cpython/blob/3.13/Lib/zipimport.py#L417-L419: `pex` and self-extracting PE binaries (presumably referring to `distlib` precedent) are use cases that are deemed important enough to support.

Not sure if this is reliable enough to merit encoding in the README, though.

---

_@paveldikov reviewed on 2025-08-11 15:32_

---

_@paveldikov reviewed on 2025-08-11 16:55_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/Cargo.toml`:32 on 2025-08-11 16:55_

done

---

_@paveldikov reviewed on 2025-08-11 17:07_

---

_Review comment by @paveldikov on `crates/uv/src/windows_exception.rs`:311 on 2025-08-11 17:07_

caused by harmonisation of `windows` versions, which in turn brought in: https://github.com/microsoft/windows-rs/pull/3452

---

_Review requested from @samypr100 by @paveldikov on 2025-08-11 17:22_

---

_Assigned to @Gankra by @zanieb on 2025-08-11 20:49_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/install.rs`:1045 on 2025-08-14 14:12_

nit: you can just do this check and return and then after do `let target = fs_err::...`

---

_Review comment by @Gankra on `crates/uv-trampoline-builder/src/lib.rs`:44 on 2025-08-14 14:17_

You can maybe use c-str literals for these: https://doc.rust-lang.org/edition-guide/rust-2021/c-string-literals.html

(Genuinely a maybe, not sure if it's useful given the APIs these funnel into)

---

_Review comment by @Gankra on `crates/uv-trampoline-builder/src/lib.rs`:102 on 2025-08-14 14:22_

should ideally have a SAFETY comment

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:69 on 2025-08-14 14:29_

Everything else in trampoline operates on utf8 and uses the `A` functions because we set a manifest declaring support for utf8 -- does that not work here?

---

_Review comment by @Gankra on `crates/uv-trampoline/src/bounce.rs`:38 on 2025-08-14 14:33_

Is enforcing this really no longer relevant?

---

_@Gankra approved on 2025-08-14 14:39_

Wow nice work! This looks pretty good to me?

---

_@zanieb reviewed on 2025-08-14 14:52_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:102 on 2025-08-14 14:52_

Yeah I think we should have a SAFETY comment for all unsafe invocations.

---

_@paveldikov reviewed on 2025-08-14 21:06_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:69 on 2025-08-14 21:06_

An earlier version used the `A` varieties but I replaced them with `W` as @samypr100 suggested those were safer. They all work but to be honest I don't really grok the kinds of edge cases where this stuff would really matter.

---

_@paveldikov reviewed on 2025-08-14 21:10_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:38 on 2025-08-14 21:10_

A similar, though not identical, constraint remains in `uv-trampoline-builder`. It appears impossible to even construct a binary with an outsize resource, so it seems moot to defend against such a condition at runtime.

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:69 on 2025-08-14 21:10_

uv-trampoline is safe to use A*, but not in uv; I do appreciate the W* consistency between both though.

---

_@samypr100 reviewed on 2025-08-14 21:10_

---

_@paveldikov reviewed on 2025-08-14 21:12_

---

_Review comment by @paveldikov on `crates/uv/src/commands/python/install.rs`:1045 on 2025-08-14 21:12_

yeah, my bad, I was dancing around unreachability as I needed to keep this code compiling on non-Windows due to limitations with cargo workspaces

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:44 on 2025-08-14 21:18_

ooh, shiny. To be honest the null-terminated strings ate up so much time in troubleshooting that I am scared to touch them too much. (I know, it's silly.)

---

_@paveldikov reviewed on 2025-08-14 21:18_

---

_@paveldikov reviewed on 2025-08-14 21:20_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:102 on 2025-08-14 21:20_

yes, I noticed the existing precedent. Unfortunately I couldn't really come up with a justification that really inspired confidence. Perhaps something like `// SAFETY: winapi call; null-terminated strings` based on my `git grep` inferences? To be honest I don't really understand the un-safety implications of win32 api calls... it seems like a 'necessary evil' type of rationale more than actual 'this is properly/verifiably safe'

---

_@paveldikov reviewed on 2025-08-14 22:05_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:102 on 2025-08-14 22:05_

ok, this was actually good since i found out that a few of the handles/ptrs weren't actually as safe as I'd thought. Added SAFETY comments -- feel free to reopen this if not adequate

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:71 on 2025-08-16 02:52_

```suggestion
            let path_str = path
                .as_os_str()
                .encode_wide()
                .chain(std::iter::once(0))
                .collect::<Vec<_>>();
```

Keeps it immutable

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:265 on 2025-08-16 03:03_

```suggestion
            let path_str = path
                .as_os_str()
                .encode_wide()
                .chain(std::iter::once(0))
                .collect::<Vec<_>>();
```

Keeps it immutable

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:265 on 2025-08-16 03:03_

```suggestion
                let name_null_term = name
                    .encode_utf16()
                    .chain(std::iter::once(0))
                    .collect::<Vec<_>>();
```

Keeps it immutable

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:269 on 2025-08-16 03:05_

Isn't name already null terminated here given it comes from `RESOURCE_` consts?

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:356 on 2025-08-16 03:06_

```suggestion
    let temp_dir = tempfile::TempDir::new()?;
```

nit: consistency w/ other uv crates

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:400 on 2025-08-16 03:06_

```suggestion
    let temp_dir = tempfile::TempDir::new()?;
```

nit: consistency w/ other uv crates

---

_Review comment by @samypr100 on `crates/uv-trampoline-builder/src/lib.rs`:572 on 2025-08-16 03:08_

```suggestion
        let temp_dir = tempfile::TempDir::new().expect("Failed to create temporary directory");
```

nit: consistency w/ other uv crates

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:65 on 2025-08-16 03:10_

```suggestion
        let resource_id_null_term = resource_id
            .encode_utf16()
            .chain(std::iter::once(0))
            .collect::<Vec<_>>();
```

keeps it immutable

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:64 on 2025-08-16 03:12_

resource_id is already null terminated, do we need to null terminate it again?

---

_@samypr100 reviewed on 2025-08-16 03:21_

---

_@samypr100 reviewed on 2025-08-16 03:28_

---

_Review comment by @samypr100 on `Cargo.lock`:84 on 2025-08-16 03:28_

It seems all dependencies were updated rather than the specific ones changed, was that intentional?

---

_@samypr100 reviewed on 2025-08-16 03:43_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:78 on 2025-08-16 03:43_

I'd also add this check to uv-trampoline-builder for consistency

---

_@paveldikov reviewed on 2025-08-16 12:51_

---

_Review comment by @paveldikov on `Cargo.lock`:84 on 2025-08-16 12:51_

Ah, it came out of a merge conflict resolution. I tried my best fixing up the lockfile by hand to minimise disruption, but cargo was not happy and insisted that I needed to regenerate it.

---

_@paveldikov reviewed on 2025-08-16 12:52_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:64 on 2025-08-16 12:52_

i think i might actually try the cstring type so i can have a more structural guarantee of null-termination

---

_@samypr100 reviewed on 2025-08-16 13:15_

---

_Review comment by @samypr100 on `Cargo.lock`:84 on 2025-08-16 13:15_

Easiest way might be to pull the one from main and run cargo build

---

_@samypr100 reviewed on 2025-08-16 13:23_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:64 on 2025-08-16 13:23_

In that case it might be easier to use the `w!` helper from windows crate which yields a terminated PCWSTR

---

_@samypr100 reviewed on 2025-08-16 13:26_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:64 on 2025-08-16 13:26_

e.g.

```rust
// Resource IDs matching uv-trampoline
const RESOURCE_TRAMPOLINE_KIND: PCWSTR = w!("UV_TRAMPOLINE_KIND");
const RESOURCE_PYTHON_PATH: PCWSTR = w!("UV_PYTHON_PATH");
// Note: This does not need to be looked up as a resource, as we rely on `zipimport`
// to do the loading work. Still, keeping the content under a resource means that it
// sits nicely under the PE format.
const RESOURCE_SCRIPT_DATA: PCWSTR = w!("UV_SCRIPT_DATA");
...
fn read_resource(handle: windows::Win32::Foundation::HMODULE, name: PCWSTR) -> Option<Vec<u8>> {
...
        let resource = FindResourceW(
            Some(handle),
            name,
            windows::core::PCWSTR(RT_RCDATA as *const _),
        );
...
fn write_resources(path: &Path, resources: &[(PCWSTR, &[u8])]) -> Result<(), Error> {
...
                UpdateResourceW(
                    handle,
                    windows::core::PCWSTR(RT_RCDATA as *const _),
                    *name,
...
```

---

_@paveldikov reviewed on 2025-08-16 15:53_

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:269 on 2025-08-16 15:53_

yeah this simplifies thanks to making the consts `PCWSTR` to begin with

---

_Review comment by @paveldikov on `crates/uv-trampoline-builder/src/lib.rs`:44 on 2025-08-16 16:17_

ended up using `PCWSTR` literals thanks to @samypr100's hint. That worked well although its presence in signatures leads to even more stuff having to be `cfg`'d out.

---

_@paveldikov reviewed on 2025-08-16 16:17_

---

_@paveldikov reviewed on 2025-08-16 16:18_

---

_Review comment by @paveldikov on `crates/uv-trampoline/src/bounce.rs`:69 on 2025-08-16 16:18_

Assuming it's fine to leave `W` variants be, but feel free to unresolve thread if any objections

---

_@samypr100 approved on 2025-08-16 18:22_

---

_Comment by @paveldikov on 2025-08-18 18:22_

@zanieb @Gankra let me know if there's anything you need from me on this one?

---

_Comment by @zanieb on 2025-08-18 19:30_

It's on my list to review and merge this week!

---

_Review comment by @geofft on `crates/uv-python/src/managed.rs`:911 on 2025-08-25 19:48_

We can clean this up now, right? (In a followup PR)

---

_@geofft approved on 2025-08-25 22:59_

This seems good to me (I have not looked at the .exe files) and I appreciate the test case involving signing. I'm curious if there are some things we can optimize (e.g., it looks like it writes out, reads in,  and deletes a temporary file which will probably just be written somewhere else, can we just write that directly? there are also some allocations we can skip) but none of that has to block landing this.

Is it really the case that zipimport will find the ZIP header at some arbitrary point in the binary? Should we worry about what happens if something else adds another ZIP header in the binary? (I thought part of the point of doing it via appending to the binary was that ZIP readers will look there.)

---

_Comment by @paveldikov on 2025-08-27 09:01_

@geofft, thank you for the feedback!

> I'm curious if there are some things we can optimize (e.g., it looks like it writes out, reads in, and deletes a temporary file which will probably just be written somewhere else, can we just write that directly? there are also some allocations we can skip) but none of that has to block landing this.

Yes, of course. I tried to keep the APIs mostly intact in this PR to minimise gratuitous disruption, but it does make sense to drop the distinction between 'generating' and 'writing' since 'generating' involves writing to disk anyway.

> Is it really the case that zipimport will find the ZIP header at some arbitrary point in the binary?

Yes, starting from the EOF and finding the first EOCD block. This is already happening with the current trampoline binary: the zip blob's EOCD block is already in an arbitrary place as far as any generic reader would be concerned -- it's close to EOF but not quite (EOF contains the `UVSC` magic number)

> Should we worry about what happens if something else adds another ZIP header in the binary?

Perhaps -- it's genuinely fragile and I am surprised this way of (ab)using `zipimport` ever took root. Although there are the following mitigating factors:
* This sounds like a very weird use case, possibly even contrived. I mean, people do very weird stuff, but this sounds objectively weirder than the typical garden-variety .exe file modifications such as changing the icon, stripping debug information, or indeed adding a signature for air-gapped environments.
* This is already a hazard with the current implementation(s) of the trampoline binaries, so there's a good chance that this use case is non-existent in the wild, because stuff would have probably broken already.
* This PR makes the situation no worse than it already is (apart from _possibly_ flipping the order of which ZIP blob comes first? If you're doing that I imagine you've already got a no-go on your hands? I A/B'd a few binaries and the difference is pretty negligible anyway, though if you are in the business of _also_ adding resources on top, all bets are off)

> (I thought part of the point of doing it via appending to the binary was that ZIP readers will look there.)

It's all code-archaeology AFAICT, but I don't think there was a deliberate reason apart from 'it happens to work' and 'concatenating byte-strings is much easier than proper PE binary editing'.

Circumstantial evidence suggests that `zipimport` came first (in Python 2.3 -- 2003!), and it was only later that `distlib` (ca. 2012/2013) and latterly `posy`/`uv-trampoline` ended up figuring out that `zipimport` is lenient enough to accept this use case.

More strategically, I think a PEP on this subject -- defining a formal, non-hacky and robust API for script trampoline binaries -- would be essential in order to prevent this from regressing on itself. Something like [PEP 397](https://peps.python.org/pep-0397/), but for script entrypoints.

---

_@zanieb reviewed on 2025-08-28 13:39_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:885 on 2025-08-28 13:39_

Note the previous use of `if cfg!` instead of `#[cfg]` was intentional so you can edit and check this code when not on a matching platform.

---

_@zanieb reviewed on 2025-08-28 13:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 13:58_

Why are these dropped? These are important?

---

_@zanieb reviewed on 2025-08-28 14:02_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:885 on 2025-08-28 14:02_

(I'm cleaning this up)

---

_@paveldikov reviewed on 2025-08-28 19:01_

---

_Review comment by @paveldikov on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 19:01_

Change of semantics. `write_to_file()` now takes a file path, not a file handle, and therefore takes care of file creation.

---

_@zanieb reviewed on 2025-08-28 19:58_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:885 on 2025-08-28 19:58_

Here's my commit https://github.com/astral-sh/uv/commit/f2b69d292c11fa74a39808ef4e6517fc308ab313 — I can't push to your repository.

---

_@zanieb reviewed on 2025-08-28 19:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 19:59_

We need it to fail if it's not creating a new file, that's why the original API took a handle.

---

_@zanieb reviewed on 2025-08-28 20:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 20:00_

A little more context at https://github.com/astral-sh/uv/pull/14790#discussion_r2220549092

---

_@paveldikov reviewed on 2025-08-28 21:58_

---

_Review comment by @paveldikov on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 21:58_

Thanks, I wasn't aware of this requirement. (I still don't _get it_, but I trust that it's valid)

IIRC I refactored it this way in order to avoid double-opening the file, which is fraught on Windows. Using a `fs_err::write_file()` one-and-done is nice because it leaves no contention for `write_resources()`  which likewise opens the file, but in a winapi way. (none of the winapi methods accept native Rust `File` objects, so you couldn't even properly re-use the file handle either.)

Maybe add a boolean `exists_ok` arg to `write_to_file()`? And replace `fs_err::write_file()` with a deliberately scoped block with the appropriate `OpenOptions`... maybe that will do the trick?

---

_@paveldikov reviewed on 2025-08-28 22:00_

---

_Review comment by @paveldikov on `crates/uv-python/src/managed.rs`:885 on 2025-08-28 22:00_

Very weird -- I've ticked the 'accept pushes from maintainers' option. (I have invited you as a collaborator to my fork anyway)

---

_@zanieb reviewed on 2025-08-28 22:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-28 22:05_

I will look a little closer

---

_Comment by @T-256 on 2025-08-29 00:16_

FWIW before merge, I just tested binary of x86_64 got from ci summary with Python3.7.
Unfortunately, using zip payload in a section of PE doesn't help with Python3.7's zipimport module.
To keep compatible with Py3.7, I'd recommend to add zip (script_data) to end of target file instead of save it on a section of PE format. it could be thought as follow-up after merge, too.


---

_Comment by @paveldikov on 2025-08-29 00:29_

@T-256 

> To keep compatible with Py3.7, I'd recommend to add zip (script_data) to end of target file instead of save it on a section of PE format.

But then it wouldn't work with code-signing, because fixed positioning of the zip blob gets wrecked by code signing tools.

Also -- the current trampoline is already incompatible with 3.7. (Do we actively want to _reintroduce_ compatibility?)

---

_Comment by @zanieb on 2025-08-29 12:20_

> Also -- the current trampoline is already incompatible with 3.7. (Do we actively want to reintroduce compatibility?)

No — if this introduced compatibility that'd be a nice property, but it's not an active goal for the project.

---

_@zanieb reviewed on 2025-08-29 12:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-29 12:30_

I think the most immediate solution is to include an API which performs the same two-step operation as `windows_script_launcher` where it writes everything to a temporary file, reads that to get the bytes, then writes that to an open file handle.

As a separate note, the two step operation in `windows_script_launcher` is pretty weird for normal operation! We're writing everything just to read it then write it again :) I worry about the performance implications of that and would like to refactor to avoid that unless the caller needs ownership of the file handle (e.g., as above).

Unfortunately, the general pattern of using `write_file` then `write_resources` does leave room for races (and this is not just theoretical, things like this have bit us before). I think it's okay for now, but we'll probably want to look for a way to avoid that too.

Circling back, I guess the two step approach of `windows_script_launcher` actually does resolve this problem... which hints to a simple solution: just construct the launcher in a temporary file that we're the sole writer to, then rename it into place (this is what we do for atomic writes in general, e.g., see `write_atomic`).

---

_@paveldikov reviewed on 2025-08-30 20:25_

---

_Review comment by @paveldikov on `crates/uv/src/commands/project/run.rs`:1984 on 2025-08-30 20:25_

Happy for you to decide which way you want to go? I do think that the two-step operation is not optimal (or at least not aesthetically pleasing), although I kept it as such to avoid gratuitous API churn -- it's a chunky PR already. But if it does incidentally give us safety, then I guess that's alright. Although I imagine it is better to make the atomicity explicit rather than incidental!

---

_@zanieb reviewed on 2025-09-12 12:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:1984 on 2025-09-12 12:48_

Yeah it looks like there'd be a bunch of API churn here because we need to hash the contents for the RECORD when writing entrypoints.

I think we should change it. I'm vaguely worried about this doubling the overhead of writing entrypoints on Windows, but they're small so I can live with it for now. It makes sense to do separately.

https://github.com/astral-sh/uv/pull/15068/commits/d4303fd40c91895a7a52b9414c6decc059a407a0 addresses my blocking comment, I think.

---

_Comment by @paveldikov on 2025-10-13 13:41_

Hello! Sorry for not engaging much on this PR in the last month and a bit. I am still interested in it -- is there any way I can help?

---

_Comment by @zanieb on 2025-10-13 13:48_

Sorry I lost track of this too. Let me take a look again!

---

_Renamed from "refactor(uv-trampoline): use `.rcdata` to store trampoline type + path to python binary" to "Use `.rcdata` to store trampoline type + path to python binary" by @konstin on 2025-10-15 11:48_

---

_@zanieb reviewed on 2025-11-09 04:01_

---

_Review comment by @zanieb on `Cargo.toml`:218 on 2025-11-09 04:01_

I wonder if we should be using https://docs.rs/pkcs12/latest/pkcs12/ instead which is maintained?

---

_@zanieb reviewed on 2025-11-09 04:11_

---

_Review comment by @zanieb on `Cargo.toml`:218 on 2025-11-09 04:11_

(It actually looks like that doesn't support writing archives)

---

_@samypr100 reviewed on 2025-11-09 04:15_

---

_Review comment by @samypr100 on `Cargo.toml`:218 on 2025-11-09 04:15_

Was there a reason P12 format was needed in particular?

---

_@zanieb reviewed on 2025-11-09 04:27_

---

_Review comment by @zanieb on `Cargo.toml`:218 on 2025-11-09 04:27_

I'm not sure.

I also briefly looked at https://github.com/ancwrd1/p12-keystore which seems fine too.

It'd be nice to use a dependency that comes from a higher trust source, even if it's just for a test.

---

_@samypr100 reviewed on 2025-11-09 04:31_

---

_Review comment by @samypr100 on `Cargo.toml`:218 on 2025-11-09 04:31_

Looking at the [test](https://github.com/astral-sh/uv/blob/d4131f25c1b2c3ae935878b8881844a2a3e09e59/crates/uv-trampoline-builder/src/lib.rs#L632-L658) and the Set-AuthenticodeSignature docs, I wonder if its possible to avoid P12 entirely by manually instantiating a System.Security.Cryptography.X509Certificates.X509Certificate2 bundle. I believe that would allows us to only rely on rcgen and not p12 (if it's possible). I'll give it a shot.

---

_Review comment by @samypr100 on `Cargo.toml`:218 on 2025-11-09 04:53_

Success, although it will require use of pwsh rather than powershell because `System.Security.Cryptography.X509Certificates.X509Certificate2::CreateFromPemFile` is .NET 5+

---

_@samypr100 reviewed on 2025-11-09 04:53_

---

_@samypr100 reviewed on 2025-11-09 04:54_

---

_Review comment by @samypr100 on `Cargo.toml`:218 on 2025-11-09 04:54_

The CI should have pwsh already so I don't think its a problem and I don't think we truly need to test in legacy powershell in this particular case as we've proven it works with legacy powershell already using pfx. I'll push my changes soon in case you're interested.

---

_@samypr100 reviewed on 2025-11-09 05:08_

---

_Review comment by @samypr100 on `Cargo.toml`:218 on 2025-11-09 05:08_

See 153d854e5d8400ca79877ca65f64f4694e8de6c4

---

_@samypr100 reviewed on 2025-11-09 05:36_

---

_Review comment by @samypr100 on `Cargo.toml`:199 on 2025-11-09 05:36_

@zanieb I also don't think we need to change the windows version as part of this PR either. Everything should work fine if we keep the previous windows versions intact. That will minimize the changes to the lock file to be just isolated to rcgen.

---

_Review comment by @samypr100 on `Cargo.toml`:199 on 2025-11-09 06:07_

After f7e549d36da686f5f6f76baf6df08a30ca74ecf8 windows version is now the same as main. It also removed small unrelated changes to windows_exception in this PR as a result of the upgrade.

---

_@samypr100 reviewed on 2025-11-09 06:07_

---

_@zanieb reviewed on 2025-11-09 14:12_

---

_Review comment by @zanieb on `Cargo.toml`:199 on 2025-11-09 14:12_

Thanks!

---

_Merged by @zanieb on 2025-11-09 14:12_

---

_Closed by @zanieb on 2025-11-09 14:12_

---

_@paveldikov reviewed on 2025-11-09 18:48_

---

_Review comment by @paveldikov on `Cargo.toml`:218 on 2025-11-09 18:48_

nice! Re the 'why' question: I did it because of a (possibly incorrect) inference that pwsh on CI did not have the ability to modify the credential store

---

```yaml
number: 6375
title: "feat: Support remote scripts with `uv run`"
type: pull_request
state: merged
author: manzt
labels:
  - enhancement
assignees: []
merged: true
base: main
head: manzt/remote-scripts
created_at: 2024-08-21T20:15:59Z
updated_at: 2024-10-16T22:13:39Z
url: https://github.com/astral-sh/uv/pull/6375
synced_at: 2026-01-10T12:53:33Z
```

# feat: Support remote scripts with `uv run`

---

_Pull request opened by @manzt on 2024-08-21 20:15_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

First off, congratulations on the 0.3 release! The PEP 723 standalone scripts support is awesome, and I can already imagine a long tail of little scripts of my own that would benefit from this functionality.

## Background

I really like the Deno CLI's support for running and installing remote scripts. 

```
deno run <url>
```

```
deno install --name foo <url>
```

I can see parallels with `uv run` and `uvx`. After mentioning this on Discord, @zanieb suggested I could take a stab at a PR to implement similar functionality for uv.

## Summary

This PR attempts to add support for executing remote standalone scripts directly with `uv run`. While this is already possible by downloading the script (i.e., via curl/wget) and then using uv run, having direct support would be convenient.

The proposed functionality is:

```sh
uv run <url>
```

Another addition/alternative could be to support running scripts via stdin:

```sh
curl -sL <url> | uv run -
```

But that is not implemented in this PR.

## Test Plan

I noticed that GitHub and `files.pythonhosted.org` URLs are used in some of the tests. I've created a personal [GitHub Gist](https://gist.github.com/manzt/cb24f3066c32983672025b04b9f98d1f) with the example from PEP 723 for now to test this functionality.

~However, I couldn't figure out how to get the `with_snapshot` config filter to filter out the tempfile path, so the test is currently failing. Any assistance with this would be appreciated.~

## Notes

I'm not totally pleased with the implementation of this PR. I think it would be better to handle the case earlier (and probably reuse the cache), and avoid mutation, but since run command requires a local path this was the simplest implementation I could come up with.

I know that performance is paramount with uv so I totally understand if this requires a different approach or something more explicit to avoid "inferring" the path. I'm just taking this as an opportunity to learn a little more Rust and acquaint myself with the code base. cheers!


---

_Comment by @zanieb on 2024-08-21 20:22_

Awesome! Thanks for putting this up.

---

_Assigned to @zanieb by @zanieb on 2024-08-21 20:22_

---

_Label `enhancement` added by @zanieb on 2024-08-21 20:22_

---

_@manzt reviewed on 2024-08-21 20:36_

---

_Review comment by @manzt on `crates/uv/tests/run.rs`:2139 on 2024-08-21 20:36_

Just copied the example from [PEP 723](https://peps.python.org/pep-0723/), but realizing now the snapshot will likely change over time (with new versions of deps). Probably better to have a more simple test / and simple dependency.

---

_@zanieb reviewed on 2024-08-21 20:37_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:2139 on 2024-08-21 20:37_

We'll probably create something in the Astral org anyway, thanks!

---

_Comment by @mgaitan on 2024-08-23 18:53_

Amazing feature. Imagine this with #6283  

---

_Comment by @zanieb on 2024-09-17 04:15_

Sorry for the delay, I've been stretched thin on reviews.

@BurntSushi would you be able to take over review of this?

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:61 on 2024-09-17 16:50_

You should be able to move this in the function definition. Parameters are actually themselves patterns, they just have to be irrefutable patterns. So, `async fn run(mut cli: Cli)` should be fine here.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:152 on 2024-09-17 16:58_

This is pretty deeply nested. I might suggest moving this to a helper function where you can use early returns to clean this up a bit.

---

_@BurntSushi reviewed on 2024-09-17 18:49_

Before getting into the weeds of how this is implemented, I wanted to pop-up a level and think about this feature more holistically. And I think I do have a significant concern here.

My main concern is that it is technically possible for a valid URL to also be a valid file path. For example:

```
$ mkdir 'https:'
$ echo wat 'https:/example.com'
$ cat https://example.com
wat
```

If `uv run https://example.com` were used, then I believe this PR would result in `uv` doing an HTTP request instead of using the file path on disk.

Now, I'm not especially worried about this happening in normal usage. The double slash for example is clearly a bit of subterfuge to provoke the ambiguity. So I think the thing where I'm worried is if someone somehow causes a `uv run` command to run where the end user _thinks_ it's hitting a local file but it's actually hitting a URL. I had a bug similarish to this in ripgrep a while back where end users thought the system `gzip` executable was being used, but if they cloned a repository containing a `gzip.exe` on Windows and ran `rg` from that same directory, then a quirk of Windows would result in executing the `gzip.exe` in the clone instead.

There are a lot of different possible mitigations to this. Or perhaps this isn't as much of a problem as I think. For example:

* This could be a feature that requires some opt-in flag to be enabled in our config.
* The default mode could be to show the Python script that will be executed on stdout first and then provide a prompt confirming it. And then add a `--no-confirm` (or whatever) flag to make it non-interactive.
* We could look for a file path matching the URL, and if it exists, either use the local file or report an error.

Maybe there's another idea I haven't thought of.

---

_Unassigned @zanieb by @zanieb on 2024-09-17 18:51_

---

_Assigned to @BurntSushi by @zanieb on 2024-09-17 18:51_

---

_Comment by @zanieb on 2024-09-17 18:53_

I generally have a preference for the "We could look for a file path matching the URL, and if it exists, either use the local file or report an error." option rather than prompting. Opt-in might make sense, but via a `--remote` flag rather than a persistent configuration option (maybe that's what you meant though).

---

_Comment by @zanieb on 2024-09-17 19:02_

See also https://github.com/astral-sh/uv/issues/7396#issuecomment-2351129628

---

_Comment by @BurntSushi on 2024-09-18 13:31_

If we go the route of prioritizing a local file when given a URL when a file exists, then I think that probably satisfies me. I can't think of a way for that to go wrong from a "accidentally executed a remote script instead of the intended local file" perspective.

There may still be an argument in favor of a `--remote` flag, but more in a vague "otherwise it's too easy to run remote scripts" sense. I don't feel as strongly about that personally.

@manzt What do you think?

---

_Comment by @manzt on 2024-09-18 17:03_

Thank you for raising this concern. I agree it's a valid issue I hadn't considered.

My preference aligns with @zanieb's suggestion to prioritize checking for a local file path matching the URL first, erroring if a match is found. For context, I checked your example with deno, which always makes the HTTP request (though it operates in a more sandboxed environment than Python, not sure why they decided on this behavior).

Regarding a `--remote` flag, while I don't have strong arguments for or against it, I appreciate its explicitness. The common use case I envision is sharing links to Gists or GitHub, where adding `--remote` before execution seems a reasonable safeguard (especially if the readme just has a copy link).

It's worth noting that with #6467, I believe scripts can already be executed via `curl <url> | uv run -` (though last time I checked the script metadata wasn't parsed from stdin). However, handling this directly in uv does seem really nice UX. 

Apologies for the delayed response!

---

_Comment by @BurntSushi on 2024-09-19 13:53_

All righty, let's go with checking for whether a local file exists or not.

@charliermarsh Do you have any opinions here? Do you think `uv run https://example.com/foo.py` should "just work," or do you think there should be an extra step needed, e.g., `uv run --remote https://example.com/foo.py`? (Above we discussed mitigating the case where `https://example.com/foo.py` actually refers to a valid file path, in which case, we would just read the local file instead.)

---

_Comment by @zanieb on 2024-09-19 14:41_

I sort of prefer "just works"

---

_Comment by @BurntSushi on 2024-09-19 14:48_

Same.

---

_Comment by @charliermarsh on 2024-09-19 15:00_

Same. Whatever we do, it would be nice to align the behavior of requirements.txt files, which can also be either remote or local.

---

_Comment by @manzt on 2024-09-19 21:36_

Seems like agreement, I’ll start addressing the prior comments and handle the file behavior!

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:61 on 2024-09-24 20:39_

TIL, thanks!

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:152 on 2024-09-25 18:35_

Done.

---

_Comment by @manzt on 2024-09-25 18:37_

Thanks for your patience. I think I was able to achieve the desired behavior and added another test.

I'm not sure how we should plan on testing the remote behavior (right now still depends on a gist with a fixture of my own).

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:50 on 2024-09-26 12:23_

There should be another line break here. Otherwise this all runs together as one paragraph when rendered.

For Rust APIs that appear on the summary page of a module (types, functions, modules), the first "Markdown paragraph" is shown as a brief summary on that page. To keep things clean, it's best to limit this to one line/sentence. Your first sentence here is perfect, but because there's no line break, that summary will also include the text below too.

---

_Review comment by @BurntSushi on `crates/uv/tests/run.rs`:2139 on 2024-09-26 12:28_

How about this:

* Submit a separate PR with a new Python program in the `scripts` directory. Name it, `uv-run-remote-script-test.py` and include a comment in the file explaining why it exists.
* Make it do something stupidly simple. Like just print a string or something.

Then once that PR is merged, we should have a stable URL for it. And then you can use it in this PR.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:177 on 2024-09-26 13:11_

Why does this start with an underscore? I think you should be able to remove it.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:58 on 2024-09-26 13:20_

I think you should be using [`Path::try_exists`](https://doc.rust-lang.org/std/path/struct.Path.html#method.try_exists) here. If it's result is anything other than `Ok(true)`, then you bail.

The idea here is that I think we want to be pretty conservative: we should only execute a remote script if we can _absolutely confirm_ there is no corresponding local file path. Otherwise, you open yourself up to other vectors where by an end user unintentionally runs a remote script.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:64 on 2024-09-26 13:23_

What happens if there is more than one positional argument? I think this means we ignore it? Should we return `Ok(None)` instead? (I'm actually not quite sure about this one.)

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:63 on 2024-09-26 13:23_

I would use an early return here. So if the target _doesn't_ look like a URL, then `return Ok(None);`. This keeps rightward drift in check and makes it easy to see under what conditions the function bails.

---

_@BurntSushi requested changes on 2024-09-26 13:24_

Nice work! This is coming along. :-) I've requested another round of changes and suggested a way forward with respect to testing.

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:177 on 2024-09-26 14:12_

Must Rust knowledge isn't that great, but it seems the `Drop` implementation for `NamedTempFile` unlinks the temporary file. MRE:

```rust
use tempfile::NamedTempFile;

fn main() {
    let path = {
        let tmpfile = NamedTempFile::new().unwrap();
        tmpfile.path().to_str().unwrap().to_string()
    };
    assert!(std::path::Path::new(&path).exists()); // panics!
}
```

The only way I could figure out how to keep it around is to have `resolve_script_target` return the owned temp file and hoist it into a higher scope (then clippy yelled at me for an unused variable, hence the underscore).

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:50 on 2024-09-26 14:13_

Awesome, thanks for taking the time to explain that.

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:58 on 2024-09-26 14:26_

> I think you should be using `Path::try_exists` here.

Thanks for the pointer!

> If it's result is anything other than `Ok(true)`, then you bail.

Maybe I'm having a hard time thinking this through, but right we "bail" from this function if a file exists. I think we want to bail for anything that is not `Ok(false)`? (I went ahead and made these changes, sorry if I'm missing something).


---

_Review comment by @manzt on `crates/uv/src/lib.rs`:63 on 2024-09-26 14:34_

Good idea.

---

_@manzt reviewed on 2024-09-26 14:35_

Thanks for the review! Learning a lot.

Just a clarifying question about Path::try_exists.

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:64 on 2024-09-26 14:40_

> What happens if there is more than one positional argument?

if there is more than one, we don't change them (so they are forwarded down to the python program). E.g., it would be nice to support:

```sh
uv run <url> [python program flags]
```

---

_@manzt reviewed on 2024-09-26 14:40_

---

_@manzt reviewed on 2024-09-26 15:06_

---

_Review comment by @manzt on `crates/uv/tests/run.rs`:2139 on 2024-09-26 15:06_

should we point to main here?

---

_Comment by @manzt on 2024-09-26 15:10_

hmm 

```sh
cargo run run https://raw.githubusercontent.com/astral-sh/uv/main/scripts/uv-run-remote-script-test.py CI
```

seems to be working on this branch, but the test suite is having trouble resolving the version of `rich`. Maybe I'm missing something in the setup of the `TestContext`.

---

_@BurntSushi reviewed on 2024-09-26 15:32_

---

_Review comment by @BurntSushi on `crates/uv/tests/run.rs`:2139 on 2024-09-26 15:32_

No I think the commit revision like you have it here is the right way to go. But I would use the full revision hash.

---

_Comment by @BurntSushi on 2024-09-26 15:34_

> hmm
> 
> ```shell
> cargo run run https://raw.githubusercontent.com/astral-sh/uv/main/scripts/uv-run-remote-script-test.py CI
> ```
> 
> seems to be working on this branch, but the test suite is having trouble resolving the version of `rich`. Maybe I'm missing something in the setup of the `TestContext`.

Ah right my bad! Our tests set `--exclude-newer` to avoid new releases of packages changing resolutions. Really sorry (especially since I'm the one who asked for a version pin!), but this means we'll need another PR to change the script we just merged to use an older version of `rich`. I think `13.7.0` should be old enough since that was published before our `EXCLUDE_NEWER` value:

https://github.com/astral-sh/uv/blob/c8357b7bf23b4ade3b98be74bd7a469c05e6ec02/crates/uv/tests/common/mod.rs#L29-L30

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:64 on 2024-09-26 15:35_

Perfect. SGTM.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:72 on 2024-09-26 15:38_

I think you can just do:

```rust
if !matches!(Path::new(target).try_exists(), Ok(false)) {
    return Ok(None);
}
```

There are actually a lot of different ways to write this, but I think I like this one the best. But the `if` with the empty body is a bit odd. :)

---

_Comment by @manzt on 2024-09-26 15:39_

No worries, I opened https://github.com/astral-sh/uv/pull/7713

---

_Comment by @manzt on 2024-09-26 15:46_

Test is working now, pointing to #7713. Will switch to hash on main once merged.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:90 on 2024-09-26 15:47_

Will this result in `script.py.py` if the `script.py` fallback is used above?

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:177 on 2024-09-26 15:53_

Ahhhh yes okay, I didn't realize it wasn't actually being used below. This is kind of interesting because I'm not sure there is actually a guarantee that `_maybe_tempfile` won't just be dropped right away. The way to fix that for sure would be this patch:

```rust
diff --git a/crates/uv/src/lib.rs b/crates/uv/src/lib.rs
index 6c21f243..c6096124 100644
--- a/crates/uv/src/lib.rs
+++ b/crates/uv/src/lib.rs
@@ -174,10 +174,10 @@ async fn run(mut cli: Cli) -> Result<ExitStatus> {
     };
 
     // Parse the external command, if necessary.
-    let mut _maybe_tempfile: Option<tempfile::NamedTempFile> = None;
+    let mut maybe_tempfile: Option<tempfile::NamedTempFile> = None;
     let run_command = if let Commands::Project(command) = &mut *cli.command {
         if let ProjectCommand::Run(uv_cli::RunArgs { command, .. }) = &mut **command {
-            _maybe_tempfile = resolve_script_target(command).await?;
+            maybe_tempfile = resolve_script_target(command).await?;
             Some(RunCommand::try_from(&*command)?)
         } else {
             None
@@ -298,7 +298,7 @@ async fn run(mut cli: Cli) -> Result<ExitStatus> {
     // Configure the cache.
     let cache = Cache::from_settings(cache_settings.no_cache, cache_settings.cache_dir)?;
 
-    match *cli.command {
+    let result = match *cli.command {
         Commands::Help(args) => commands::help(
             args.command.unwrap_or_default().as_slice(),
             printer,
@@ -1185,7 +1185,9 @@ async fn run(mut cli: Cli) -> Result<ExitStatus> {
                 commands::build_backend::prepare_metadata_for_build_editable(&wheel_directory)
             }
         },
-    }
+    };
+    drop(maybe_tempfile);
+    result
 }
 
 /// Run a [`ProjectCommand`].
```

But this brings up an interesting point: what happens if executing the downloaded Python script fails? I guess it would show the temporary file path and not the URL, which is potentially confusing.

---

_@BurntSushi reviewed on 2024-09-26 15:53_

---

_@BurntSushi reviewed on 2024-09-26 15:53_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:58 on 2024-09-26 15:53_

Yes, you're 100% right. I got the logic flipped around! Whoops.

---

_@manzt reviewed on 2024-09-26 15:55_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:72 on 2024-09-26 15:55_

ooo, I like this.

---

_@manzt reviewed on 2024-09-26 16:08_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:177 on 2024-09-26 16:08_

Thanks! I applied the patch.

> But this brings up an interesting point: what happens if executing the downloaded Python script fails? I guess it would show the temporary file path and not the URL, which is potentially confusing.

There is already some indication of the tempfile location with the summary of parsing script 723 metadata:

```sh
    Reading inline script metadata from: [TEMP_PATH].py
```

Perhaps we could add some context/logging that says "Downloading [URL] to [TEMP_PATH]?"

thus tracebacks from a failed Python script would have more context?



---

_@manzt reviewed on 2024-09-26 16:09_

---

_Review comment by @manzt on `crates/uv/tests/run.rs`:2139 on 2024-09-26 16:09_

Done.

---

_@BurntSushi reviewed on 2024-09-26 16:11_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:177 on 2024-09-26 16:11_

> Perhaps we could add some context/logging that says "Creating a tempfile for [URL] in [TEMP_PATH]?

I like this!

---

_@manzt reviewed on 2024-09-26 16:18_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:90 on 2024-09-26 16:18_

Ah yeah, you are right. tempfile adds a hash and a suffix to the prefix.

```sh
cargo run run https://raw.githubusercontent.com/astral-sh/uv/main/scripts/uv-run-remote-script-test.py CI
# Reading inline script metadata from: /var/folders/gs/j5y7yv816z71dzy18r4qmy3h0000gq/T/uv-run-remote-script-test.pyznxLtC.py
# Installed 4 packages in 21ms
# Hello CI, from uv!
```

It might be easiest to:

```rust
    let temp_file = tempfile::Builder::new()
        .suffix(".py")
        .tempfile?;
```

Depends on how useful it is to try for the file name. It's only used to report to the end user and would show up in traceback messages from Python errors.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:90 on 2024-09-26 16:22_

I think including the file name in there as a prefix is fine.  Especially if we add a log message saying that the remote script was written to the temp file.

I think my issue here is the case where the `script.py` _fallback name_ is used (from above). Does that result in a double `.py` suffix?

---

_@BurntSushi reviewed on 2024-09-26 16:22_

---

_@manzt reviewed on 2024-09-26 16:34_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:90 on 2024-09-26 16:34_

I think the issue also happens if the file has as `.py`. For example:

https://raw.githubusercontent.com/astral-sh/uv/main/scripts/uv-run-remote-script-test.py

results in:

uv-run-remote-script-test.pyznxLtC.py

So `script.py` would similarly result in `script.py[hash].py`.

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:90 on 2024-09-26 16:36_

Yeah, so I think you want to strip the `.py` suffix from the file name, but keep the `.py` suffix in the temp file builder.

---

_@BurntSushi reviewed on 2024-09-26 16:36_

---

_@manzt reviewed on 2024-09-26 16:41_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:90 on 2024-09-26 16:41_

Ah, much more simple. Done!

---

_@manzt reviewed on 2024-09-26 16:49_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:177 on 2024-09-26 16:49_

I've added a test and added the reporting:

```
Downloading remote script to: [TEMP_PATH]
```

---

_Comment by @BurntSushi on 2024-09-27 14:54_

The CI failure looks legitimate. My guess is that the file path is just totally invalid on Windows, which in turn causes `Path::new(...).try_exists()` to fail and thus bail out of the remote logic. So I think I would change the `if` to this:

```rust
    // Only continue if we are absolutely certain no local file exists.
    //
    // We don't do this check on Windows since the file path would
    // be invalid anyway, and thus couldn't refer to a local file.
    if cfg!(unix) && !matches!(Path::new(target).try_exists(), Ok(false)) {
        return Ok(None);
    }
```

Also, I think we need to add docs to `uv run` mentioning this feature. :)

---

_Comment by @manzt on 2024-09-27 15:14_

Thanks for the tips!

> Also, I think we need to add docs to uv run mentioning this feature. :)

I took a stab at updating the docs.

---

_Comment by @BurntSushi on 2024-09-27 15:16_

I think you need to run `cargo dev generate-cli-reference`. Basically, changes to the docs need to be propagated out to other places.

---

_Comment by @manzt on 2024-09-27 15:21_

Ah, sorry... wasn't sure what the source of truth was. Thanks!

---

_Comment by @manzt on 2024-09-27 15:30_

Any thoughts on how to update the test checking for the local file? Is there a way to mark that a test _should_ fail on windows for example?

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:89 on 2024-09-27 16:06_

Hmmmm. This is an issue I think. We should be using a `Printer` here derived from config settings. Otherwise this message won't be squashed when, e.g., `--quiet` is passed. I'm not quite sure how to solve that. Another approach would be to emit this message after a `Printer` is constructed below. See:

```
    // Configure the `Printer`, which controls user-facing output in the CLI.
    let printer = if globals.quiet {
        Printer::Quiet
    } else if globals.verbose > 0 {
        Printer::Verbose
    } else if globals.no_progress {
        Printer::NoProgress
    } else {
        Printer::Default
    };
```

---

_Review comment by @BurntSushi on `docs/reference/cli.md`:57 on 2024-09-27 16:06_

```suggestion
When used with a file ending in `.py` or an HTTP(S) URL, the file will be treated as a script and run with a Python interpreter, i.e., `uv run file.py` is equivalent to `uv run python file.py`. For URLs, the script is downloaded to a temporary file before execution. If the script contains inline dependency metadata, it will be installed into an isolated, ephemeral environment. When used with `-`, the input will be read from stdin, and treated as a Python script.
```

---

_@BurntSushi reviewed on 2024-09-27 16:07_

> Any thoughts on how to update the test checking for the local file? Is there a way to mark that a test should fail on windows for example?

I don't think it should fail though? It looks like it's failing because it can't write a temp file? I'm not sure what the issue is there.

---

_@manzt reviewed on 2024-09-27 16:50_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:89 on 2024-09-27 16:50_

I updated the script to check whether the tempfile exists and report that a file was downloaded (reusing the `Printer`).

I think this is the most minimal way to respect `--quiet`. From an end user perspective, it would probably be nicer to report when we are actually starting to download the script, but I'm not quite should how to solve that without a bigger refactor.

One idea would be to create the temp file immediately and kick off the download, but await a future for the download/writing after eprinting "Downloading remote script...". Maybe too complicated.

---

_Comment by @manzt on 2024-09-27 16:54_

> I don't think it should fail though? It looks like it's failing because it can't write a temp file? I'm not sure what the issue is there.

I think on Windows we skip checking for a local file, which would result in requesting`https://example.com/path/to/main.py`

> It looks like it's failing because it can't write a temp file?

Right, I'm also not sure of this error... but if we were to write the tempfile I think it would still fail. Maybe I'm missing something.


---

_Comment by @BurntSushi on 2024-09-27 16:58_

> I think on Windows we skip checking for a local file, which would result in requestinghttps://example.com/path/to/main.py

OK, so you're referring to the `run_url_like_with_local_file_priority` test here. I was looking at `run_remote_pep723_script`.

For `run_remote_pep723_script`, I think the test should pass.

For `run_url_like_with_local_file_priority`, right, the thing being tested doesn't make sense on Windows. So I would probably just add a `#[cfg(unix)]` on that test with a comment explaining why that's there.

---

_@BurntSushi reviewed on 2024-09-27 16:59_

---

_Review comment by @BurntSushi on `crates/uv/src/lib.rs`:89 on 2024-09-27 16:59_

I think what you have is fine for now. Thanks!

---

_Comment by @manzt on 2024-09-27 17:08_

> OK, so you're referring to the `run_url_like_with_local_file_priority` test here. I was looking at `run_remote_pep723_script`.

Yes, apologies for the confusion. I added the `#[cfg(unix)]` with a comment.

---

_Comment by @manzt on 2024-09-27 17:08_

> For run_remote_pep723_script, I think the test should pass.

Agreed.

---

_Comment by @manzt on 2024-10-01 20:00_

I wonder if the issue on Windows is related to some behavior behind how [`NamedTempFile`](https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html#windows) works on Windows. Are we "Opening a named temporary file in a world-writable directory."?

---

_@charliermarsh reviewed on 2024-10-10 13:04_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:86 on 2024-10-10 13:04_

I think this needs to go through the uv client. Otherwise, it won't respect `--offline`, SSL certs, etc.

---

_Comment by @BurntSushi on 2024-10-10 13:49_

> I wonder if the issue on Windows is related to some behavior behind how [`NamedTempFile`](https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html#windows) works on Windows. Are we "Opening a named temporary file in a world-writable directory."?

I'm not sure. It seems like it's [`NamedTempFile::persist`](https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html#method.persist) that's failing.

Oh, I think I see the problem. You're using `write_atomic` and passing the path of the temporary file you just created to it. And `write_atomic` tries to create another temporary file at that same path and persists it. Specifically, I think this is where the error is coming from:

https://github.com/astral-sh/uv/blob/c07000718aa8eb3321f5d46bd454ae863ffc1963/crates/uv-fs/src/lib.rs#L147-L156

I don't think you need `write_atomic` here. Can you say more about why you used it? I think just a [`write_all`](https://doc.rust-lang.org/std/io/trait.Write.html#method.write_all) on the `NamedTempFile` itself is sufficient.

---

_Comment by @manzt on 2024-10-10 14:12_

> Oh, I think I see the problem.

Thanks for explanation. I've learned a lot from this PR.

> I don't think you need write_atomic here. Can you say more about why you used it? I think just a write_all on the NamedTempFile itself is sufficient.

It was naive of me. I was looking for how files were written (async) in other parts of the project, but for a tempfile I realize we really don't need. Replaced with `write_all`!

---

_@manzt reviewed on 2024-10-10 14:40_

---

_Review comment by @manzt on `crates/uv/src/lib.rs`:86 on 2024-10-10 14:40_

Ok, using the client builder now. I went with re-resolving the global args locally since the contents of file downloaded by this script updates the resolution later.

---

_Comment by @charliermarsh on 2024-10-10 14:49_

Do we need a file for this? Could we just read into memory and pass the contents over stdin?

---

_Comment by @charliermarsh on 2024-10-10 14:50_

(I could also look into that separately, but I'm wondering if it would remove a lot of the complexity around tempfiles, drop, etc.)

---

_Comment by @manzt on 2024-10-10 14:55_

> Do we need a file for this? Could we just read into memory and pass the contents over stdin

Definitely! However, I went this direction because writing to a tempfile was the easiest way to support PEP 723 metadata and passing down arguments to the Python script. IIRC PEP 723 is only parsed (currently) from `RunCommand::PythonScript | RunCommand::PythonGuiScript` (not stdin):

https://github.com/astral-sh/uv/blob/7bac708b97bcd032fa8b03cf34b45938f4933469/crates/uv/src/lib.rs#L150-L172

and using stdin resolves to `python -c <contents>` and doesn't capture other addtional arguments/flags for the python script.


---

_Comment by @charliermarsh on 2024-10-10 15:25_

Okay, makes sense. I think we should follow-up by changing that -- it seems like an oversight that we don't detect scripts there!

---

_Comment by @charliermarsh on 2024-10-10 16:06_

Funnily enough: https://github.com/astral-sh/uv/issues/8097

---

_Comment by @charliermarsh on 2024-10-10 16:06_

I defer to @BurntSushi to merge.

---

_@BurntSushi approved on 2024-10-10 16:06_

I think this LGTM now! Amazing work @manzt. Thank you for sticking with this. I know there was a lot of back-and-forth to get this merged, but I think it's in a good state now!

---

_Comment by @BurntSushi on 2024-10-10 16:07_

I think you just need to rebase this on `main`.

---

_Merged by @BurntSushi on 2024-10-10 18:10_

---

_Closed by @BurntSushi on 2024-10-10 18:10_

---

_Comment by @BurntSushi on 2024-10-10 18:11_

> I think you just need to rebase this on `main`.

Nope that was wrong! I usually do rebase & merge for my PRs, so that was selected (even though I was always planning to squash this PR) and _that_ way greyed out. But squash & merge was fine. Interesting.

---

_Comment by @manzt on 2024-10-10 18:13_

Ok good! I tried rebasing and it was scary :)

---

_Branch deleted on 2024-10-10 21:54_

---

_Comment by @hauntsaninja on 2024-10-11 10:38_

Downloading has some semantic advantages, `__file__` is meaningful, tracebacks are better, etc

---

_Comment by @charliermarsh on 2024-10-11 10:40_

Yeah I looked into it and agree that downloading is actually better.

---

_Comment by @afeblot on 2024-10-16 22:13_

Was coming here to suggest/ask for this feature, and found it just released already. I love you :-) 

---

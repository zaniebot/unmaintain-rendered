```yaml
number: 19233
title: "[ty] Add a `--quiet` mode"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: zb/printer
created_at: 2025-07-09T12:30:30Z
updated_at: 2025-07-10T14:40:49Z
url: https://github.com/astral-sh/ruff/pull/19233
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Add a `--quiet` mode

---

_@zanieb_

Adds a `--quiet` flag which silences diagnostic, warning logs, and messages like "all checks passed" while retaining summary messages that indicate problems, e.g., the number of diagnostics.

I'm a bit on the fence regarding filtering out warning logs, because it can omit important details, e.g., the message that a fatal diagnostic was encountered. Let's discuss that in https://github.com/astral-sh/ruff/pull/19233#discussion_r2195408693

The implementation recycles the `Printer` abstraction used in uv, which is intended to replace all direct usage of `std::io::stdout`. See https://github.com/astral-sh/ruff/pull/19233#discussion_r2195140197

I ended up futzing with the progress bar more than I probably should have to ensure it was also using the printer, but it doesn't seem like a big deal. See https://github.com/astral-sh/ruff/pull/19233#discussion_r2195330467

Closes https://github.com/astral-sh/ty/issues/772

---

_Label `ty` added by @zanieb on 2025-07-09 12:30_

---

_Renamed from "Add a `--quiet` mode" to "[ty] Add a `--quiet` mode" by @zanieb on 2025-07-09 12:30_

---

_@zanieb reviewed on 2025-07-09 12:33_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:391 on 2025-07-09 12:33_

I need to figure out why https://github.com/astral-sh/ruff/pull/18049 was a problem here but not in uv, otherwise this could be a regression.

---

_@zanieb reviewed on 2025-07-09 14:09_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:8 on 2025-07-09 14:09_

The `Printer` abstraction is lifted from uv, but is a little different here because we reuse the existing `VerbosityLevel` enum, support locking the stream, and don't need stderr support yet.

---

_@zanieb reviewed on 2025-07-09 14:10_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:59 on 2025-07-09 14:10_

Initially I wrote a `stderr` handler too, but we don't actually need it here yet.

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:88 on 2025-07-09 14:13_

We don't support locking in uv's printer, but in ty we lock stdout for the hot loop of printing diagnostics which seems reasonable and worth retaining.

---

_@zanieb reviewed on 2025-07-09 14:13_

---

_@zanieb reviewed on 2025-07-09 14:13_

---

_Review comment by @zanieb on `crates/ty_project/src/lib.rs`:116 on 2025-07-09 14:13_

I just renamed this for clarity, as I initially thought this was used to report more than progress.

---

_@zanieb reviewed on 2025-07-09 15:27_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:48 on 2025-07-09 15:27_

I don't love this name, but it's a simple way to handle things for now. I could see us doing things like `stream_for_summary`, `stream_for_diagnostics`, etc. in the future?

---

_@zanieb reviewed on 2025-07-09 15:30_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:403 on 2025-07-09 15:30_

Annoyingly, we want to defer creation of the `ProgressBar` but need to be able to take ownership of the `ProgressDrawTarget` so we wrap these in `Option` types. We could probably do something else, but this was the easiest solution for now.

---

_@zanieb reviewed on 2025-07-09 15:32_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:393 on 2025-07-09 15:32_

I got a bit in the weeds here handling the progress bar. We have handling for it in uv, and I thought it'd be reasonable to copy over. Arguably, we could just cut scope here and continue to use the `DummyReporter`, but I think it's nice to consolidate the logic for showing and hiding output in the main binary.

---

_Comment by @github-actions[bot] on 2025-07-09 15:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:04_

This may actually be too much? Or we might want to use error level logs for things like the "a fatal diagnostic was encountered"? I don't have strong feelings here, it just seemed like the most obvious level to use.

---

_@zanieb reviewed on 2025-07-09 16:04_

---

_@zanieb reviewed on 2025-07-09 16:06_

---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:06_

It's also plausible this problem is obviated by using a `warn_user` system for pretty-printing important user-facing warnings.

---

_@MichaReiser reviewed on 2025-07-09 16:15_

---

_Review comment by @MichaReiser on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:15_

> It's also plausible this problem is obviated by using a warn_user system for pretty-printing important user-facing warnings.

We can't use a `warn_user` like system because those wouldn't show up in the LSP. That's why we use Diagnostics for almost everything and `warn` only for cases where something unexpected went wrong but we can recover from.

---

_@zanieb reviewed on 2025-07-09 16:19_

---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:19_

The warning I'm talking about though is in the CLI though. (I think that's further evidence that this level is fine and we should just consider tweaking that one warning if needed)

---

_@MichaReiser reviewed on 2025-07-09 16:24_

---

_Review comment by @MichaReiser on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:24_

Oh sorry. I only commented on `warn_user` specifically. 

>  Or we might want to use error level logs for things like the "a fatal diagnostic was encountered"? I don't have strong feelings here, it just seemed like the most obvious level to use.

I think suppressing said message when using `--quiet` is fine or we should make it an error. The reason I picked warning is because it was mainly meant as a hint that you have to scroll all the way to the top to see that there are panic diagnostics.

---

_@zanieb reviewed on 2025-07-09 16:45_

---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:87 on 2025-07-09 16:45_

Alright, I won't worry about it.

---

_Marked ready for review by @zanieb on 2025-07-09 16:45_

---

_Review requested from @carljm by @zanieb on 2025-07-09 16:45_

---

_Review requested from @AlexWaygood by @zanieb on 2025-07-09 16:45_

---

_Review requested from @sharkdp by @zanieb on 2025-07-09 16:45_

---

_Review requested from @dcreager by @zanieb on 2025-07-09 16:45_

---

_@zanieb reviewed on 2025-07-09 16:50_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:403 on 2025-07-09 16:50_

Just for the record, I tried to use `Either` to avoid two `Options`, but couldn't find a way to take ownership of the wrapped value and didn't want to deal with changing the signature in the trait.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-09 16:57_

---

_Comment by @MichaReiser on 2025-07-09 20:16_

I plan on reviewing this tomorrow morning 

---

_Label `cli` added by @MichaReiser on 2025-07-10 05:59_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:324 on 2025-07-10 06:03_

Rendering diagnostics is fairly expensive (especially when using the `full` format). We should avoid iterating and rendering all diagnostics if we end up discarding the output anyway

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:403 on 2025-07-10 06:14_

Yeah, it's a bit awkward with `set_files` taking a `&mut`. 

The following works. You could also change `Pending` to `Pending(Option<IndicatifDrawTarget>)`

```rust
/// A progress reporter for `ty check`.
enum IndicatifReporter {
    Pending(indicatif::ProgressDrawTarget),

    Initialized(indicatif::ProgressBar),
}

impl From<Printer> for IndicatifReporter {
    fn from(printer: Printer) -> Self {
        Self::Pending(printer.progress_target())
    }
}

impl ty_project::ProgressReporter for IndicatifReporter {
    fn set_files(&mut self, files: usize) {
        let target = match std::mem::replace(
            self,
            IndicatifReporter::Pending(indicatif::ProgressDrawTarget::hidden()),
        ) {
            Self::Pending(target) => target,
            Self::Initialized(_) => panic!("The progress reporter should only be initialized once"),
        };

        let bar = indicatif::ProgressBar::with_draw_target(Some(files as u64), target);
        bar.set_style(
            indicatif::ProgressStyle::with_template(
                "{msg:8.dim} {bar:60.green/dim} {pos}/{len} files",
            )
            .unwrap()
            .progress_chars("--"),
        );
        bar.set_message("Checking");
        *self = Self::Initialized(bar)
    }

    fn report_file(&self, _file: &ruff_db::files::File) {
        match self {
            IndicatifReporter::Initialized(progress_bar) => {
                progress_bar.inc(1);
            }
            IndicatifReporter::Pending(_) => {
                panic!("`report_file` called before `set_files`")
            }
        }
    }
}
```

Both solutions aren't really different to what you have. The only difference is that they encode the contract a bit more explicitly. I'm fine with any implementation

---

_Review comment by @MichaReiser on `crates/ty/src/logging.rs`:38 on 2025-07-10 06:15_

I'd prefer to keep this as a `bool` flag. It seems more confusing to users if they can use `-qq` but it doesn't change the behavior in anyway

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:310 on 2025-07-10 06:16_

Shouldn't this be printed according to the documentation of quiet:

> Silences output except for summary information.

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:48 on 2025-07-10 06:19_

I'm inclined to go with `stream_for_summary` as those are the main messages that should still be written when `-q` is used

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:162 on 2025-07-10 06:22_

I think this should implement `std::io::Write` which is what rust's Stdout implements 

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:85 on 2025-07-10 06:24_

Should this be a no-op if the `Stdout` already holds the lock? An alternative is to do what Rust's stdout type does and have a separate `StdoutLock` type that `lock` returns. This avoids this question altogether because `StdoutLock` doesn't provide a lock method.

---

_@MichaReiser approved on 2025-07-10 06:25_

Thanks. There are three changes we should make before landing but this otherwise looks good to me.

* We should avoid rendering the diagnostics when running in `--quiet` because that's a lot of unnecessary work otherwise
* Disallow `-qq` (and variance) until we actually do support other quiet modes
* I think there's one summary message that now gets omitted that should be rendered as part of `--quiet`

---

_@zanieb reviewed on 2025-07-10 11:25_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:48 on 2025-07-10 11:25_

Works for me.

---

_@zanieb reviewed on 2025-07-10 11:26_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:403 on 2025-07-10 11:26_

I like the encoded contract better, thanks for the suggestion.

---

_@zanieb reviewed on 2025-07-10 11:27_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:310 on 2025-07-10 11:27_

Fair, but I think the documentation should be changed there. I think a "success" message is not important enough to display under `--quiet`.

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:162 on 2025-07-10 11:29_

I copied this from uv (ref https://github.com/astral-sh/uv/pull/2227) maybe it's because we're using anstream over there? I'm fine with changing it if there aren't other problems, I wrote both initially.

---

_@zanieb reviewed on 2025-07-10 11:29_

---

_@zanieb reviewed on 2025-07-10 11:30_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:85 on 2025-07-10 11:30_

I'm not sure. We'd just drop the old lock (which would release it) and take a new one, which seems fine?

I didn't want to introduce more types unless it mattered.

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:310 on 2025-07-10 11:35_

How is it different from *Found x diagnostics*. It captures the same information when x is 0

---

_@MichaReiser reviewed on 2025-07-10 11:35_

---

_@zanieb reviewed on 2025-07-10 11:35_

---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:38 on 2025-07-10 11:35_

I'd like to push back on that, I think we'll want a `--qq` silent mode to match uv and this is forwards compatible. You can also provide `-vvvvvvv` and it doesn't do anything more.

---

_@MichaReiser reviewed on 2025-07-10 11:36_

---

_Review comment by @MichaReiser on `crates/ty/src/printer.rs`:85 on 2025-07-10 11:36_

I suspect this will dead lock because it constructs the new lock before dropping the old lock

---

_@zanieb reviewed on 2025-07-10 11:41_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:310 on 2025-07-10 11:41_

I think it's important to omit this message given our previous thread on the message in Ruff https://github.com/astral-sh/ruff/pull/8631#issuecomment-2013230885

---

_@zanieb reviewed on 2025-07-10 11:43_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:48 on 2025-07-10 11:43_

The reason I didn't do this initially is because `stream_for_summary` doesn't really fit the use case for something like the version command

---

_@zanieb reviewed on 2025-07-10 11:55_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:48 on 2025-07-10 11:55_

(I'll come up with some names though)

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:85 on 2025-07-10 11:55_

That seems fairly easy to fix, but is a good point.

---

_@zanieb reviewed on 2025-07-10 11:55_

---

_@MichaReiser reviewed on 2025-07-10 11:57_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:310 on 2025-07-10 11:57_

Fair enough. Maybe `stream_for_summary` isn't such a good name ;)

It might be good to add some documentation to `stdout_important` for when it should be used

---

_@zanieb reviewed on 2025-07-10 11:58_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:310 on 2025-07-10 11:58_

Because a message on success contains no information that isn't represented by the exit code, but on failure the message captures the number of errors?

---

_@MichaReiser reviewed on 2025-07-10 12:41_

---

_Review comment by @MichaReiser on `crates/ty/src/logging.rs`:38 on 2025-07-10 12:41_

>  I think we'll want a --qq silent mode

What does this do? Does it silence everything? 

I don't feel too strongly. I just think both approaches are forward compatible and I'm not entirely sure yet if we indeed want `-qq` in the future (because the use case isn't clear to me not because I'm oposed to it)

---

_@MichaReiser reviewed on 2025-07-10 14:15_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:327 on 2025-07-10 14:15_

We should move this check outside the `for` loop as the result is always the same for all diagnostics.

---

_@MichaReiser approved on 2025-07-10 14:16_

Thank you

---

_@MichaReiser reviewed on 2025-07-10 14:17_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:327 on 2025-07-10 14:17_

Oh, never mind. It's in here because of the `max_severity` check below

---

_@zanieb reviewed on 2025-07-10 14:38_

---

_Review comment by @zanieb on `crates/ty/src/lib.rs`:327 on 2025-07-10 14:38_

Yep haha, that broke the tests and I was like.. oh mhm

---

_@zanieb reviewed on 2025-07-10 14:38_

---

_Review comment by @zanieb on `crates/ty/src/printer.rs`:162 on 2025-07-10 14:38_

This is non-trivial so we dropped it.

---

_@zanieb reviewed on 2025-07-10 14:40_

---

_Review comment by @zanieb on `crates/ty/src/logging.rs`:38 on 2025-07-10 14:40_

Yeah, "silent" mode.

It saves you from piping the command's output to `/dev/null` which is error prone in various shells (as we've discovered in our install scripts).

---

_Merged by @zanieb on 2025-07-10 14:40_

---

_Closed by @zanieb on 2025-07-10 14:40_

---

_Branch deleted on 2025-07-10 14:40_

---

```yaml
number: 5769
title: "feat(mangen): Support flatten_help"
type: pull_request
state: open
author: aparcar
labels: []
assignees: []
base: master
head: flat-man
created_at: 2024-10-07T09:11:58Z
updated_at: 2025-07-10T15:41:59Z
url: https://github.com/clap-rs/clap/pull/5769
synced_at: 2026-01-12T16:14:17Z
```

# feat(mangen): Support flatten_help

---

_@aparcar_

The `flatten_help` argument combines all subcommands on a single page. Until now this wasn't supported for `mangen`. With this command the sections `SYNOPSIS` as well as `(SUB)COMMANDS` are changed to imitate the style of `git stash --help`.

I reached out via discussions and it looks like this doesn't work just yet https://github.com/clap-rs/clap/discussions/5761

This is not necessarily the most beautiful implementation due to my limited knowledge of both `clap` and Rust itself. 

I implemented this for a project I'm working on and the results look quite okay:

![image](https://github.com/user-attachments/assets/30930387-c975-41fd-ac0e-36d735e3af95)

Not using `man.render()` but call each command manually feels a bit awkward but does work just fine, see example here https://github.com/rosenpass/rosenpass/pull/434/files#diff-352422e105afd3138a54ea14e33af782243398f1d768d271655729dd4bcb99d2R18

---

_@epage reviewed on 2024-10-07 13:40_

---

_Review comment by @epage on `clap_mangen/src/lib.rs`:224 on 2024-10-07 13:40_

Please add a test for this in https://github.com/clap-rs/clap/blob/master/clap_mangen/tests/testsuite/roff.rs

As mentioned in our contrib documentation, please split this into two commits
1. A commit that adds the test using `flatten_help` that passes, showing the current behavior
2. A commit that adds support for `flatten_help` and updates the test snapshot to show the new behavior

---

_@epage reviewed on 2024-10-07 13:43_

---

_Review comment by @epage on `clap_mangen/src/lib.rs`:228 on 2024-10-07 13:43_

There are times when you should sow the usage for the current command

See https://github.com/clap-rs/clap/blob/d2874a50cf594ace49a9dfa644e734f83491a403/clap_builder/src/output/usage.rs#L107-L113

In general, I'd recommend checking how we use flatten_help to change output
- https://github.com/clap-rs/clap/blob/master/clap_builder/src/output/help_template.rs
- https://github.com/clap-rs/clap/blob/master/clap_builder/src/output/usage.rs

---

_@epage reviewed on 2024-10-07 13:44_

---

_Review comment by @epage on `clap_mangen/src/lib.rs`:1 on 2024-10-07 13:44_

Please document in the PR description any style differences between `--help` and man's handling of `flatten_help`

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:224 on 2024-10-16 15:10_

Done

---

_@aparcar reviewed on 2024-10-16 15:10_

---

_@aparcar reviewed on 2024-10-16 15:10_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:228 on 2024-10-16 15:10_

I tried to adopt the style

---

_@aparcar reviewed on 2024-10-16 15:19_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:99 on 2024-10-16 15:19_

I'm quite sure you don't love this way of implementing it, do you have a cleaner idea?

---

_@aparcar reviewed on 2024-10-16 16:08_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:1 on 2024-10-16 16:08_

Before documenting it I want to make sure that the `flatten_help` implementation for `--help` isn't incomplete. In the picture below are subcommand details printed directly after the options without another section title, is that intended?

<img width="754" alt="image" src="https://github.com/user-attachments/assets/d1412005-3587-47d6-ad42-c8b7e1f7766c">


---

_Review comment by @epage on `clap_mangen/src/lib.rs`:1 on 2024-10-16 22:37_

>  In the picture below are subcommand details printed directly after the options without another section title, is that intended?

Yes, that is intentional as we don't really have a good way of rendering sub-headers at this time.

---

_@epage reviewed on 2024-10-16 22:37_

---

_@epage reviewed on 2024-10-16 22:39_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:129 on 2024-10-16 22:39_

Please use `flatten_help.roff` in this commit.  That way we can see how `flatten_help.roff` itself changes between commits.

---

_Review comment by @epage on `clap_mangen/src/render.rs`:87 on 2024-10-16 22:44_

Why are we manually building this, rather than using `subcommand.get_bin_name().unwrap_or_else(|| cmd.get_name());`

---

_@epage reviewed on 2024-10-16 22:44_

---

_@epage reviewed on 2024-10-16 22:45_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:93 on 2024-10-16 22:45_

`to_string` can lead to sloppy code. Please instead use `to_owned` or whatever the appropriate conversion is

---

_@epage reviewed on 2024-10-16 22:45_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:87 on 2024-10-16 22:45_

With that change, do we even need a `name` parameter?

---

_@epage reviewed on 2024-10-16 22:47_

---

_Review comment by @epage on `clap_mangen/src/lib.rs`:228 on 2024-10-16 22:47_

I am  not seeing a call to `is_subcommand_required_set` or `is_args_conflicts_with_subcommands_set` to handle this

---

_@epage reviewed on 2024-10-16 22:48_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:111 on 2024-10-16 22:48_

Seems like we should be consistent between `flatten` or not on whether the last entry has a `br` after it.  Since we don't have one today, it appears, it seems like we shouldn't in this.  If we wanted to add it, that would be a separate commit

---

_@epage reviewed on 2024-10-16 22:52_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:32 on 2024-10-16 22:52_

For consistency, shouldn't this be called `usage`?

---

_@epage reviewed on 2024-10-16 22:53_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:32 on 2024-10-16 22:53_

Please split out the commits for
- Splitting this function out
- Moving this function below `synopsis` (also, I'm assuming it doesn't need to be `pub(crate)`)

---

_@epage reviewed on 2024-10-16 22:55_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:99 on 2024-10-16 22:55_

where possible, we should match how our regular usage renderer does things

---

_@aparcar reviewed on 2024-10-17 13:00_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:32 on 2024-10-17 13:00_

Done

---

_@aparcar reviewed on 2024-10-17 13:00_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:32 on 2024-10-17 13:00_

Done

---

_@aparcar reviewed on 2024-10-17 13:00_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:111 on 2024-10-17 13:00_

Fixed

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:93 on 2024-10-17 13:00_

Fixed

---

_@aparcar reviewed on 2024-10-17 13:01_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:87 on 2024-10-17 13:01_

Cleaned up 

---

_@aparcar reviewed on 2024-10-17 13:01_

---

_@aparcar reviewed on 2024-10-17 13:01_

---

_Review comment by @aparcar on `clap_mangen/tests/testsuite/roff.rs`:129 on 2024-10-17 13:01_

Fixed

---

_@aparcar reviewed on 2024-10-17 13:14_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:99 on 2024-10-17 13:14_

I changed the style to be less invasive, what do you think?

---

_@aparcar reviewed on 2024-10-18 09:31_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:228 on 2024-10-18 09:31_

Added

---

_@aparcar reviewed on 2024-10-21 12:46_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:1 on 2024-10-21 12:46_

I tried to make a nice comparison but I'm a bit confused with the following issue. To allow `--help` on the `man.rs` example, I made the following change:

```diff
diff --git a/clap_mangen/examples/man.rs b/clap_mangen/examples/man.rs
index e42c5b16..8b8e8a82 100644
--- a/clap_mangen/examples/man.rs
+++ b/clap_mangen/examples/man.rs
@@ -32,7 +32,11 @@ And a few newlines.",
             Command::new("test")
                 .about("does testing things")
                 .arg(arg!(-l --list "Lists test values")),
-        );
+        )
+        .flatten_help(true);
+
+    let _ = cmd.get_matches();
+    Ok(())
 
-    Man::new(cmd).render(&mut io::stdout())
+    // Man::new(cmd).render(&mut io::stdout())
 }
```

I now see a help output which could be compared to generated Man output, however arguments and specifically styling is entirely missing. I compared it to the `git.rs` example but couldn't find an obvious change that would drop styling and subcommand printing. Am I missing something here? 

<img width="634" alt="image" src="https://github.com/user-attachments/assets/52428ee3-567b-4d31-abdc-268666e9c7b9">


---

_@aparcar reviewed on 2024-10-21 12:51_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:1 on 2024-10-21 12:51_

Sorry for the noise, turns out I have to enable a bunch of extra clap features...

```diff
 [dependencies]
 roff = "0.2.1"
-clap = { path = "../", version = "4.0.0", default-features = false, features = ["std", "env"] }
+clap = { path = "../", version = "4.0.0", default-features = false, features = [
+    "std",
+    "env",
+    "help",
+    "usage",
+    "color",
+] }
```

---

_@aparcar reviewed on 2024-10-21 13:30_

---

_Review comment by @aparcar on `clap_mangen/src/lib.rs`:1 on 2024-10-21 13:30_

Added to commit message, also please see https://github.com/clap-rs/clap/pull/5769#issuecomment-2426691534

---

_Review comment by @epage on `clap_mangen/src/render.rs`:68 on 2024-10-22 18:08_

something is off about this commit

---

_@epage reviewed on 2024-10-22 18:08_

---

_@epage reviewed on 2024-10-22 18:09_

---

_Review comment by @epage on `clap_mangen/src/lib.rs`:1 on 2024-10-22 18:09_

> Differences between flattened `--help` and the flattened man:
> * `--help` prints the description at the very beginning while Man uses a
>   section called DESCRIPTION below the USAGE.
> * `--help` prints placeholder `[OPTIONS]` while Man prints all options
> * `--help` prints positional options (aka arguments) in its own section
>   called arguments while Man prints them at the end of OPTIONS.
> * `--help` prints subcommands as their own section after the options
>   section while Man uses an extra section called SUBCOMMANDS
> * `--help` prints `after_long_help` at the very end while Man uses the
>   section EXTRA which comes before the sections VERSION and AUTHORS

Most of these differences sound unrelated to flattening

---

_@epage reviewed on 2024-10-22 18:10_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:151 on 2024-10-22 18:10_

Please add these in the test commit

---

_@epage reviewed on 2024-10-22 18:12_

---

_Review comment by @epage on `clap_mangen/tests/snapshots/flatten_help.roff`:29 on 2024-10-22 18:12_

Hmm, this surprised me at first but I guess this is what git does for `git help stash`

---

_@epage reviewed on 2024-10-22 18:15_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:118 on 2024-10-22 18:15_

We should probably cover more cases that `help` covers, see https://github.com/clap-rs/clap/blob/61f5ee514f8f60ed8f04c6494bdf36c19e7a8126/tests/builder/help.rs#L3280-L3971

e.g.
- we don't need to cover short vs long
- we should cover recursion

It'd be a big help to reuse the same commands as those tests so we can easily compare the help output vs man output

---

_Review comment by @epage on `clap_mangen/src/render.rs`:69 on 2024-10-22 18:20_

The way this is structured doesn't make the intent clear, e.g.
```suggestion
    let flatten = cmd.is_flatten_help_set();
    let mut first = true;

    if !cmd.is_subcommand_required_set() || cmd.is_args_conflicts_with_subcommands_set() {
        let mut line = usage(cmd, cmd.get_bin_name().unwrap_or_else(|| cmd.get_name()));
        if cmd.has_subcommands() && !flatten {
            let (lhs, rhs) = subcommand_markers(cmd);
            line.push(roman(lhs));
            line.push(italic(
                cmd.get_subcommand_value_name()
                    .unwrap_or_else(|| subcommand_heading(cmd))
                    .to_lowercase(),
            ));
            line.push(roman(rhs));
        }
        roff.text(line);

        first = false;
    }

    if flatten {
        let mut ord_v = Vec::new();
        for subcommand in cmd.get_subcommands() {
            ord_v.push((
                subcommand.get_display_order(),
                subcommand.get_bin_name().unwrap_or_else(|| cmd.get_name()),
                subcommand,
            ));
        }
        ord_v.sort_by(|a, b| (a.0, &a.1).cmp(&(b.0, &b.1)));
        for (_, name, cmd) in ord_v {
            if !first {
                roff.control("br", []);
            } else {
                first = false;
            }
            roff.text(usage(cmd, name));
        }
    }
}
```
- groups `ord_v` declaration with where its used, making the intent for where its relevant clearr
- groups the flatten blocks together to make it clear that they are coupled

---

_@epage reviewed on 2024-10-22 18:20_

---

_@epage reviewed on 2024-10-22 18:22_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:87 on 2024-10-22 18:22_

This is not addressed.  We are still passing in the `name` and still using `get_name`

---

_@aparcar reviewed on 2024-10-23 09:17_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:68 on 2024-10-23 09:17_

It's marked as outdated, I don't see this anymore.

---

_@aparcar reviewed on 2024-10-23 09:17_

---

_Review comment by @aparcar on `clap_mangen/tests/snapshots/flatten_help.roff`:29 on 2024-10-23 09:17_

So nothing to do for me here?

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:69 on 2024-10-23 09:20_

Applied

---

_@aparcar reviewed on 2024-10-23 09:20_

---

_@aparcar reviewed on 2024-10-23 09:32_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:87 on 2024-10-23 09:32_

This change no longer exists, it's marked outdated

---

_@aparcar reviewed on 2024-10-23 09:33_

---

_Review comment by @aparcar on `clap_mangen/tests/testsuite/roff.rs`:151 on 2024-10-23 09:33_

Done

---

_Review comment by @aparcar on `clap_mangen/tests/testsuite/roff.rs`:118 on 2024-10-23 10:19_

I added all relevant tests from `help.rs` and at least the recursive stuff isn't working right now. Is the recursive stuff strictly required for this PR to move forward?

---

_@aparcar reviewed on 2024-10-23 10:19_

---

_@epage reviewed on 2024-10-23 21:16_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:342 on 2024-10-23 21:16_

Please add these tests like the other ones, in a commit before the feature work

---

_@epage reviewed on 2024-10-23 21:16_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:144 on 2024-10-23 21:16_

Are these tests still needed with the addition of the `--help` tests?

---

_@epage reviewed on 2024-10-23 21:17_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:68 on 2024-10-23 21:17_

I'm still seeing it.

---

_@epage reviewed on 2024-10-23 21:19_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:87 on 2024-10-23 21:19_

Whether the lines this originally applied to are there or not, the problem is still there.

---

_@epage reviewed on 2024-10-23 21:21_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:93 on 2024-10-23 21:21_

There are other `to_string` calls but I believe they are needed as they should be stripping ANSI escape codes

---

_@epage reviewed on 2024-10-23 21:23_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:118 on 2024-10-23 21:23_

The recursive behavior is part of the definition of `flatten_help`.  We could possibly split it out but we'd need to be tracking it somehow.  Its likely best to go ahead and do that as it might affect the design in important ways

---

_@aparcar reviewed on 2025-02-10 19:55_

---

_Review comment by @aparcar on `clap_mangen/src/render.rs`:68 on 2025-02-10 19:55_

Should be fixed

---

_@epage reviewed on 2025-07-10 15:39_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:58 on 2025-07-10 15:39_

This isn't recursive but should be (same with `flat_subcommands`)

---

_@epage reviewed on 2025-07-10 15:39_

---

_Review comment by @epage on `clap_mangen/tests/testsuite/roff.rs`:118 on 2025-07-10 15:39_

Split out the recursion conversation to https://github.com/clap-rs/clap/pull/5769/commits/11d560f1f74a7ce3b91cfad6417f302b7d3ff187#r2198095292

---

_@epage requested changes on 2025-07-10 15:41_

See https://github.com/clap-rs/clap/pull/5769#issuecomment-3057982003

---

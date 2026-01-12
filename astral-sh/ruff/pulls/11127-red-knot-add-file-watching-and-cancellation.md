```yaml
number: 11127
title: "Red knot: Add file watching and cancellation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: red-knot-file-watching
created_at: 2024-04-24T13:23:32Z
updated_at: 2024-04-25T07:54:57Z
url: https://github.com/astral-sh/ruff/pull/11127
synced_at: 2026-01-12T15:55:34Z
```

# Red knot: Add file watching and cancellation

---

_@MichaReiser_

## Summary

Okay, I must admit this PR does more than what the title says. 

The changes not covered by the title are:

1. It adds a new `lint_syntax` method that performs two checks. It mainly serves to see if file changes are correctly picked up. The implemented checks are:
  1. Whether any line contains more than 88 characters (yeah, it should be the line width, but I didn't bother for the prototype)
  2. Whether any string literal uses single quotes instead of double quotes (again, a very dumb implementation. It even flags strings that contain double quotes, which normally would be considered okay)
2. It changes `Source` to use `SourceKind` instead of always an `Arc<str>`. This is necessary to support IPython notebooks. There's absolutely no good reason why this is part of this PR and I'm happy to split it out if people find that useful. I think it doesn't matter much.


The main change of the PR is that red-knot now accepts an "entry path" and runs the analysis in watch mode. This is interesting because it tackles a few challenges:

* How to wait for file changes or until the user presses Ctrl+C without using a spin loop. That's not conceptually difficult, but it's something I've never done before, so it took me a few approaches to get it right.
* How can we ensure that we get a mutable reference to `program` after all scheduled analysis have been completed
* How to cancel pending analysis. 

I'm undecided if the ultimate goal should be that this code is isolated and reused across the LSP, CLI watch mode and regular CLI. It would certainly be nice, but I could also see this be part of the "Host" specific code, and each host should have as much flexibility as possible to implement the most efficient scheduling for its needs.

Future Improvements:

* We should ignore changes to files that are ignored in the settings and isn't a dependency of Program
* The current error handling is mostly unwrapping. We should do better. 
* File watching...: Correctly handle folders, new files, cases where a user deletes a file and creates a folder with the same name etc.
* The invalidation logic requires more love, but I don't think it's important to fully implement it right now because the PR proves the important point, that we can get a mutable Program (db)


## Test plan

I ran the CLI locally, then made changes to the test file by adding a new single quoted string and I then verified that the console showed the new diagnostic.

---

_Label `internal` added by @MichaReiser on 2024-04-24 13:40_

---

_Marked ready for review by @MichaReiser on 2024-04-24 13:40_

---

_Comment by @MichaReiser on 2024-04-24 13:41_

CC: @BurntSushi for the case I do anything horrible inside of `main.rs` 

---

_Comment by @github-actions[bot] on 2024-04-24 13:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-04-24 15:46_

Okay, I think what I want to do tomorrow is:

* Figure out if we can move the orchestration thread out of the main loop. I suspect that this should be straightforward by adding one more channel :laughing: 
* Add a `program.check(context)` and `program.check_file(id, context)` functions and move the logic for checking a program there. The `context` would expose a method to schedule a dependency so that it's up to the host to decide if that should run on the same thread (queuing) or on a thread pool.

Edit: I think I should also be able to move the `Arc<AtomicUsize>` counting the pending analysis into the orchestrator by adding a new `StartAnalysis` message (it means some more messaging but it allows isolating the logic and I could even use a regular `usize`)

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:35 on 2024-04-24 22:11_

I guess maybe the parsing errors should be raised as diagnostics here? Could just be a TODO

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:67 on 2024-04-24 22:15_

Obviously there are a couple things in this file that are just prototype stubs; is it useful to include TODO comments to remind ourselves later where those things are?

---

_Review comment by @carljm on `crates/red_knot/src/program.rs`:46 on 2024-04-24 22:40_

Now that we cache symbol tables, they should also be removed here?

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:278 on 2024-04-24 22:42_

```suggestion
                        // Deletion after creation means that ruff never saw the file.
```

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:294 on 2024-04-24 22:43_

maybe better to assert here if we really think it shouldn't happen?

---

_Review comment by @carljm on `crates/red_knot/src/watch.rs`:62 on 2024-04-24 22:48_

if we at least know error handling will change the signature (e.g. should `FileWatcher::from_handler` return a `Result`?) it might be good to at least reflect that in the signature sooner rather than later, since it will affect other code?

---

_@carljm approved on 2024-04-24 22:50_

Very cool! Sorry I didn't see this / review it sooner so you could have landed it today.

I didn't carefully follow every detail in `main.rs`; hoping that as you mentioned some of that will get broken up and be a little easier to follow.

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:67 on 2024-04-25 07:28_

I don't think so. We'll replace the implementation with the actual rules from ruff. 

---

_@MichaReiser reviewed on 2024-04-25 07:28_

---

_@MichaReiser reviewed on 2024-04-25 07:30_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:294 on 2024-04-25 07:30_

Yeah, not sure. I don't think it's something I want to drip Ruff over. I think we probably want a `Change` that means `anything` happened to the file so that we can take the most defensive invalidation for that file.

---

_Comment by @MichaReiser on 2024-04-25 07:33_

> Very cool! Sorry I didn't see this / review it sooner so you could have landed it today.

All good. I'm not in a rush. Having the review next morning is still a perfectly quick review and I should have tagged you as reviewer! Thank you

---

_Merged by @MichaReiser on 2024-04-25 07:54_

---

_Closed by @MichaReiser on 2024-04-25 07:54_

---

_Branch deleted on 2024-04-25 07:54_

---

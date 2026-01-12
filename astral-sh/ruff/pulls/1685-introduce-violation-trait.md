```yaml
number: 1685
title: Introduce Violation trait
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: violation-trait
created_at: 2023-01-06T07:38:57Z
updated_at: 2023-01-07T20:28:46Z
url: https://github.com/astral-sh/ruff/pull/1685
synced_at: 2026-01-12T05:36:32Z
```

# Introduce Violation trait

---

_Pull request opened by @not-my-profile on 2023-01-06 07:38_

For every available rule registry.rs currently defines:

1. A CheckCode variant to identify the rule.
2. A CheckKind variant to represent violations of the rule.
3. A mapping from the CheckCode variant to a placeholder CheckKind instance.
4. A mapping from the CheckKind variant to CheckCode.
5. A mapping from the CheckKind to a string description.
6. A mapping from the CheckKind to a boolean indicating if autofix is available.
7. A mapping from the CheckKind to a string describing the autofix if available.

Since registry.rs defines all of this for every single rule and
ruff has hundreds of rules, this results in the lines specific to
a particular rule to be hundreds of lines apart, making the code
cumbersome to read and edit.

This commit introduces a new Violation trait so that the rule-specific
details of the above steps 5.-7. can be defined next to the rule
implementation. The idea is that once all CheckCode/CheckKind variants
have been converted to this new approach then the steps 1.-4. in
registry.rs could simply be generated via a declarative macro, e.g:

    define_rule_mapping!(
        E501 => pycodestyle::LineTooLong,
        ...
    );

(where `pycodestyle::LineTooLong` would be a struct that implements the
new Violation trait).

The define_rule_mapping! macro would then take care of generating the
CheckCode and CheckKind enums, as well as all of the implementations for
the previously mentioned mappings, by simply calling the methods of the
new Violation trait.

There is another nice benefit from this approach: We want to introduce
more thorough documentation for rules and we want the documentation of a
rule to be defined close by the implementation (so that it's easier to
check if they match and that they're less likely to become out of sync).
Since these new Violation structs can be defined close by the
implementation of a rule we can also use them as an anchor point for the
documentation of a rule by simply adding the documentation in the form
of a Rust doc comment to the struct.



---

_Converted to draft by @not-my-profile on 2023-01-06 07:42_

---

_Comment by @not-my-profile on 2023-01-06 11:20_

I think that this new structure would also simplify contributions since you would no longer need a script (`scripts/add_check.py `) to generate the necessary boilerplate but could just copy some example code from `CONTRIBUTING.md` to some plugin file and add a single line to `registry.rs`.

And the proposed `define_rule_mapping!` macro would also address #419.

The conversion to the new structure would would require changes like the ones in my second commit for every single rule,  so this conversion would probably best be automated via a single-use Python script.

---

_Comment by @charliermarsh on 2023-01-06 12:36_

\cc @squiddy 

---

_Comment by @charliermarsh on 2023-01-06 12:43_

I think something like this based on traits and macros makes sense and is very likely where we should go (so as to get rid of the current approach in `registry.rs`, which is very unwieldy).

---

_@charliermarsh reviewed on 2023-01-06 12:45_

---

_Review comment by @charliermarsh on `src/registry.rs`:974 on 2023-01-06 12:45_

I'm feeling dumb but trying to wrap my head around why we still need the `CheckKind` enum at all in this world. I believe we do, but I'm trying to explain why to myself.

---

_Review comment by @charliermarsh on `src/registry.rs`:1184 on 2023-01-06 12:46_

So, the idea is that once we've migrated all checks, everything in this file could be generated via a macro (apart from the code-to-kind mapping)?

---

_@charliermarsh reviewed on 2023-01-06 12:46_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:203 on 2023-01-06 12:46_

We could almost certainly auto-generate these too for the initial migration.

---

_@charliermarsh reviewed on 2023-01-06 12:46_

---

_@not-my-profile reviewed on 2023-01-06 14:54_

---

_Review comment by @not-my-profile on `src/registry.rs`:974 on 2023-01-06 14:54_

We could get rid of it if we used boxed trait objects, i.e. `Box<&dyn Violation>` ... method calls on trait objects come with some overhead and boxing requires a heap allocation ... but that overhead is probably negligible. The trait would however need to be changed to be [object safe](https://doc.rust-lang.org/reference/items/traits.html#object-safety) which would mean it could no longer have `Serialize` or `DeserializeOwned` as supertraits because these traits aren't object-safe ... that could be worked around by using something like [erased-serde](https://crates.io/crates/erased-serde).

As long as we don't need to use trait objects I think continuing to use an enum is easier. And since we can automatically generate the enum along with all necessary implementations via a macro it wouldn't be as annoying anymore.

---

_Review comment by @charliermarsh on `src/registry.rs`:974 on 2023-01-06 14:55_

I agree.

---

_@charliermarsh reviewed on 2023-01-06 14:55_

---

_Review comment by @not-my-profile on `src/registry.rs`:1184 on 2023-01-06 14:57_

Yes that's very much the idea :)

---

_@not-my-profile reviewed on 2023-01-06 14:57_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:203 on 2023-01-06 14:58_

Yes that's what I meant by:

> this conversion would probably best be automated via a single-use Python script

---

_@not-my-profile reviewed on 2023-01-06 14:58_

---

_Comment by @not-my-profile on 2023-01-06 15:03_

Ok, great then I'll go ahead with attempting to write a Python script to convert everything in registry.rs to the new structure.

---

_@charliermarsh reviewed on 2023-01-06 15:12_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:162 on 2023-01-06 15:12_

I think ideally `autofix_available` and `autofix_description` would be coupled. We enforce it with a test now. (e.g., `autofix_available` is effectively `autofix_description.is_some()`).


---

_@charliermarsh reviewed on 2023-01-06 15:12_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:162 on 2023-01-06 15:12_

I think ideally `autofix_available` and `autofix_description` would be coupled. We enforce it with a test now. (e.g., `autofix_available` is effectively `autofix_description.is_some()`).


---

_@charliermarsh reviewed on 2023-01-06 15:12_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:203 on 2023-01-06 15:12_

Ah sorry, I thought this was referring specifically to the code-to-kind mapping.

---

_@charliermarsh reviewed on 2023-01-06 15:13_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:152 on 2023-01-06 15:13_

I think for now I'd prefer that these are still defined in `registry.rs`, even if they're defined as standalone structs that implement `Violation`. Wdyt?

---

_@not-my-profile reviewed on 2023-01-06 15:18_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:162 on 2023-01-06 15:18_

(I think GitHub glitched ... resolving this since the same comment is above.)

---

_@not-my-profile reviewed on 2023-01-06 15:19_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:203 on 2023-01-06 15:19_

No worries :)

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:162 on 2023-01-06 15:22_

Oh sorry, it said it failed the first time.

---

_@charliermarsh reviewed on 2023-01-06 15:22_

---

_@not-my-profile reviewed on 2023-01-06 15:37_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:162 on 2023-01-06 15:37_

Thanks for bringing that up! I agree. I think the best solution is to replace both trait methods with:

```rs
    fn autofix_formatter(&self) -> Option<fn(&Self) -> String> { None }
```

Since implementing that trait method however becomes a bit awkward since you need to define the formatter as an additional function, I think it would make sense to introduce another trait `AlwaysAutofixableViolation` that implements `Violation` via a blanket implementation ... I already implemented that, I'll force push it shortly.

---

_@not-my-profile reviewed on 2023-01-06 15:40_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:152 on 2023-01-06 15:40_

This would certainly simplify the conversion of `registry.rs` since then the conversion script wouldn't have to deal with editing multiple files / module visibility and adding imports. So yes I think for the initial conversion that makes sense ... nice idea, I didn't consider that.

We can then gradually over time move the structs & Violation impls to the other modules ... do you agree?

---

_@not-my-profile reviewed on 2023-01-06 16:18_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:152 on 2023-01-06 16:18_

I just realized that there is a slight drawback ... we have to make all the struct fields public because otherwise the other modules cannot construct the structs.

And we still have to update every `Check::new` call to construct these new structs and call `.into()`. (but we have to do that in any case)

---

_@charliermarsh reviewed on 2023-01-06 22:41_

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/ast_bool_op.rs`:152 on 2023-01-06 22:41_

Could we instead introduce a `registry.rs` in each plugin folder (like `flake8_simplify/registry.rs`), and define the checks for each plugin within that crate? Maybe make the fields `pub(crate)`? Or does that have the same issues?

---

_Comment by @not-my-profile on 2023-01-06 23:00_

Alright ... I have implemented this in 10 commits ... the commit message of the 3rd commit contains the Python code I used to generate the new structs & trait implementations (that code isn't exactly pretty but it works and it shouldn't really matter since nobody will ever run it again).

I think this is ready for review ... (you probably want to look at this commit by commit) ... once reviewed somebody will have to perform these steps again since the main branch has now advanced a couple commits since I started the process. Redoing all of this is quite a hassle and only makes sense when this PR gets merged right after and all other PRs are put on hold in the meantime.

---

_@not-my-profile reviewed on 2023-01-06 23:22_

---

_Review comment by @not-my-profile on `src/flake8_simplify/plugins/ast_bool_op.rs`:152 on 2023-01-06 23:22_

I would prefer to deal with the moving of the new structs/impls to other modules in a followup PR ... this PR is already huge.

---

_Comment by @not-my-profile on 2023-01-06 23:50_

Sidenote 1: I didn't include the `summary` function in the `Violation` trait since I don't think having it makes sense ... I think the message should be a short error message and more thorough documentation IMO belongs in the docstring of the struct (which could then also be shown by e.g. `--explain` or a dedicated web app, see also #1467).

Sidenote 2: I intentionally did not include a `category` function in the `Violation` trait since I think `CheckCode::category` is best generated completely automatically just based on the code prefixes (or probably `define_rule_mapping!`) via a proc macro.

---

_Comment by @charliermarsh on 2023-01-06 23:51_

Great, I'll try to review this soon!

---

_Comment by @charliermarsh on 2023-01-06 23:52_

(As for the merging: let's just aim to do a final rebase + force push to this branch once it's in a finished state, then merge without squashing, then I'm happy to be responsible for updating outstanding PRs. Does that sound good?)

---

_Comment by @not-my-profile on 2023-01-06 23:54_

Yes that sounds great :)

---

_Comment by @charliermarsh on 2023-01-06 23:56_

I promise not to squash :)

---

_Review comment by @charliermarsh on `src/violation.rs`:30 on 2023-01-07 00:27_

Nit: should this be `autofix_message` or `fix_message`, for consistency with `message` (and with the docstring)?

---

_Review comment by @charliermarsh on `src/violation.rs`:42 on 2023-01-07 00:28_

Is this name required to differ from `autofix_description`, to avoid a collision?

---

_Review comment by @charliermarsh on `src/registry.rs`:1323 on 2023-01-07 00:29_

I'm fine with removing `summary`. It really just existed for a few `flake8-bugbear` checks that had super long messages, and that I preserved verbatim by default.

---

_Review comment by @charliermarsh on `src/violation.rs`:1 on 2023-01-07 00:30_

Given the discussion in #1546, my understanding is that everywhere we used `Violation` in this PR should be called `Rule`, and that the `Check` struct should be renamed to violation. Is that right? Am I misunderstanding?

---

_Review comment by @charliermarsh on `src/violations.rs`:1 on 2023-01-07 00:31_

We should do some light reformatting of this file prior to merging: put all the section headers (`// pycodestyle errors`) on their own lines, add a blank line before each `define_violation!(...)`, etc.

---

_Review comment by @charliermarsh on `src/registry.rs`:861 on 2023-01-07 00:32_

I think `MockReference`, `Branch`, etc. (the enums used as members on the `Violation` structs) should be moved out of this file.

---

_Review comment by @charliermarsh on `src/registry.rs`:1211 on 2023-01-07 00:35_

I'm having trouble commenting in the right spots due to the size of the PR, but do you think `lint_source` and `category` should eventually be part of the `Violation` trait? It might be hard to move `lint_source` right now because we need a way to go from "enabled codes" to "sources of lint to invoke", and going from "enabled code" to `Violation` is an awkward operation (since it requires populating fields with placeholders -- although maybe this is doable now via associated functions?).


---

_@charliermarsh reviewed on 2023-01-07 00:35_

This looks really nice! I'm happy with the direction here. Thank you for all the work you put into this.

---

_Review comment by @not-my-profile on `src/violation.rs`:30 on 2023-01-07 06:00_

Good point. I think I favor `description` over `message` because "message" begs the question "message to whom and about what?" So I think I'd rather rename `message` to `description`, to have `description` and `autofix_description`.

---

_@not-my-profile reviewed on 2023-01-07 06:00_

---

_@not-my-profile reviewed on 2023-01-07 06:02_

---

_Review comment by @not-my-profile on `src/violation.rs`:42 on 2023-01-07 06:02_

No that's not necessary, trait method calls can be qualified like `<MyStruct as MyTrait>.foobar()`. However since the function does something different, I really think it should be called something different.

---

_@not-my-profile reviewed on 2023-01-07 06:37_

---

_Review comment by @not-my-profile on `src/violation.rs`:1 on 2023-01-07 06:37_

Yes I think you are misunderstanding.

The `self` in the trait refers to a specific violation, so I think it makes sense to also name the trait `Violation`.
This also means we cannot name the `Check` struct "Violation" as well since that would lead to [E0252](https://doc.rust-lang.org/stable/error_codes/E0252.html).

I agree that `Check` should be renamed ... I am starting to think that `Diag` (short for Diagnostic) would make sense. I just elaborated a bit more in a comment on the issue: https://github.com/charliermarsh/ruff/issues/1546#issuecomment-1374393270

---

_@not-my-profile reviewed on 2023-01-07 06:44_

---

_Review comment by @not-my-profile on `src/violations.rs`:1 on 2023-01-07 06:44_

Good point ... I can adapt the Python script of the third commit to take care of that.

---

_@not-my-profile reviewed on 2023-01-07 06:45_

---

_Review comment by @not-my-profile on `src/registry.rs`:861 on 2023-01-07 06:45_

I agree but I'd like to leave moving stuff around between modules to followup PRs.

---

_@not-my-profile reviewed on 2023-01-07 06:53_

---

_Review comment by @not-my-profile on `src/violation.rs`:30 on 2023-01-07 06:53_

Although looking at the Language Server Protocol it does have `Diagnostic.message` ... so message does probably make sense ... LSP also has `CodeAction.title` so we could name the second function `autofix_title`. WDYT?

---

_@not-my-profile reviewed on 2023-01-07 07:08_

---

_Review comment by @not-my-profile on `src/registry.rs`:1211 on 2023-01-07 07:08_

Yes I think it probably makes sense to add `lint_source` to the `Violation` trait ... since the `lint_source` function has so few match arms, doing that manually in the future shouldn't pose any issues. And yes since the Violation trait isn't object-safe anyway we can easily add functions that don't take `&self` and therefore can be called without instantiation.

I don't like the name "category", [see my comment here](https://github.com/charliermarsh/ruff/issues/1546#issuecomment-1374397617). I'd rather call the function `origin`. In any case that function can be completely autogenerated just based on the code prefixes, so I think it makes more sense to generate it via a proc macro instead of adding it to the trait.

---

_Comment by @not-my-profile on 2023-01-07 07:23_

@charliermarsh I'm starting the rebase.

---

_Marked ready for review by @not-my-profile on 2023-01-07 08:05_

---

_@not-my-profile reviewed on 2023-01-07 08:11_

---

_Review comment by @not-my-profile on `src/violation.rs`:30 on 2023-01-07 08:11_

Changed to `autofix_title` ... I think it nicely emphasizes that the description should not be too detailed.

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-07 08:24_

---

_Comment by @not-my-profile on 2023-01-07 16:05_

Let me know when you want to merge this, then I can rebase the PR for a final time :)

---

_@charliermarsh reviewed on 2023-01-07 16:24_

---

_Review comment by @charliermarsh on `src/registry.rs`:861 on 2023-01-07 16:24_

Ok, no prob.

---

_@charliermarsh reviewed on 2023-01-07 16:26_

---

_Review comment by @charliermarsh on `src/violation.rs`:1 on 2023-01-07 16:26_

Ah, right, ok. This makes sense. Does "rule" exist in the codebase at all then? Would a "rule" be the function that takes in the source code data (AST nodes, tokens, whatever) and generates diagnostics?

---

_@charliermarsh reviewed on 2023-01-07 16:31_

---

_Review comment by @charliermarsh on `src/violation.rs`:30 on 2023-01-07 16:31_

Yeah that sounds reasonable. I'm happy to mirror the LSP conventions where they map nicely.

---

_@charliermarsh reviewed on 2023-01-07 16:31_

---

_Review comment by @charliermarsh on `src/registry.rs`:1211 on 2023-01-07 16:31_

Origin is fine.


---

_Comment by @charliermarsh on 2023-01-07 16:32_

Cool, let's just sort out the final semantics around Violation vs. Diagnostic vs. Rule, then hopefully we can get this merged some time this weekend (or whenever you're available, no pressure).

---

_Comment by @charliermarsh on 2023-01-07 16:33_

(I also want to do a quick sanity check to ensure there are no major perf changes prior to merging. I don't expect any, but it's a large change and it's worth validating.)

---

_@not-my-profile reviewed on 2023-01-07 16:55_

---

_Review comment by @not-my-profile on `src/violation.rs`:1 on 2023-01-07 16:55_

Yes I'd consider the functions that create the diagnostics to be the definitions of the rules.

---

_Comment by @charliermarsh on 2023-01-07 17:31_

> (I also want to do a quick sanity check to ensure there are no major perf changes prior to merging. I don't expect any, but it's a large change and it's worth validating.)

No noticeable changes, as expected.


---

_Review comment by @charliermarsh on `src/violations.rs`:27 on 2023-01-07 17:36_

Can these references be `Self` instead? It looks like it from my testing.

---

_Review comment by @charliermarsh on `src/violations.rs`:49 on 2023-01-07 17:36_

Similarly, can this be `let Self(length, limit) = self`?

---

_@charliermarsh approved on 2023-01-07 17:39_

Ok, this looks good to me.

To articulate some of the outstanding work for future PRs:

- Move some structs out of `registry.rs`.
- Move some methods like `lint_source` off of `CheckCode`.
- Update the `CONTRIBUTING.md` to point to `violations.rs`.
- Rename `Check` to `Diagnostic`.
- Rename `CheckKind` to... something else.
- Get rid of or rename `Message`.
- Change `category()` to `origin()`.
- Remove the `checks` and `plugins` convention; replace with `rules`.

---

_Review comment by @not-my-profile on `src/violations.rs`:27 on 2023-01-07 18:32_

Yes they can be. However I think it makes more sense to get rid of `placeholder` entirely instead. We only use it for documentation and I think it often does a very poor job at that e.g. `'...' % ... expected mapping but got sequence` or `... is banned: ...` really are not informative and looking at the placeholder message often is confusing since it's not clear what is part of the message and what is interpolated.

I think as soon as we have a system where the rules can be documented via Rust doc comments on the structs we can look into getting rid of placeholder in favor of the new doc comments.

So I think you can add "Document rules via doc comments on violation structs" to your list of things to do for future PRs :)

If we still want to document the message format strings I think that's best achieved via a proc macro. In any case I don't think hand-written placeholder implementations make much sense.

---

_@not-my-profile reviewed on 2023-01-07 18:32_

---

_@not-my-profile reviewed on 2023-01-07 18:35_

---

_Review comment by @not-my-profile on `src/violations.rs`:49 on 2023-01-07 18:35_

Yes that works. However I would rather have us establish the practice of using named fields instead e.g.

```rust
struct LineTooLong { length: usize, limit: usize }
```

in which case we could get rid of that `let` statement entirely and just use e.g. `self.length` and `self.limit`.

(but again I think that's something for future PRs)

---

_@charliermarsh reviewed on 2023-01-07 18:42_

---

_Review comment by @charliermarsh on `src/violations.rs`:49 on 2023-01-07 18:42_

I agree with this.

---

_@charliermarsh reviewed on 2023-01-07 18:45_

---

_Review comment by @charliermarsh on `src/violations.rs`:27 on 2023-01-07 18:45_

I agree. Though, of course, we do need the placeholders _for now_ until we have an alternative.

---

_Comment by @not-my-profile on 2023-01-07 18:53_

I have time right now ... should I do the rebase now?

---

_Comment by @charliermarsh on 2023-01-07 18:55_

Go for it. I’m busy for a bit but will merge this next, then rebase any open PRs.

---

_Merged by @charliermarsh on 2023-01-07 20:14_

---

_Closed by @charliermarsh on 2023-01-07 20:14_

---

_Comment by @charliermarsh on 2023-01-07 20:28_

Having the violations defined in one struct like this is so much cleaner. It's a huge improvement.

---

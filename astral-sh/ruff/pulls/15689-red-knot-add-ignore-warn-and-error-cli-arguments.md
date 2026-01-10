```yaml
number: 15689
title: "[red-knot] Add `--ignore`, `--warn`, and `--error` CLI arguments"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/ignore-warn-error-cli-arguments
created_at: 2025-01-23T13:09:49Z
updated_at: 2025-01-24T15:20:49Z
url: https://github.com/astral-sh/ruff/pull/15689
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Add `--ignore`, `--warn`, and `--error` CLI arguments

---

_Pull request opened by @MichaReiser on 2025-01-23 13:09_

## Summary

Add support for enabling rules from the CLI and changing the rule's severity by adding
the `--warn`, `--error` and `--ignore` CLI arguments. 

The "tricky" part of this PR was that we need to preserve the order in which 
the arguments were specified so that e.g. `--ignore division-by-zero --error division-by-zero` resolves
to enabling the rule with an `error` severity because the `--error `argument comes last. 

The way this is solved is by manually implementing `Args` as [described here](https://docs.rs/clap/latest/clap/_derive/index.html#flattening-hand-implemented-args-into-a-derived-application)

## Test Plan

Added CLI tests, I did some manual testing as well. 


---

_Review requested from @carljm by @MichaReiser on 2025-01-23 13:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-23 13:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-23 13:09_

---

_Label `red-knot` added by @MichaReiser on 2025-01-23 13:09_

---

_@MichaReiser reviewed on 2025-01-23 13:10_

---

_Review comment by @MichaReiser on `.gitignore`:33 on 2025-01-23 13:10_

doh!

---

_@MichaReiser reviewed on 2025-01-23 13:10_

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:94 on 2025-01-23 13:10_

I moved this code from `main.rs`. The only difference is the added `rules` field and the `let rules = ...` in `to_options` 

---

_Review comment by @dcreager on `crates/red_knot/src/args.rs`:129 on 2025-01-23 14:43_

style nit: I think this will be easier to follow with some carefully named intermediate variables:

```suggestion
            let indices = matches.indices_of(arg_id).into_iter().flatten();
            let args = matches.get_many::<String>(arg_id).into_iter().flatten();
            rules.extend(indices.zip(args).map(|(index, rule)| (index, rule, level)));
```

---

_@dcreager approved on 2025-01-23 14:44_

---

_Review comment by @carljm on `crates/red_knot/src/args.rs`:35 on 2025-01-23 17:29_

I assume we still plan to rename this to `--python`, since it doesn't have to be a virtualenv?

But I see all this code was just moved from `main.rs`, so makes sense not to modify it in this PR.

---

_Review comment by @carljm on `crates/red_knot/src/args.rs`:132 on 2025-01-23 17:32_

```suggestion
        // Sort by their index so that values specified later override earlier ones.
```

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:259 on 2025-01-23 17:42_

Why is this included, when there's no unresolved-import diagnostic in the code? Is it just to verify that it's fine to specify a level for a diagnostic that doesn't occur?

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:304 on 2025-01-23 17:43_

This comment seems misplaced?

---

_@carljm approved on 2025-01-23 17:44_

Nice, exciting to see this start coming together!

---

_@MichaReiser reviewed on 2025-01-23 17:51_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:259 on 2025-01-23 17:51_

it's mainly to assert that you can specify multiple `--warn` arguments. I admit, I was too lazy to write another test (just so much boilerplate)

---

_@carljm reviewed on 2025-01-23 17:52_

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:259 on 2025-01-23 17:52_

Maybe add something in the code in this test that would emit this, so we aren't just asserting you can give the option twice, but also that it does something?

---

_@MichaReiser reviewed on 2025-01-23 17:53_

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:35 on 2025-01-23 17:53_

Yeah, there's an issue for that, somewhere.

---

_@sharkdp reviewed on 2025-01-24 09:49_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:359 on 2025-01-24 09:49_

I played around with the CLI for a bit and one thing I tried initially was to silence this error
```
error[lint:invalid-assignment] /home/shark/playground/test.py:4:1 Object of type `tuple[Literal["bar"]]` is not assignable to attribute `__slots__` of type `tuple[Literal["foo"]]`
```
by copying (what I thought was) the rule name, and then added `--ignore lint:invalid-assignment`.

This leads to
```
warning[unknown-rule] Unknown lint rule `lint:invalid-assignment`
```

It would be nice if we could show a better error message for this specific case (by checking if the rule exists after stripping `lint:` from the start of the argument?):

```
warning[unknown-rule] Unknown lint rule `lint:invalid-assignment`. Did you mean `invalid-assignment`?
```

---

_@sharkdp reviewed on 2025-01-24 09:50_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:365 on 2025-01-24 09:50_

Is it intended that we want to fail with exit code 1 in case of warnings?

---

_@MichaReiser reviewed on 2025-01-24 09:51_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:365 on 2025-01-24 09:51_

No, there's another issue that looks into changing this behavior.

---

_@sharkdp reviewed on 2025-01-24 09:58_

---

_Review comment by @sharkdp on `crates/red_knot/src/args.rs`:157 on 2025-01-24 09:58_

Minor: If I read "list of rules", I'm thinking of something like `--error=invalid-assignment,divison-by-zero`. If we don't want to support this, I think we might want to clarify this here with something like
```suggestion
                .help("Treat the given rule as having severity 'error'. Can be specified multiple times.")
```

---

_@MichaReiser reviewed on 2025-01-24 14:34_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:359 on 2025-01-24 14:34_

Nice find. I was pretty sure it's something I called out in the CLI proposal but it seems I didn't. 

A general concern around using `lint:<name>` in diagnostics is that it prevents copy-pasting the diagnostic code into a configuration or even a suppression comment. It's something that bothers me about clippy where the diagnostic uses `-` but `allow` and `deny` require `_`. 

We could change the diagnostic code or always accept `lint:`. But I'm not yet sure what the best design is here. Adding an improved message seems a good first step.

---

_Comment by @github-actions[bot] on 2025-01-24 15:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-01-24 15:20_

---

_Closed by @MichaReiser on 2025-01-24 15:20_

---

_Branch deleted on 2025-01-24 15:20_

---

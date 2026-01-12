```yaml
number: 666
title: Handle multiple unused submodule imports
type: pull_request
state: closed
author: squiddy
labels: []
assignees: []
base: main
head: handle-multiple-unused-submodule-imports
created_at: 2022-11-10T06:03:22Z
updated_at: 2022-12-26T07:18:24Z
url: https://github.com/astral-sh/ruff/pull/666
synced_at: 2026-01-12T05:36:31Z
```

# Handle multiple unused submodule imports

---

_Pull request opened by @squiddy on 2022-11-10 06:03_

I've got the tests and the right behaviour in place, but the code is not pretty right now. 

- [x] Cleanup code

---

This resolves the issue where ruff doesn't see any unused imports in code like this:

    import multiprocessing.pool
    import multiprocessing.process
    z = multiprocessing.pool.ThreadPool()

The underlying issue was twofold:

1. When tracking submodule imports, we used the first part of the module (multiprocessing in this example) as the key in the bindings map. Therefore both submodule imports would end up as one entry, with no way to differentiate between them.

2. When tracking references to imports, we only looked at Name expressions. A compound (attribute) expression such as multiprocessing.process resulted in two attempts to mark names/imports used:

   One for multiprocessing and one for process. Marking multiprocessing used together with the issue in 1) meant that all imports of multiprocessing were marked as used.

The solution:

1. For submodule imports, use the full name as key in the bindings map.

2. Detect Attribute access (such as foo.bar.baz, but not foo.bar["baz"]) and use that to check if it matches an import. If the visitor is not in such an Attribute in the tree right now, keep looking at Name expressions.

   With an attribute access like `multiprocessing.pool.ThreadPool`, iterate through all parts of it and try to look them up in bindings. This is necessary because certain modules can have different access patterns, e.g.

       import os
       os.path.exists("foo")

  instead of

       import os.path
       os.path.exists("foo")

Closes #60 

---

_Comment by @charliermarsh on 2022-11-11 00:39_

Awesome! Will review this soon!

---

_Comment by @charliermarsh on 2022-11-11 22:08_

@squiddy - Re-reading the summary -- do you want a code review on this, or did you wanna do any cleanup first?

---

_Comment by @squiddy on 2022-11-12 05:34_

I should have specified that more. I'd really appreciate a code review on the approach itself, the cleanup part is primary about the changes to `handle_node_load`.

---

_Comment by @charliermarsh on 2022-11-12 17:15_

You got it :)

---

_Marked ready for review by @charliermarsh on 2022-11-12 17:15_

---

_Comment by @charliermarsh on 2022-11-13 15:54_

(Overall, what you've written in the summary makes sense. Haven't read the code yet.)

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1965 on 2022-11-13 16:16_

I think we can avoid cloning any expressions here like:

```rust
let ids: Vec<(String, &Expr)> = match &expr.node {
    ExprKind::Name { id, .. } => vec![(id.clone(), expr)],
    ExprKind::Attribute { value, .. } => {
        let path = compose_call_path(expr).unwrap();
        let segments: Vec<&str> = path.split('.').collect();
        if segments.len() > 1 {
            (1..segments.len())
                .rev()
                .map(|i| (segments[..i].join("."), &**value))
                .collect()
        } else {
            vec![(segments.first().unwrap().to_string(), value)]
        }
    }
    _ => return,
};
```

---

_Review comment by @charliermarsh on `src/check_ast.rs`:2395 on 2022-11-13 16:17_

Is this safe? What assumptions is it making? (That there's at least one member in `ids`, and at least one member in `self.scope_stack`?)

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1970 on 2022-11-13 16:22_

I think this _always_ has to be true, right?

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1966 on 2022-11-13 16:24_

I'd love to get rid of the ID clones... trying to figure out if that's possible.

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1966 on 2022-11-13 16:35_

I think this gets it down to one copy:

```rust
let call_path;
let ids: Vec<(&str, &Expr)> = match &expr.node {
    ExprKind::Name { id, .. } => vec![(id, expr)],
    ExprKind::Attribute { value, .. } => {
        call_path = compose_call_path(expr).unwrap();
        call_path
            .rmatch_indices('.')
            .map(|(i, _)| (&call_path[..i], &**value))
            .collect()
    }
    _ => return,
};
```

---

_Review comment by @charliermarsh on `src/check_ast.rs`:1971 on 2022-11-13 16:36_

What about this case? This seems like it should be valid but is currently unhandled.

```py
import a.b.c.d

foo = a.b.c.d
```

Ruff gives me:

```py
Found 2 error(s).
foo.py:1:1: F401 `a.b.c.d` imported but unused
foo.py:9:7: F821 Undefined name `a`
```


---

_@charliermarsh reviewed on 2022-11-13 16:37_

---

_Comment by @charliermarsh on 2022-11-20 22:05_

@squiddy - Would you like me to clean up based on the comments and get this merged? Totally up to you.

---

_Comment by @squiddy on 2022-11-21 05:53_

> @squiddy - Would you like me to clean up based on the comments and get this merged? Totally up to you.

I've give it a try, thanks for the review.

---

_@squiddy reviewed on 2022-11-26 07:48_

---

_Review comment by @squiddy on `src/check_ast.rs`:1965 on 2022-11-26 07:48_

Thank you. Closing in favor of https://github.com/charliermarsh/ruff/pull/666#discussion_r1020931419, where you tidied this up even more.

---

_@squiddy reviewed on 2022-11-26 07:49_

---

_Review comment by @squiddy on `src/check_ast.rs`:1966 on 2022-11-26 07:49_

This is great, thank you. Applied it.

---

_Review comment by @squiddy on `src/check_ast.rs`:1971 on 2022-11-26 08:10_

`ids` didn't include the full identifier (only `a.b.c` in this case). Extended it to and your test case passes now.

---

_@squiddy reviewed on 2022-11-26 08:10_

---

_@squiddy reviewed on 2022-11-26 08:12_

---

_Review comment by @squiddy on `src/check_ast.rs`:1970 on 2022-11-26 08:12_

Yes, you are right. Resolving this because the current code doesn't care anymore.

---

_Review comment by @squiddy on `src/check_ast.rs`:2395 on 2022-11-26 08:37_

Yes to both.

At least one member in `ids` should be safe to assume. We're either called with a `Name` or `Attribute`, return otherwise. 
You are right about the `scope_stack`. If that can be empty, we would crash here. I'm not too familiar with that yet, can this be empty?

---

_@squiddy reviewed on 2022-11-26 08:37_

---

_@squiddy reviewed on 2022-11-26 08:39_

---

_Review comment by @squiddy on `src/check_ast.rs`:2395 on 2022-11-26 08:39_

I could initialize `not_found` with the first entry of `ids`, which is then either the name or the full expression for an attribute (e.g. `multiprocessing.pool.Pool`).

If we find a match in the imports, we return early.

If we don't, we'll report the whole expression/identifier as unknown.

---

_@squiddy reviewed on 2022-11-26 08:45_

---

_Review comment by @squiddy on `src/check_ast.rs`:2395 on 2022-11-26 08:45_

Okay, removed the unwrap and instead pick the first entry from `ids`. If that is empty, we return.

Please let me know what you think.

---

_Comment by @charliermarsh on 2022-11-26 21:28_

Cool, gonna profile this tonight to make sure it's not a significant regression, then we can merge.

---

_Comment by @charliermarsh on 2022-11-27 04:53_

Introduces a small slowdown so wanna see if I can optimize this a bit, then will merge.

---

_Comment by @squiddy on 2022-12-09 10:40_

@charliermarsh I know you're very busy - good evidence to the success of ruff - so I'm not being pushy here.

Can I help you here move this forward? If it's the slowdown that is blocking this, how would I go about that measuring that? Is it just hyperfine + running ruff on the cmdline, or did you do some in-depth profiling?

I'm happy for anything you can share that helps me help you. :)

---

_Comment by @charliermarsh on 2022-12-09 21:29_

Thanks for this really kind and understanding message (and not pushy at all). I've been a bad maintainer on this one, asking you to make changes and then failing to follow-up, so I apologize.

I did try to get this across the finish line once or twice but was struggling to eke out any more performance gains and felt mixed about introducing a performance penalty. But the blocker now is that I need to do a fairly substantial rebase due to https://github.com/charliermarsh/ruff/pull/1147 and https://github.com/charliermarsh/ruff/pull/1137. If you're up for trying to handle that, it'd be appreciated. But otherwise, I'll continue to try to get to this when I can.


---

_Comment by @squiddy on 2022-12-10 04:09_

I'm doing the rebase now and see what else I can do.

---

_@squiddy reviewed on 2022-12-10 04:25_

---

_Review comment by @squiddy on `src/pyflakes/mod.rs`:2248 on 2022-12-10 04:25_

Unsure about this test. Why not report twice?

---

_@charliermarsh reviewed on 2022-12-10 04:28_

---

_Review comment by @charliermarsh on `src/pyflakes/mod.rs`:2248 on 2022-12-10 04:28_

Yeah reporting twice is better, I think.

---

_@charliermarsh reviewed on 2022-12-10 04:28_

---

_Review comment by @charliermarsh on `src/pyflakes/mod.rs`:2248 on 2022-12-10 04:28_

(These are literally the Pyflakes tests ported to Rust, so if we change behavior (as we're doing in this PR), we'll likely need to tweak the expectations of the tests.)

---

_Comment by @squiddy on 2022-12-10 07:29_

Currently experimenting with putting this attribute specific logic after the "simple" name-based lookup, under the assumption that `Name` are way more common generally than just inside `Attribute`, e.g. in function calls or variable lookups.

Benchmarks against CPython look promising, but I need to fix one more test. :crossed_fingers: 

---

_Comment by @squiddy on 2022-12-26 07:18_

Closing for now. I haven't made any good progress, might look into that again at some later point.

---

_Closed by @squiddy on 2022-12-26 07:18_

---

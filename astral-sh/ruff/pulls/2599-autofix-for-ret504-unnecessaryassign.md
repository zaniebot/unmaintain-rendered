```yaml
number: 2599
title: Autofix for RET504 UnnecessaryAssign
type: pull_request
state: closed
author: Sawbez
labels: []
assignees: []
base: main
head: main
created_at: 2023-02-06T01:37:17Z
updated_at: 2023-05-24T02:25:29Z
url: https://github.com/astral-sh/ruff/pull/2599
synced_at: 2026-01-12T03:50:02Z
```

# Autofix for RET504 UnnecessaryAssign

---

_Pull request opened by @Sawbez on 2023-02-06 01:37_

Works on #2263 

The only problem that I'm not too sure how to work around is how to add multiple fixes to a single violation. Additionally, I would like to remove the usage of `.clone` on the `Located<ExprKind>`, but I cannot seem to figure out how.

---

_Comment by @Sawbez on 2023-02-06 01:40_

For more context, currently my fix only changes the return, and doesn't remove the variable assignment (and thus I haven't changed the snap tests). E.x:
```py
def f():
    var = 2
    return var
```
becomes
```py
def f():
    var = 2
    return 2
```

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-06 18:03_

I think the framing here should be that we want to replace the `Assign` statement and the `Return` statement with the `return ${value}`.

Something like:

```rust
diagnostic.amend(Fix::replacement(
    unparse_expr(assign_expr, checker.stylist),
    assign_stmt.location,
    return_stmt.end_location.unwrap(),
));
```

---

_@charliermarsh reviewed on 2023-02-06 18:03_

---

_@charliermarsh reviewed on 2023-02-06 18:03_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/visitor.rs`:18 on 2023-02-06 18:03_

As a tip, we never need to use `Located<T>`. Instead, you can do `Expr` (which is equivalent to `Located<ExprKind>`).

---

_@Sawbez reviewed on 2023-02-06 22:56_

---

_Review comment by @Sawbez on `src/rules/flake8_return/rules.rs`:344 on 2023-02-06 22:56_

The only issue with this is that it won't work for something that is multi-line.
For example:
```py
def f(x):
    val = 2
    print(x)
    return val
```
Would fail and produce:
```py
def f(x):
    return 2
```
Unless we are tracking everything between those two with `source_slicer` or `source_locator` (something along those lines, I've seen it in other checkers). Additionally, do we want to leave behind empty space? The start of the expression is after the indentation, according to the AST.

---

_Review comment by @Sawbez on `src/rules/flake8_return/visitor.rs`:18 on 2023-02-06 22:57_

Thanks for the tip!


---

_@Sawbez reviewed on 2023-02-06 22:57_

---

_@Sawbez reviewed on 2023-02-07 01:03_

---

_Review comment by @Sawbez on `src/rules/flake8_return/rules.rs`:344 on 2023-02-07 01:03_

Another thing I'm slightly confused on is this code I'm trying:
```rust
if let Some(assign_loc) = stack.assigns.get(id.as_str()){
    println!("return {:?}", assign_loc.last().unwrap());
    diagnostic.amend(Fix::replacement(
        unparse_expr(assign_expr, checker.stylist),
        *assign_loc.last().unwrap(),
        expr.end_location.unwrap(),
    ));
}
```
This code causes ruff to hang, and the prints show why:
```console
return Location { row: 5, column: 4 }
return Location { row: 12, column: 4 }
return Location { row: 20, column: 4 }
return Location { row: 19, column: 4 }
return Location { row: 21, column: 4 }
return Location { row: 25, column: 4 }
return Location { row: 33, column: 4 }
... (this pattern repeats for a LOT of lines)
```
any clue why this could be happening?

---

_@charliermarsh reviewed on 2023-02-07 16:37_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-07 16:37_

I think for now we limit this to the version in which the assignment and the return are consecutive statements, if we can.

If we want to support fixes in which there are interleaving statements, we have to do something clever like structure the fix as a replacement with "all the content before the statement, followed by the new return" (rather than two separate fixes).

---

_@charliermarsh reviewed on 2023-02-07 16:38_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-07 16:38_

(I can help debug - what file / command are you running to get that output?)

---

_Review comment by @Sawbez on `src/rules/flake8_return/rules.rs`:344 on 2023-02-07 23:27_

@charliermarsh 
This is the command:
```console
> cargo run resources/test/fixtures/flake8_return/RET504.py --ignore=ALL --extend-select RET504 --no-cache --fix
```
The only difference in the code is adding the `*assign_loc.last().unwrap(),` and `if let Some(assign_loc) = stack.assigns.get(id.as_str())` with the `println`. I don't see why this would be called multiple times - perhaps I am simply unfamiliar with the `ruff` internals. The location and fix is correct, but then it gets called over and over and gets stuck.

---

_@Sawbez reviewed on 2023-02-07 23:27_

---

_@charliermarsh reviewed on 2023-02-08 01:52_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 01:52_

I haven't invested whether it handles all the cases we care about, but this version doesn't suffer from whatever recursion issue was being hit previously:

```rust
if checker.patch(diagnostic.kind.rule()) {
    if let Some(assign_expr) = stack.assign_values.get(id.as_str()) {
        if let Some(assign_loc) = stack.assigns.get(id.as_str()) {
            diagnostic.amend(Fix::replacement(
                unparse_stmt(
                    &create_stmt(StmtKind::Return {
                        value: Some(Box::new(assign_expr.clone())),
                    }),
                    checker.stylist,
                ),
                *assign_loc.last().unwrap(),
                expr.end_location.unwrap(),
            ));
        }
    }
}
```

---

_@charliermarsh reviewed on 2023-02-08 01:54_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 01:54_

As an aside, this:

```py
def f(x):
    val = 2
    print(x)
    return val
```

Actually doesn't trigger `RET504` if I'm not mistaken, since we treat any function calls between the assignment and the return (even if they don't involve the variable) as negating the rule.

---

_Review comment by @Sawbez on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 04:05_

Ah, I think you're right -- I should have this finished by tomorrow. My only other thought is how to work with the `Assign` statement being on another line, how would you ensure that there is just the assignment + whitespace/comment on the next few lines, would you have loop through the source code directly and check if each line is whitespace/comment or is there a better way?

What I'm talking about is something like this:
```py
def safe_average(lst):
    """There is a extra newline and extra indentation after the assignment"""
    return_value = 2.4
    # This is also a comment explaining why we need to check for comments, and there is indentation on the next line too!
    
    
    """
    Also, the next comment has *critical* information, but would the autofix remove it?
    I presume we would have to keep track of this, or just ignore this for now and 
    put it in an issue somewhere.
    """
    # We return this function in this manner because if the length was zero we would raise a ZeroDivisionError
    return sum(lst) / len(lst) if len(lst) > 0 else 0 
```

---

_@Sawbez reviewed on 2023-02-08 04:05_

---

_@charliermarsh reviewed on 2023-02-08 04:39_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 04:39_

So here's what I'd try to do (though it'll require more bookkeeping than we do now)...

When we track the assignment, track the start and end locations of the entire assignment statement.

Then, when we create the fix, structure it as: "Replace all the content from the start of the assignment with: (1) the content from the end of the assignment to the start of the return + (2) the modified return statement."


---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 04:45_

We also have the ability to skip the fix if there's a comment between the assign and the return -- we have a `has_comments_in` utility that takes a range as input, and returns true if there are any comments. So we could do that instead.

Or we could even really restrictive and say that we're only going to do this fix if it's purely whitespace between the assignment and the return. That's actually pretty reasonable IMO!

---

_@charliermarsh reviewed on 2023-02-08 04:45_

---

_Review comment by @Sawbez on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 05:21_

I'll try for the first option, if it seems a bit too difficult (shouldn't be, but will require a bit more restructuring and there's also a whitespace issue, but Black would fix that... not sure what the policy is for that) it's a bit late for me right now so I'll get to it by tomorrow. Sorry if I've asked too many questions or been a hassle, but thanks for helping me.

---

_@Sawbez reviewed on 2023-02-08 05:21_

---

_@charliermarsh reviewed on 2023-02-08 16:54_

---

_Review comment by @charliermarsh on `src/rules/flake8_return/rules.rs`:344 on 2023-02-08 16:54_

No apology needed at all. I wish I could be even more helpful! Just trying to juggle a few things at once. Appreciate your effort on the issue :)


---

_Comment by @Sawbez on 2023-02-09 02:48_

This is nearly finished - all it needs is to dedent the output if it isn't already. @charliermarsh How would you recommend going about this? 

---

_Review requested from @charliermarsh by @Sawbez on 2023-02-10 00:54_

---

_Comment by @Sawbez on 2023-02-10 00:59_

The code is working but it does have a false positive that changes the function of the code. I'm unsure how you would work around that -- the false positive originates from the check itself, and the autofix runs under the assumption that the check isn't a false positive.

We could patch the check, as it appears the problem in question isn't really refactor-able (thus why it's a false-positive in my opinion) unless you use a ternary statement, which is not the focus of this check. On the linked issue, it appears this was actually patched (no longer raises `RET504`) on the original plugin.

---

_Comment by @charliermarsh on 2023-02-10 01:11_

> On the linked issue, it appears this was actually patched

Where can I find the linked issue?

---

_Comment by @Sawbez on 2023-02-10 03:01_

> > On the linked issue, it appears this was actually patched
> 
> Where can I find the linked issue?

@charliermarsh It’s linked in the test
```python
# Can be refactored false positives
# https://github.com/afonasev/flake8-return/issues/47#issuecomment-1122571066
def get_bar_if_exists(obj):
    result = ""
    if hasattr(obj, "bar"):
        result = str(obj.bar)
    return result
```

---

_Comment by @charliermarsh on 2023-02-10 03:39_

Hmm, I think those issues still exist upstream -- I see them when I run `flake8-return` locally, and we have that same patch in our own code.

---

_Comment by @charliermarsh on 2023-02-10 03:47_

Spent a little time trying to fix that issue but it's really tough without more rigorous branch analysis.

---

_Comment by @charliermarsh on 2023-02-10 03:48_

I think we should try to be really conservative here, and only fix if they're consecutive statements -- otherwise, we risk breaking code, like above. What do you think?

---

_Comment by @Sawbez on 2023-02-10 04:30_

@charliermarsh I think there are pretty much three valid options here:

1. Only fix if there are strictly comments.
2. Only fix if there are no if statements, reassignments, or other complicated constructs.
3. Only fix if there is nothing between

I can probably implement any of these. Personally, I think the strictly comments (1) is probably the best in-between that will not cause breaking fixes and still fix most of the cases that `RET504` was directly supposed to catch. If you would rather opt for one of the other options I can do so. 

---

_Comment by @charliermarsh on 2023-02-10 13:18_

I think (1) makes sense. Do you wanna give that a try?

---

_Comment by @Sawbez on 2023-02-10 15:52_

Yep. I’ll do it

---

_Comment by @Sawbez on 2023-02-11 04:36_

This is very close to done. All I need to do is add one more check before going through the autofix, it will also probably fail CI because it's in the middle of being completed.

---

_Comment by @Sawbez on 2023-02-12 00:42_

@charliermarsh This is finished. You can take a look - the implementation isn't pretty but if you have any better ideas they would be well appreciated.

---

_Comment by @charliermarsh on 2023-02-12 00:47_

@Sawbez - Awesome, thank you for all the work! I likely won't get to this tonight, but will review as soon as I can.

---

_Closed by @Sawbez on 2023-02-13 00:08_

---

_Branch deleted on 2023-02-13 00:08_

---

_Branch restored on 2023-02-13 00:08_

---

_Reopened by @Sawbez on 2023-02-13 00:08_

---

_Comment by @Sawbez on 2023-02-13 00:12_

 Whoops- ignore that

---

_@charliermarsh reviewed on 2023-02-16 04:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:385 on 2023-02-16 04:30_

For some reason I'm having trouble pushing edits but these need to change to `.slice(...)` now instead of `. slice_source_code_range(...)`.

---

_@charliermarsh reviewed on 2023-02-16 04:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:390 on 2023-02-16 04:31_

I think we need a more rigorous way to detect this, if I'm interpreting the comment correctly. Indentation isn't a totally "safe" way to detect whether we have control flow like `if` or `while` etc., since users could do `if x: y` on a single line.

What's this logic guarding against? Maybe we can track this condition in some other way.

---

_@charliermarsh reviewed on 2023-02-16 04:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:378 on 2023-02-16 04:32_

When would this be `None`?

---

_@charliermarsh reviewed on 2023-02-16 04:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/helpers.rs`:49 on 2023-02-16 04:32_

Could we perhaps use the `indentation` helper in `crates/ruff/src/ast/whitespace.rs`?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/rules.rs`:376 on 2023-02-16 04:33_

Can we add some comments about what `assign_expr`, `assign_locations`, etc. represent here? I wrote some of this originally and even I can't remember what they mean and how they apply to this fix heh.

---

_@charliermarsh reviewed on 2023-02-16 04:33_

---

_@charliermarsh reviewed on 2023-02-16 04:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/visitor.rs`:19 on 2023-02-16 04:33_

Can this be `FxHashMap<&'a str, &'a Expr>`? Would that let you avoid the clone?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_return/visitor.rs`:42 on 2023-02-16 04:51_

Is this the same as the previous version? I know we have the logic in `// This is hacky ...`, but what if we visit a tuple here? Then we'd iterate over the names as part of _this_ method via the recursive call above. In the current version,  the `self.visit_assign_target` in the `Tuple` case above would have no effect, I think? Given `(a, b, c)`.

---

_@charliermarsh reviewed on 2023-02-16 04:51_

---

_Comment by @Sawbez on 2023-02-16 15:56_

I will start working on this later today.

---

_Comment by @Sawbez on 2023-02-16 15:56_

Thanks for the tips!

---

_Comment by @charliermarsh on 2023-02-16 17:00_

@Sawbez - I'm sorry for the slow review here. The autofix stuff has to be very carefully done, and the `flake8-return` logic is always really hard for me to reason about, so I struggled to find time to really dig into it.

---

_@Sawbez reviewed on 2023-02-16 23:53_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/rules.rs`:390 on 2023-02-16 23:53_

Yeah, I see what you mean with the one-liner. The main problem is that there's no `Dedent` token during the code that ensures it's comments.

---

_@Sawbez reviewed on 2023-02-17 00:02_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/rules.rs`:378 on 2023-02-17 00:02_

If you return an undefined variable (a problem in itself, but shouldn't cause a panic). The second `if let` can be removed however the third one is also important because it would fail if the variable was assigned to, but only *after* the return statement (similar problem). In my earlier testing it appeared that every assignment would be in the vector even if they were after the return statement (which was against an assumption I had made and was causing that infinite loop error because it was slicing the wrong way)

---

_@Sawbez reviewed on 2023-02-17 00:06_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/rules.rs`:390 on 2023-02-17 00:06_

I might have to be checking the line itself too (e.g. slicing the line and checking if there is any tokens there)

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/helpers.rs`:49 on 2023-02-17 00:17_

Maybe? But I would guess it would be slower because I would have to create a bunch of useless intermediate objects because of the way that the function takes in input as a `Located<T>`, and I mainly just have a `&str` 

---

_@Sawbez reviewed on 2023-02-17 00:17_

---

_@Sawbez reviewed on 2023-02-17 00:18_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/helpers.rs`:49 on 2023-02-17 00:18_

It would also be kind of inconvenient. I might have to iterate through it in a sliding window fashion manually (it would also be more performant).

---

_@Sawbez reviewed on 2023-02-17 00:19_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/helpers.rs`:49 on 2023-02-17 00:19_

To clarify, the function you are referring to would be inconvenient (and quite possibly slower) either way, and rewriting without `TupleWindows` would be faster either way.

---

_@Sawbez reviewed on 2023-02-17 00:23_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/rules.rs`:376 on 2023-02-17 00:23_

Will do.


---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/visitor.rs`:19 on 2023-02-17 00:23_

I will try this.


---

_@Sawbez reviewed on 2023-02-17 00:23_

---

_Review comment by @Sawbez on `crates/ruff/src/rules/flake8_return/visitor.rs`:42 on 2023-02-17 00:27_

I'm confused on what you mean here

---

_@Sawbez reviewed on 2023-02-17 00:27_

---

_Comment by @Sawbez on 2023-02-17 00:36_

@charliermarsh No problem. As long as you get to it eventually, then I'm good. Take as long (or as little) time as you would like.

---

_Comment by @charliermarsh on 2023-05-24 02:25_

I take responsibility for not getting this merged, but I suspect the code has changed enough that we'd want to revisit from scratch. Having seen more bugs and issues related to the `flake8-return` rules, I'd probably only want to support this behavior for trivial cases (e.g., return immediately following an assignment).

---

_Closed by @charliermarsh on 2023-05-24 02:25_

---

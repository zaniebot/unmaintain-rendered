```yaml
number: 18281
title: "[ty] Improve completions by leveraging scopes"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-scopes
created_at: 2025-05-23T16:27:18Z
updated_at: 2025-05-29T14:31:32Z
url: https://github.com/astral-sh/ruff/pull/18281
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Improve completions by leveraging scopes

---

_Pull request opened by @BurntSushi on 2025-05-23 16:27_

This PR improves completions by leveraging scopes. That is, we try to
identify an expression corresponding to the cursor's current position.
We then use that expression to find it scope. Our completions are then
the names in that scope's (and its ancestors') symbol tables.

The most straight-forward way this improves on the status quo can be
seen from a simple example:

```python
def foo():
    def foofoo(): ...
<CURSOR>
```

Today, this would include `foofoo` in the completions. But with this
PR, `foofoo` is not included.

I had originally started off with a bit more shenanigans in terms of
identifying the expression under the cursor. But in the end, at least
for scopes, a very simple solution seems to work well.

The major caveat here is that this *doesn't* work well when requesting
completions before having typed anything. While this doesn't fail in
every case, it is somewhat common. For example:

```python
def foo():
    def foofoo(): ...
    <CURSOR>
```

In this instance, the AST nodes for `foo` and `foofoo` actually end
at exactly the same byte offset, so there is nothing for us to easily
latch on to that comes after `foofoo` but before the end of `foo`.
In particular, we really want to enumerate the symbols for the scope
contained in the function body of `foo`. But it's unclear how to
(easily) actually get the scope given the cursor's position. One
approach might be to tweak the ranges of the AST nodes to make finding
overlapping AST nodes easier. Either way, in this case currently, we
fall back to the global scope. So we don't end up with a complete
failure, but I'm guessing this will prove to be frustrating for users.

(I do not think we need to fix this in this PR.)

Note that this doesn't apply when something has been typed. For example:

```python
def foo():
    def foofoo(): ...
    f<CURSOR>
```

In this case, we see that `f` is part of the body of `foo` and
correctly provide completions for both `foo` and `foofoo`.

There are some other cases where a `<CURSOR>` being in leading/trailing
whitespace results in sub-optimal failures. The commented out test
cases in the second commit cover a smattering of them.

Ref https://github.com/astral-sh/ty/issues/86


---

_Review requested from @carljm by @BurntSushi on 2025-05-23 16:27_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-23 16:27_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-23 16:27_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-23 16:27_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-05-23 16:27_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-05-23 16:27_

---

_Comment by @github-actions[bot] on 2025-05-23 16:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-23 16:35_

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

_Review request for @dcreager removed by @BurntSushi on 2025-05-23 16:51_

---

_Review request for @carljm removed by @BurntSushi on 2025-05-23 16:51_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-05-23 16:51_

---

_Label `server` added by @MichaReiser on 2025-05-23 17:35_

---

_Label `ty` added by @MichaReiser on 2025-05-23 17:35_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:131 on 2025-05-23 17:50_

This is funny but makes sense :)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:147 on 2025-05-23 17:50_

I forgot that our completion is indeed naive... I was like, wait, why does `foo` show up but makes sense

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:230 on 2025-05-23 17:54_

Instead of assuming any relationship between node and scope. I think I'd prefer if the IDE integration traverses downwards to find a node who's indentation matches the indentation at the current position, then lookup the scope for that node. This may require passing the scope to `model.completions` because our AST, unfortunately, doesn't support traversing upwards once you have an `AnyNodeRef`

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:345 on 2025-05-23 17:54_

Should we instead use `#[test(ignore="reason")]` or have the test pass but we add a TODO comment that documents the expected output (this is what we generally do in ty)

The benefit of having the test is that it will start failing if this ever gets fixed and the expected output is already documented (which should make it easy to accept the changes)

---

_@MichaReiser approved on 2025-05-23 17:56_

Nice. I think this is a great starting point! We can look into those tricky cases in a separate PR. 

I recommend uncommenting the tests and instead add a TODO to each test with the expected snapshot

---

_@BurntSushi reviewed on 2025-05-23 18:05_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:345 on 2025-05-23 18:05_

I'm fine with uncommenting and adding the expected snapshot as a TODO.

I think `#[ignore(reason = "...")]` ` is probably not that much better than what I have. It does avoid bitrot though.

---

_@BurntSushi reviewed on 2025-05-23 18:06_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:230 on 2025-05-23 18:06_

I don't think scopes are a public API, but I think I get what you're saying. I'm not sure that will work, but I think it will work in some cases and certainly seems worth exploring!

---

_@sharkdp reviewed on 2025-05-23 19:39_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:147 on 2025-05-23 19:39_

It looks like some other part of the LSP stack filters this out, though? Like if I try this in the editor, it won't actually show "foo" when "g" is already typed.

---

_@sharkdp reviewed on 2025-05-23 19:54_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:23 on 2025-05-23 19:54_

Just something I randomly noticed: there's no deduplication here. If you have something like the following, `foo` will show up twice in the suggested completions:
```py
def foo():
    pass

class C:
    def foo(self):
        pass

    def bar(self):
        f<CURSOR>
```


---

_@sharkdp reviewed on 2025-05-23 19:59_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:773 on 2025-05-23 19:59_

Does it make sense to add tests where identifiers would potentially clash with keywords? Something like
```py
classy_variable_name = 1

class<CURSOR>
```

---

_@sharkdp reviewed on 2025-05-23 20:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-23 20:07_

This does not take type information into account. For example, it will include symbols from unreachable branches:
```py
import sys

if sys.version_info < (3, 0):
    legacy_symbol = 1

l<CURSOR>  # will include legacy_symbol
```

The `extend_with_declarations_and_bindings` function in https://github.com/astral-sh/ruff/pull/18251 does something similar to this, but excludes definitely-unbound symbols.

I'm not sure what the desired functionality here is (or if there is a performance tradeoff). Maybe it's fine to error on the side of providing too many completions.

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:773 on 2025-05-23 20:09_

Another thing that might be worth adding (it already works fine!):
```py
some_symbol = 1

print(f"{some<TAB>
```

---

_@sharkdp reviewed on 2025-05-23 20:09_

---

_@AlexWaygood reviewed on 2025-05-23 20:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-23 20:09_

I think we would at least want to make sure that unreachable bindings are included in the autocomplete suggestions if the user is editing a branch of code that is itself understood by ty to be unreachable

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-26 10:48_

Yeah, I think it would be useful to exclude symbols that are unreachable although that doesn't necessarily need to be done in this PR itself.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:147 on 2025-05-26 11:08_

Yeah, filtering is usually done on the client side so that it's consistent across other language servers but the server can do it's own filtering as well.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:458 on 2025-05-26 11:15_

This is due to error recovery. The source code contains a syntax error because the identifier is absent (`[ for bar in [1, 2, 3]`) and the parser recovers by converting `for` into an identifier. The fix is to improve error recovery where the parser is converting keywords into an identifier. There are benefits for doing this but maybe not doing this conversion might be more beneficial. Point 12 in #10653.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:559 on 2025-05-26 11:19_

Yeah, parentheses aren't part of the node's range unless it's relevant e.g., tuples.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/completion.rs`:23 on 2025-05-26 11:27_

Yeah, we would need to model shadowing here somehow because the completions should understand the kind of "foo" symbol.

I guess a naive way to solve this would be to keep a running list of completions as we traverse the scope upwards and avoid adding a symbol if it already exists.

---

_@dhruvmanila approved on 2025-05-26 11:29_

Nice!

---

_@AlexWaygood reviewed on 2025-05-26 12:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-26 12:00_

Note that I'm actually saying the opposite to Dhruv and David here. I agree that excluding unreachable bindings makes sense if the user is editing reachable code. But what if we've got a situation like this?

Config: `--python-version=3.10`

```py
import sys

if sys.version_info >= (3, 11):
    long_variable_name = []
    lo<CURSOR>
```

I think it'll be pretty annoying for the user there if we don't include `long_variable_name` in the list of autocomplete suggestions when the cursor is at that location. But the binding is in unreachable code!

---

_@sharkdp reviewed on 2025-05-26 12:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-26 12:24_

That's why I was wondering if we should lean towards providing too many completions.

But note that other LSPs also have degraded experience in unreachable code sections:

![image](https://github.com/user-attachments/assets/92e73013-59f3-49c3-8f78-f7411953c290)

That's not a *great* reason for not trying to do better here. But I also don't think it's high priority to provide the best-possible LSP experience inside unreachable sections of code. For other languages, I often deliberately change the config when I'm editing otherwise unreachable code.

---

_@carljm reviewed on 2025-05-27 19:35_

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:23 on 2025-05-27 19:35_

I think another important issue revealed by this example is that we aren't modeling the quirks of Python name resolution, e.g. that names in a class scope are not visible within nested function scopes (so only the global `foo` should even be visible here.) Doesn't necessarily need to be in this PR, but I think this will be important to fix; having all methods of a class wrongly show up as autocomplete suggestions within other methods of the class will be pretty annoying.

---

_@BurntSushi reviewed on 2025-05-29 13:58_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:23 on 2025-05-29 13:58_

For now, I've "fixed" this by just sorting and de-duplicating the completions returned by the server. I had assumed the client would de-duplicate since it was already doing prefix matching, but that does not appear to be the case. I've also added tests covering these cases (along with an appropriate FIXME for the false positives that @carljm points out).

I agree that what we're doing right now is _very_ dumb. I'm working my way up to fancier things. :-) I agree that the false positives here should be fixed _and_ that completions should be more than just name strings.

---

_@BurntSushi reviewed on 2025-05-29 14:09_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:773 on 2025-05-29 14:09_

Yes! Nice test cases.

---

_@BurntSushi reviewed on 2025-05-29 14:13_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_model.rs`:66 on 2025-05-29 14:13_

Yeah I agree we can do better here, but I think I generally prefer offering too many completions in a case like this until we can implement what @AlexWaygood suggests.

---

_Comment by @BurntSushi on 2025-05-29 14:15_

OK, I've addressed what feedback I think is plausible to do in this PR. I've recorded the notes about where completions can be improved (which I'll try to summarize in GitHub issues when I get a chance) for future work. I don't have a good sense of how hard some of these things are yet. e.g., Detecting whether the cursor is in an unreachable scope and ensuring that instance methods aren't treated as if they were normal functions.

---

_Merged by @BurntSushi on 2025-05-29 14:31_

---

_Closed by @BurntSushi on 2025-05-29 14:31_

---

_Branch deleted on 2025-05-29 14:31_

---

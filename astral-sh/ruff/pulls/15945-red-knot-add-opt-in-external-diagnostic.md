```yaml
number: 15945
title: "[red-knot] add opt-in external diagnostic snapshotting to mdtest"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/diagnostic-tests
created_at: 2025-02-04T19:59:18Z
updated_at: 2025-02-05T18:08:10Z
url: https://github.com/astral-sh/ruff/pull/15945
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] add opt-in external diagnostic snapshotting to mdtest

---

_Pull request opened by @BurntSushi on 2025-02-04 19:59_

This PR makes it possible to opt into diagnostic snappshotting
in mdtests. To opt in, add `snapshot-diagnostic` to the info string
on at least one of the code blocks in a single test. Like this:

````markdown
## Unresolvable module import

```py snapshot-diagnostic
import zqzqzqzqzqzqzq
```
````

This integrates with `insta` via external file snapshotting, so
one can just use `cargo insta` to manage the snapshots like you
would anywhere else.

While highly desirable, this doesn't support inline snapshotting.
I don't see an easy way to make this work with `insta`, so I think
we'd have to roll our own snapshotting system. I actually do think
that's probably worth doing, but this at least gets us some integration
with mdtests and full diagnostic output without too much effort.

Reviewers are encouraged to go commit-by-commit!

Closes astral-sh/ruff#15867


---

_Review requested from @carljm by @BurntSushi on 2025-02-04 19:59_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-04 19:59_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-04 19:59_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-04 19:59_

---

_Comment by @BurntSushi on 2025-02-04 20:07_

Yessss. My favorite kind of problem. /s

![windows-slashes](https://github.com/user-attachments/assets/74aa00bb-7a3f-4d48-bb0b-65a5f635b24a)

---

_Comment by @github-actions[bot] on 2025-02-04 20:08_

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

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:339 on 2025-02-04 21:10_

Will this include a leading `=` in the `value`? Should we use `eat_char` instead of `first` to avoid that?

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:151 on 2025-02-04 21:20_

This worries me, particularly in combination with non-rendered info string being used to mark a code block as snapshotted. I think it will be confusing to have code blocks in our mdtest files that look like they are asserting a particular code example is diagnostic-free when it isn't. In source form, this is somewhat mitigated by the presence of the `snapshot-diagnostic` tag (though I still think it's not ideal); in rendered form it could be very confusing.

I'm inclined to instead try without this special case, and just require the inline assertions too. I realize this could seem redundant, but I'm not sure it really is. I think out-of-file full snapshots are a pretty inconvenient way to make any kind of semantic assertions (I mean mostly for the reader here; snapshotting makes it pretty convenient for the _writer_. But I think readability of tests is very important!), so I don't think we want to be relying on out-of-file snapshots for testing semantics, only for testing diagnostic display.

So maybe this partly depends how we use the out-of-file snapshot feature. If we mostly just take existing semantic tests that we wanted to have anyway, add a snapshot-diagnostic tag to some of them, and then also have full diagnostic snapshotting, then I think it's fine (and preferable) if we still require the inline comment assertions. But if we end up needing a lot of edge-case tests of diagnostic rendering that aren't really doubling as semantic tests, I could see it getting irritating to have to still provide the inline comments.

If we envision mostly the latter case, then I would rather segregate our diagnostic-snapshot tests entirely into separate files, that are clearly marked as solely testing diagnostic output, not semantics. In this case I think not requiring inline comments is OK. But if we are going to have intermixed tests in the same file, or tests serving double duty as both tests of semantics and diagnostic rendering, then I don't want to permit leaving out the inline assertions.

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:145 on 2025-02-04 21:26_

I'm wondering about the rationale or need for this. It seems confusing. Is it difficult to implement snapshotting for some files in a test and not others? If so, then I wonder if we should find a different way to signal the need for snapshotting, that is scoped to the markdown heading and not to the code block. If not, then it seems less confusing to me to only snapshot the code blocks explicitly marked for snapshotting.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/basic.md`:127 on 2025-02-04 21:28_

Depending how we resolve my other comment about inline assertions vs snapshots, and how we intend to use snapshotting, I might prefer to have a dedicated file demonstrating snapshotting, rather than a couple random tests in the import semantics test.

If these stay here, then I don't want to omit the inline assertion comment on the "Unresolvable module import" test.

---

_@carljm reviewed on 2025-02-04 21:30_

Thank you, this is awesome!

I didn't carefully review the code changes yet, my comments are more on the feature design first.

---

_@MichaReiser reviewed on 2025-02-04 21:54_

---

_Review comment by @MichaReiser on `crates/red_knot_test/README.md`:151 on 2025-02-04 21:54_

Have you considered using an inline marker for diagnostics instead of enabling snapshoting for all diagnostics in a file. E.g something like # error[snapshot] "expected error



---

_@BurntSushi reviewed on 2025-02-04 22:07_

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:151 on 2025-02-04 22:07_

Just a quick (not full) response for now:

The main reason I went with this approach is because I perceive needing to write inline assertions for tests specifically for diagnostics to be pretty annoying. Like, just thinking about my recent work on ruff when I upgraded `annotate-snippets`, I had to go through something 1000+ snapshots. If I didn't have tooling to let me automatically update snapshots, that would have taken me way longer. So I guess where I'm going with this is that I perceive the inline assertions to be very manual, and I worry about the work required to update them when diagnostics change.

With all that said, the inline assertions also contain a lot less information. And are probably less susceptible to churn. So maybe this is fine. But there's definitely still some work involved in creating them in the first place that I can imagine being annoying if I want to write a test for diagnostics and not semantics.

I was also thinking that it would be nice for the semantic tests to also double as diagnostic tests. But maybe that's not a good idea?

> If we envision mostly the latter case, then I would rather segregate our diagnostic-snapshot tests entirely into separate files

Do you have a sense of how to enforce this?

> Have you considered using an inline marker for diagnostics instead of enabling snapshoting for all diagnostics in a file. E.g something like # error[snapshot] "expected error

I thought about this, and it felt really confusing to me to enable diagnostics for only one file instead of just everything in the test. I guess "snapshot diagnostics from everything that would be emitted by `red-knot check`" makes more sense to me than "snapshot diagnostics only from this file." I see the former as more holistic and the thing we most want to test.

But I don't have nearly as much experience as you @MichaReiser in working on diagnostics and testing them. Do you have different instincts here?

---

_@BurntSushi reviewed on 2025-02-04 22:10_

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:145 on 2025-02-04 22:10_

I think it's pretty easy to implement. See my other comment above for why I went this direction. But I agree that if I keep this direction, then it probably makes more sense to use something scoped to a section and not to a code block. Do you have any opinions on what that might look like? I was unsure of that direction since I don't think there are any section-scoped options right now.

---

_@carljm reviewed on 2025-02-04 22:22_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:151 on 2025-02-04 22:22_

> I perceive the inline assertions to be very manual, and I worry about the work required to update them when diagnostics change.
> 
> With all that said, the inline assertions also contain a lot less information. And are probably less susceptible to churn. So maybe this is fine. But there's definitely still some work involved in creating them in the first place that I can imagine being annoying if I want to write a test for diagnostics and not semantics.

Yeah, makes sense. For inline assertions that just assert an error code (the majority), I don't think changes to diagnostic rendering will cause churn; those assertions should only need to change if semantics or rule codes change. We do sometimes use the inline assertions to assert that some semantic information is present and clearly worded in a diagnostic message; these might be more likely to churn with changes to diagnostic rendering, if the rendering change also implies a different way of wording the message.

> > If we envision mostly the latter case, then I would rather segregate our diagnostic-snapshot tests entirely into separate files
> 
> Do you have a sense of how to enforce this?

If we want to go in this direction (segregate snapshot tests, don't have tests doing double duty) then I think perhaps just using a file-wide marker (rather than section-wide or per-code-block) to choose snapshotting would both enforce this separation and also address the other question of scoping of the `snapshot-diagnostics` option?

---

_@carljm reviewed on 2025-02-04 22:25_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:145 on 2025-02-04 22:25_

I don't think there's existing markdown syntax that lends itself naturally to this. One way to leverage existing mdtest syntax would be to make it an option in the toml config that can be optionally provided (as its own code block), but this is pretty verbose, and I'm not sure I like having mdtest-specific (as opposed to red-knot) configuration in those toml blocks.

So beyond that, I guess it could be something like adding `[snapshot]` to the end of the header line? Or on the next non-empty line after the header?

---

_@BurntSushi reviewed on 2025-02-04 23:11_

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:151 on 2025-02-04 23:11_

I thought a little more about this, and maybe it's premature to have this split between inline assertions and diagnostics. If I roll that part back and still require the inline assertions (with the snapshot essentially just being an opt-in add-on at that point), are you okay with diagnostic tests being inter-mingled here? Or do you think it's still worth separating them out and using a file-scoped option?

---

_@BurntSushi reviewed on 2025-02-04 23:12_

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:151 on 2025-02-04 23:12_

I say premature because I don't really have a ton of experience writing these inline assertions. I can believe it's not as annoying as I thought it would be.

---

_@carljm reviewed on 2025-02-04 23:23_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:151 on 2025-02-04 23:23_

If we continue to require the inline assertions for now, I'm fine with intermingling; in that case the tests still serve just as well as semantic tests. Trying that out and seeing how we like it seems fine to me.

---

_@MichaReiser reviewed on 2025-02-05 07:41_

---

_Review comment by @MichaReiser on `crates/red_knot_test/README.md`:151 on 2025-02-05 07:41_

I'm okay with test-level opt-in to snapshotting. The only drawback is that I could see it being useful to only select a single (or few) diagnostics to be snapshotted to avoid having to write extra diagnostic tests (we might still want to do that for complex diagnostics):

https://github.com/astral-sh/ruff/blob/d082c1b2026afdc0fda0d90553f0d71157877242/crates/red_knot_python_semantic/resources/mdtest/binary/custom.md#L81-L107

The syntax I've in mind is:

```python
a / 0 # error[snapshot]: [division-by-zero]

# or 
a / 0 # snapshot: [division-by-zero]
```

It would be nice if we could just snapshot one diagnostic as an example instead of writing an extra test for the diagnostic that otherwise duplicates that test case. 

Or a similar one https://github.com/astral-sh/ruff/blob/5e9259c96c910fe9031b7781867c8b82bffa0b26/crates/red_knot_python_semantic/resources/mdtest/binary/integers.md#L73-L102

The mdtest framework would collect all diagnostics for all assertions marked as `[snapshot`] for a single test and write them to the same snapshot file. 

The counterargument why having a separate test is useful is that it provides better isolation and prevents otherwise unrelated changes (e.g. adding a new expression to one such test) to change the diagnostic output because the line numbers change. 






---

_Label `red-knot` added by @MichaReiser on 2025-02-05 09:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:446 on 2025-02-05 10:46_

nit: is it worth creating an `Item` struct?

```rs
struct Item<'s> {
    key: &'s str,
    value: Option<&'s str>
}
```

then this could be

```rs
        keyvals: &[Item<'s>],
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:493 on 2025-02-05 10:47_

hrm, it feels like we'll probably forget to update this error message at some point if we do end up adding more supported keyvals. Not that I have a better suggestion :/

---

_@AlexWaygood reviewed on 2025-02-05 10:47_

---

_@AlexWaygood reviewed on 2025-02-05 11:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:145 on 2025-02-05 11:17_

We could use an HTML comment at the top of the file? I think they're hidden by GitHub's MarkDown rendering?

````md
<!-- snapshot-diagnostics -->

# This is my mdtest

Tests for the `foo` Python feature

```py
foo()
```
````

Rendered, this looks like:

<!-- snapshot-diagnostics -->

# This is my mdtest

Tests for the `foo` Python feature

```py
foo()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:150 on 2025-02-05 11:19_

grammar nit: "one of the need"

```suggestion
one of the needs to label each diagnostic manually when it ought to be covered by
```

Though "absolves one of the needs" also sounds a bit strange to my ears -- I'd either go with "absolves the need" or "partially absolves the need"

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:307 on 2025-02-05 11:38_

(see also: https://github.com/astral-sh/ruff/issues/13939)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:18 on 2025-02-05 11:43_

since it seems like we're manually writing the original source code into the snapshot anyway... I wonder if we could add line numbers? That would make it really easy to cross-reference between the line numbers in the diagnostic and the line numbers in the original source code

```snap
1 | # Topmost component resolvable, submodule not resolvable:
2 | import a.foo  # error: [unresolved-import] "Cannot resolve import `a.foo`"
3 |
4 | # Topmost component unresolvable:
5 | import b.foo  # error: [unresolved-import] "Cannot resolve import `b.foo`"
```

I don't think this costs us much in terms of how the snapshot is rendered by GitHub -- it already does not apply MarkDown rendering to this file, due to the `.snap` file extension:

<details>
<summary>Screenshot of current rendering of snapshot</summary>

![image](https://github.com/user-attachments/assets/bd530746-d309-46bd-a25d-abafa2349c4e)

</details>


---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:96 on 2025-02-05 11:45_

nit: add a `MatchFileMode::should_skip_unmatched_diagnostics_without_assertions()` method?

---

_@AlexWaygood reviewed on 2025-02-05 11:45_

---

_@AlexWaygood reviewed on 2025-02-05 11:51_

Overall this is awesome!

I have similar concerns to @carljm:
- To me it's pretty counter-intuitive that adding `snapshot-diagnostics` to one code fence would enable snapshotting for all embedded files in the mdtest suite. I would prefer a mechanism that makes it more obvious that it toggles the setting for the whole mdtest (if that's the way we want to enable snapshotting), such as an HTML comment at the top of the file (https://github.com/astral-sh/ruff/pull/15945/files#r1942686379)
- I'm also not sure I'm keen on having mdtest automatically ignore unmatched diagnostics if there are no inline assertions and diagnostic snapshotting is enabled. It feels too implicit and error-prone to me, and I don't find adding an inline assertion to be particularly arduous if you only assert the error code (omitting the error message). One of the reasons I'm most excited about snapshotting is I think we can assert the full error message much less in our inline assertions; the full error message feels like something that's much better covered by our snapshots, enabling us to keep our inline assertions concise, readable, and focussed on whether we capture Python's semantics correctly.

---

_@MichaReiser reviewed on 2025-02-05 11:53_

---

_Review comment by @MichaReiser on `crates/red_knot_test/README.md`:151 on 2025-02-05 11:53_

Another consideration is how we'd envision inline snapshots to work. 

* With `error[snapshot]`: I'd expect that the md test framework is in full control of the ``` code block and it adds it if there's any `error[snapshot]` and otherwise removes it. It isn't something a user ever has to write.
* With a test-level marker: It's up to users if they want an inline snapshot or out of bound snapshot. Out of bound snapshots use the test level marker (e.g. what Alex suggested with a `<!-- snapshot-diagnostics -->`) and inline snapshots use ```output```. 


---

_@MichaReiser reviewed on 2025-02-05 11:54_

---

_Review comment by @MichaReiser on `crates/red_knot_test/README.md`:145 on 2025-02-05 11:54_

What would be neat long term if output snapshots would be a link to the snapshot file. Should we use 

```
- [Snapshots]: 
```

or similar to enable this in the future? 

Either way. I'm fine with whatever marker you pick :)

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:151 on 2025-02-05 13:35_

In out-of-band discussion, we (Carl hasn't weighed in yet, but based on discussion here I think he'll be fine with this) decided to simplify here and just permit enabling of snapshots at the level of the section. But we agreed that annotated snapshots for more granular testing will likely be useful and probably something we should add in the future after we get some more experience.

---

_@BurntSushi reviewed on 2025-02-05 13:35_

---

_@BurntSushi reviewed on 2025-02-05 13:36_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/import/basic.md`:127 on 2025-02-05 13:36_

I ended up rolling back the change that ignores unmatched diagnostics when there aren't any inline assertions. And I moved this config to be a section-level config.

---

_@BurntSushi reviewed on 2025-02-05 13:37_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/parser.rs`:446 on 2025-02-05 13:37_

Ended up deleting this code.

I'm not a huge fan of anonymous tuples myself, but I tend not to mind them so much when their use is "local." And especially, when we aren't storing them anywhere. But I also don't feel strongly.

---

_@BurntSushi reviewed on 2025-02-05 13:39_

---

_Review comment by @BurntSushi on `crates/red_knot_test/README.md`:150 on 2025-02-05 13:39_

Yeah "needs" sounds weird to me too. I ended up deleting this, but getting rid of "one of" would be a good change here!

---

_@BurntSushi reviewed on 2025-02-05 13:40_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:307 on 2025-02-05 13:40_

Blech, yeah. I think we need more fine grained color configuration than just a global switch. The global switch makes it very easy to abuse.

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/matcher.rs`:96 on 2025-02-05 13:40_

I could go either way here to be honest. But I ended up deleting this.

---

_@BurntSushi reviewed on 2025-02-05 13:40_

---

_@MichaReiser reviewed on 2025-02-05 13:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:18 on 2025-02-05 13:49_

Some markdown viewers support `py=` (note the `=`) to enable line numbers. Unfortunately, GitHub doesn't

> it already does not apply MarkDown rendering to this file, due to the .snap file extension:

We can enable markdown rendering by using `.md.snap` and adding it to `.gitattributes` https://github.com/astral-sh/ruff/blob/8e3633f55ac8e3768df7d2e51619f37e6a8580c8/.gitattributes#L20

---

_@BurntSushi reviewed on 2025-02-05 14:03_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:18 on 2025-02-05 14:03_

I like this

---

_@AlexWaygood reviewed on 2025-02-05 14:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:30_

unfortunately GitHub's MarkDown rendering for these snapshots seems to have disastrous consequences :( it seems to think you're trying to create an HTML table

<https://github.com/astral-sh/ruff/blob/ag/diagnostic-tests/crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap>

---

_@AlexWaygood reviewed on 2025-02-05 14:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:31_

we may want to go back to using just `.snap` as the extension rather than `.md.snap`, if this is the result. Or adjust the contents of the snapshot so that GitHub doesn't think we're trying to create a table 😆

---

_@BurntSushi reviewed on 2025-02-05 14:37_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:37_

Yeah it looks like it's the same for our other `.md.snap` files: https://github.com/astral-sh/ruff/blob/e760bc2d60cb6e72b0c0f86fe9cc054df09f9014/crates/ruff_linter/src/rules/pylint/rules/snapshots/ruff_linter__rules__pylint__rules__unreachable__tests__while.py.md.snap#L1

I'm going to give up on this one for now, since I don't think making the snapshots render will in GitHub is a high priority goal for me.

This might also just be hard to fix, since `cargo insta` adds its own little header to snapshots and I'm not sure that can be customized.

---

_@BurntSushi reviewed on 2025-02-05 14:38_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/parser.rs`:493 on 2025-02-05 14:38_

Yup, definitely a downside. I ended up deleting this code anyway.

---

_@BurntSushi reviewed on 2025-02-05 14:39_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:18 on 2025-02-05 14:39_

I added line numbers, and make the file names be `.md.snap`, but the GitHub rendering is busted. It looks busted for our other `.md.snap` files too. Since the priority here IMO is the snapshot file itself in dev workflows, I'm going to keep the line numbers but abandon trying to get GitHub rendering nice for now.

---

_@AlexWaygood reviewed on 2025-02-05 14:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:42_

I agree that beautiful syntax highlighting in GitHub's rendering shouldn't be a priority, but I do tend to use GitHub for browsing through code quite a lot -- it would be quite nice for me if they were at least legible when viewed on GitHub :-) Could we just go back to using `.snap` for the extension rather than `.md.snap`? That would be good enough for me

---

_Review requested from @carljm by @BurntSushi on 2025-02-05 14:44_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-05 14:45_

---

_@AlexWaygood approved on 2025-02-05 14:48_

LGTM other than https://github.com/astral-sh/ruff/pull/15945#discussion_r1943070593. Thank you!

---

_@BurntSushi reviewed on 2025-02-05 14:54_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:54_

Ooooo, yes, no problem!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 14:56_

Thank you ❤️

---

_@AlexWaygood reviewed on 2025-02-05 14:56_

---

_@BurntSushi reviewed on 2025-02-05 15:00_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/red_knot_test__basic.md_-_Structures_-_Unresolvable_submodule_imports.md.snap`:1 on 2025-02-05 15:00_

OK, yes, now it's at least readable hah: https://github.com/astral-sh/ruff/blob/98745f8d39d8c531ccde052ad19659221eefe297/crates/red_knot_python_semantic/resources/mdtest/snapshots/basic.md_-_Structures_-_Unresolvable_submodule_imports.snap

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:1 on 2025-02-05 16:15_

Is it possible to change the snapshot extension to `md.snap` so that we get markdown rendering on Github? (you might also have to insert an artificial newline at the start of the file to have the title separated from `---`)

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:247 on 2025-02-05 16:16_

Nit: Should we avoid collecting the diagnostic if `should_snapshot_diagnostics` is false?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:312 on 2025-02-05 16:17_

Should/could we insert a link to the mdtest file?

---

_@MichaReiser approved on 2025-02-05 16:18_

This is great! Thanks for taking the time to integrate diagnostic snapshoting into mdtests

---

_@AlexWaygood reviewed on 2025-02-05 16:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:1 on 2025-02-05 16:21_

Andrew tried that, but unfortunately the MarkDown rendering made it completely unreadable, as GitHub thought we were trying to make an HMTL table. See discussion in https://github.com/astral-sh/ruff/pull/15945#discussion_r1943048249 :-)

---

_@carljm approved on 2025-02-05 16:27_

This approach looks great to me as a starting point to experiment with and see how we like it! Thanks for considering my comments.

---

_@BurntSushi reviewed on 2025-02-05 17:37_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/resources/mdtest/snapshots/basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:1 on 2025-02-05 17:37_

Yeah I think for now I'd prefer not to spend more time fiddling with this. The existing `.md.snap` files don't render well either: https://github.com/astral-sh/ruff/blob/a3f802acb4aacb7a6ff3d8daaae4dd9cf3620f8d/crates/ruff_linter/src/rules/pylint/rules/snapshots/ruff_linter__rules__pylint__rules__unreachable__tests__if.py.md.snap#L4

---

_@BurntSushi reviewed on 2025-02-05 17:38_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:247 on 2025-02-05 17:38_

Yeah that's fair. Done.

---

_@MichaReiser reviewed on 2025-02-05 17:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/basic.md_-_Structures_-_Unresolvable_submodule_imports.snap`:1 on 2025-02-05 17:51_

Yeah, we might need a few newlines after the table but we can follow up on that separately

---

_@BurntSushi reviewed on 2025-02-05 17:54_

---

_Review comment by @BurntSushi on `crates/red_knot_test/src/lib.rs`:312 on 2025-02-05 17:54_

OK, so I've at least put the full path to the mdtest file here.

I'm not sure about a link. Since this already isn't rendering well as Markdown on GitHub. But if we get that working then I think just turning it into a relative link that GitHub understands should be straight-forward.

---

_Merged by @BurntSushi on 2025-02-05 18:02_

---

_Closed by @BurntSushi on 2025-02-05 18:02_

---

_Branch deleted on 2025-02-05 18:02_

---

```yaml
number: 15704
title: "[red-knot] Allow specifying paths for mdtest code blocks in a separate preceding line"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - ty
assignees: []
base: main
head: rk-mdtest-paths
created_at: 2025-01-24T00:27:36Z
updated_at: 2025-02-02T23:49:29Z
url: https://github.com/astral-sh/ruff/pull/15704
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Allow specifying paths for mdtest code blocks in a separate preceding line

---

_@InSyncWithFoo_

## Summary

Resolves #15695.

## Test Plan

Unit tests and Markdown tests.

---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-24 00:27_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-24 00:27_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-24 00:27_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-24 00:27_

---

_Comment by @InSyncWithFoo on 2025-01-24 00:36_

[The first commit](https://github.com/astral-sh/ruff/pull/15704/commits/7435cc950b3b9417582fa4aaaecf7ef9f8a23679) is the main change. It also changes the default path for `pyi` blocks from `test.py` and `test.pyi`, which leads to [the second commit](https://github.com/astral-sh/ruff/pull/15704/commits/40ffa083c793d42182ae34d39e375f6d3e3292a7):

`````markdown
<!-- Before (only code block in the section; path defaults to `test.py`) -->
```pyi path=a.pyi
b: int = ...
```

<!-- After (path defaults to `test.pyi`) -->
```pyi
b: int = ...
```
`````

[The third](https://github.com/astral-sh/ruff/pull/15704/commits/48446681dcb43412eb8e45c6ec69cc356d3ee4e5) converts all existing `path=` to the new format.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:421 on 2025-01-24 01:18_

On the line immediately following this, we should update the error message to recommend the new-style way of giving the file path instead.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:360 on 2025-01-24 01:21_

I know the issue said to support both old and new way of giving the path, but the rationale for that was to avoid having to convert all existing tests, and you've already done that in this PR. So I don't think there's any reason we should continue to support both ways of giving a file path; there should just be one way to do it (which renders well). So I would support just removing the `config` capture group and the `config` hash-map altogether, since it seems we are using it only for `path`. And I don't think we'd want to use it for anything meaningful in future, either, since it doesn't render.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:404 on 2025-01-24 01:22_

Nice, thank you for fixing both of these issues!!

---

_@carljm approved on 2025-01-24 01:23_

This looks great to me, thank you!

I'd like to make sure others on the team are good with it before I merge, so I'll wait until tomorrow.

If you want to avoid extra work, feel free to also wait on addressing my review comments (at least the one about only supporting the new way) until others on the team have had a chance to offer a different opinion.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:7 on 2025-01-24 02:29_

If we're open to bikeshedding the syntax, what about using a comment on the first line of the file content?

````suggestion
```pyi
# path=mod.pyi
````

That would keep the path tightly coupled to the content as before.  (And personally I think it [looks a bit nicer](https://gist.github.com/dcreager/24965355e25b4183d33aab40eef19282) in the rendered Markdown view)

---

_@dcreager reviewed on 2025-01-24 02:29_

---

_@InSyncWithFoo reviewed on 2025-01-24 02:42_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:7 on 2025-01-24 02:42_

I considered this too, but with `#@` to avoid potential ambiguity:

`````markdown
```pyi
#@ a.pyi
x: int = ...
```

```pyi
#@ b.pyi
y: str = ...
```

```py
from a import x
from b import y

z = (x, y)
```
`````

---

Rendered:

> ```pyi
> #@ a.pyi
> x: int = ...
> ```
> 
> ```pyi
> #@ b.pyi
> y: str = ...
> ```
> 
> ```py
> from a import x
> from b import y
> 
> z = (x, y)
> ```

---

It ended up not being chosen because I think comments don't make good headers. Compare:

> `a.pyi`:
> 
> ```pyi
> x: int = ...
> ```
> 
> `b.pyi`:
> 
> ```pyi
> y: str = ...
> ```
> 
> ```py
> from a import x
> from b import y
> 
> z = (x, y)
> ```

---

Mixing named and unnamed files makes this look worse than it could have been, but enforcing might not be the solution either, as there could be sections like this, where files might not interact with each other:

`````markdown
## Foobar

A long paragraph of explanation. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras feugiat auctor ligula a volutpat. Aliquam eu eleifend lectus. Duis bibendum, quam et dapibus maximus, mauris odio consequat augue, non volutpat dui diam ac ligula. Aenean et massa volutpat, varius velit ac, bibendum lectus.

```py
x = 1
```

Another long paragraph. Quisque dictum vitae massa quis vehicula. Nunc ipsum diam, porttitor accumsan nunc quis, volutpat efficitur enim. Fusce magna tortor, pellentesque eu diam nec, molestie iaculis nisi. Praesent ut lacinia nunc. Nulla tempor, elit at tempus volutpat, enim massa euismod tortor, sodales rutrum leo dui ac leo.

`foo.py`:

```py
y = 0
```

`bar.pyi`:

```py
z = 2
```
`````

---

Rendered:

> ## Foobar
> 
> A long paragraph of explanation. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras feugiat auctor ligula a volutpat. Aliquam eu eleifend lectus. Duis bibendum, quam et dapibus maximus, mauris odio consequat augue, non volutpat dui diam ac ligula. Aenean et massa volutpat, varius velit ac, bibendum lectus.
> 
> ```py
> x = 1
> ```
> 
> Another long paragraph. Quisque dictum vitae massa quis vehicula. Nunc ipsum diam, porttitor accumsan nunc quis, volutpat efficitur enim. Fusce magna tortor, pellentesque eu diam nec, molestie iaculis nisi. Praesent ut lacinia nunc. Nulla tempor, elit at tempus volutpat, enim massa euismod tortor, sodales rutrum leo dui ac leo.
> 
> `foo.py`:
> 
> ```py
> y = 0
> ```
> 
> `bar.pyi`:
> 
> ```py
> z = 2
> ```

---

_Label `red-knot` added by @dhruvmanila on 2025-01-24 04:38_

---

_Comment by @MichaReiser on 2025-01-24 07:32_

Thanks for working on this. It would have been great to share and discuss a proposal on the issue before proceeding directly to implementation. 

It would also be great to add more context to your PR summaries. It seems you've thought a lot about why you chose this specific notation but this appearant in the PR's summary. This makes it difficult to a) understand the PR because I've to read the code changes to see the new notation and b) avoid discussions that you've already thought about. 

That's why I think the next step here is to take a step back and explore possible notation, and ask for preferences. I want to avoid that we have to change this notation in the near future and, because of it, think it makes sense to discuss this a little more. E.g. I do like what @dcreager proposed, but I could also see using **`test.py`**. Another alternative is modifying the mdtest framework to update the test files in place: It reads the `[path=]` attribute and adds a title in front of each code snippet (or updates it). That's obviously more involved. 


---

_Comment by @carljm on 2025-01-24 17:53_

> Another alternative is modifying the mdtest framework to update the test files in place

I don't think we should do this. It seems very complex and error-prone, and unnecessary; I don't see what benefits it brings to justify the complexity. It's not like the `path=` version in the tag string is significantly easier to write.

---

_@carljm reviewed on 2025-01-24 18:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:7 on 2025-01-24 18:00_

I find all of these options (and also `**test.py**:` on prior line) pretty reasonable to read in the rendered output. 

Although the readability of the rendered version matters, on a day-to-day basis I'm more interested in what is readable (and writable) in the source format, since that's what we are working with every day.

After playing around with all three options (`test.py`:, **test.py**:, and `# test.py`) in both nvim and vscode, my favorite option is `test.py`:, as currently implemented in this PR. I find the comment version blends in too much with the file source code and doesn't stand out enough, and the bold version is more visual noise and more characters to type; the backticked version seems to me like the nicest balance.

---

_@MichaReiser reviewed on 2025-01-24 18:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:7 on 2025-01-24 18:34_

I'm fine with that. We should explore how we'd want to support other attributes, as pointed out by @sharkdp on the issue. 

---

_@carljm reviewed on 2025-01-24 19:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:7 on 2025-01-24 19:16_

I'm certainly open to discussion if others have strong preference in a different direction!

Supporting other attributes is a good point. The best option may differ depending on the nature of the attribute. For the `stage` attribute proposed for multi-stage tests, I think it's closely associated with the filename (as in, this is "stage 1" of "foo.py") so I would probably envision putting it on the same line, e.g. something like:

````markdown
`test.py`, stage=1:

```py
# file contents
```
````

---

_Comment by @AlexWaygood on 2025-01-24 19:42_

Thanks for working on this @InSyncWithFoo! This is very useful work, and I appreciate it.

I do agree with @MichaReiser that it would have been great to have a design discussion on the issue before going ahead and implementing this. If we now decide that we'd prefer a different design, it will mean an unnecessary amount of work for you. And it actually makes us feel bad for asking you to change your design now, because we're aware that it will mean more work for you :-) I do also agree that it would be _really_ nice if you could give more detailed descriptions about the decisions you've taken in your PR summaries. I know that you're always happy to explain the decisions you've taken when we ask you about them, but it's much harder for a second or third reviewer to get up to speed on a PR if they have to read the whole discussion thread in order to understand the PR, rather than just reading the summary in the PR description.

On to the PR itself... on the whole, I think this is a big improvement! It makes tests like those in `import/builtins.md` much easier to read:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/a3fc2a82-b3c4-489f-bca4-6ea07b42c12f)

</details>

However, the rendered "empty code blocks" in something like this test still look pretty odd to me. I think it would take up much less vertical space if the filename were inside the code block as a "magic comment" like @dcreager suggested:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/52c3c788-2cd4-4ec9-9496-af292e284715)

</details>

And I think the readability of [`exception/control_flow.md`](https://github.com/InSyncWithFoo/ruff/blob/rk-mdtest-paths/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md) actually goes down quite a bit. In this test suite, the names of the files aren't actually relevant for most of the test cases. The only reason there are generally multiple Python snippets in any one test case is to maintain the flow of the prose; otherwise it would have to be "cut up" with subheadings into too many subheadings. Having `foo.py:` before each Python snippet in that file feels quite distracting to me if you're reading the test file "as documentation" (the aim for that file specifically is that it should double as both a test suite and documentation). I think this would also be improved by having the filenames as magic comments inside the code blocks, but even better might be to continue using the attribute tags so that the filenames aren't rendered at all for the tests in `exception/control_flow.md`. (I'm not sure I agree with @carljm's suggestion in https://github.com/astral-sh/ruff/pull/15704#discussion_r1927955384 that we should remove the existing way of naming files.)

The objections (https://github.com/astral-sh/ruff/pull/15704#discussion_r1928010098 and https://github.com/astral-sh/ruff/pull/15704#discussion_r1929054871) to magic comments seem mostly to be that they don't stand out enough on the page when rendered. But I think that could be remedied by using all-caps?

<details>
<summary>Screenshot of what all-caps comments would look like</summary>

![image](https://github.com/user-attachments/assets/46a61256-147c-43a1-826c-5c9292e31cd6)

</details>

---

_Comment by @MichaReiser on 2025-01-24 19:49_

My 2 cents before heading out. I like `test.py`. @AlexWaygood makes a good point that it can be distracting in tests where the file names aren't relevant, in which case I'd keep support for the old notation and leave it up to the test author to pick the best format. 

---

_Comment by @carljm on 2025-01-24 19:56_

I don't like "meaningless" filenames as a hack to allow multiple tests under a single heading to begin with; I think they are also distracting when reading the test in source form, which is equally (if not more) important IMO. I do recognize the problem that is working around (that sometimes having every test file under its own heading breaks up the flow of the prose too much.) I almost wonder if the most natural solution to that is to actually consider every prose text paragraph a break between tests? I don't think I've seen a case yet where we had a true multi-file test, and needed individual prose comments on different files in the multi-file test, in every case I can find we just have some prose commentary and then all the file contents listed one after the other.

---

_Comment by @AlexWaygood on 2025-01-24 20:01_

> I almost wonder if the most natural solution to that is to actually consider every prose text paragraph a break between tests? I don't think I've seen a case yet where we had a true multi-file test, and needed individual prose comments on different files in the multi-file test, in every case I can find we just have some prose commentary and then all the file contents listed one after the other.

That's a good idea. Should we do that first, in that case? I would prefer not to merge a readability regression in `exception/control_flow.md`

---

_Comment by @dcreager on 2025-01-24 20:02_

> I almost wonder if the most natural solution to that is to actually consider every prose text paragraph a break between tests?

That or allow multiple files with an unspecified filename, and consider each a separate test.  (Each with an identical copy of all files that have paths specified)

---

_Comment by @carljm on 2025-01-24 20:02_

My concern about keeping two ways to specify the path is that I think it will be not be obvious at all to contributors who are less familiar with mdtest which way they should use and what the consequences are. So it will just be one more thing we have to watch out for in code reviews, or else people will just copy some example and then we'll have meaningful file names missing from rendered output.

I also don't have any problem with the rendering of the empty-file cases in this PR; I think it appropriately emphasizes the important information to the reader, and it's worth a bit of vertical space for that.

I agree that all-caps `PATH:` can make the comment-format path stand out better, but I also find that ugly and repetitive.

---

_Comment by @carljm on 2025-01-24 20:03_

> That or allow multiple files with an unspecified filename, and consider each a separate test. (Each with an identical copy of all files that have paths specified)

Oh, I like that idea! Just make the "default filename" include a counter (`test1.py`, `test2.py`) instead of being a fixed name, and I think it would naturally fall out with the right behavior.

---

_Comment by @carljm on 2025-01-24 20:05_

> Should we do that first, in that case? I would prefer not to merge a readability regression in `exception/control_flow.md`

What is the concrete consequence of a small and temporary readability regression, in documentation that currently very few people are reading in rendered form?

I think it would be totally fine to merge this PR as-is and do that as a follow-up. Unless @InSyncWithFoo is up for including that change in this PR, which is also great.

---

_Comment by @AlexWaygood on 2025-01-24 20:15_

> What is the concrete consequence of a small and temporary readability regression

I agree that it's not a _huge_ regression, but as a general principle, I prefer never to merge regressions even if it's been promised that there will be an immediate followup to fix the regression. I know that I myself have in the past promised to do immediate followups, only to find out that there was some unexpected complication that made the followup much harder than anticipated :-)

> in documentation that currently very few people are reading in rendered form?

I use GitHub for browsing through our code (including our tests) all the time. I think a lot of people browse code in this way.

---

_Comment by @InSyncWithFoo on 2025-01-24 23:17_

> I do agree [...] that it would have been great to have a design discussion on the issue before going ahead and implementing this. If we now decide that we'd prefer a different design, it will mean an unnecessary amount of work for you. And it actually makes us feel bad for asking you to change your design now, because we're aware that it will mean more work for you :-)

Please don't be. I did this entirely of my own free will. That said, I'm learning the ropes, so it's actually me who should feel bad, given how much time all of you have spent to review my PRs. I believe it's better for me to practice more and not less, so feel free to tell me to rewrite; I'm always appreciative of every one of your reviews, however the PRs might turn out to be.

> It would also be great to add more context to your PR summaries. It seems you've thought a lot about why you chose this specific notation but this appearant in the PR's summary. This makes it difficult to a) understand the PR because I've to read the code changes to see the new notation and b) avoid discussions that you've already thought about.
>
> [...]
> 
> I do also agree that it would be <em>really</em> nice if you could give more detailed descriptions about the decisions you've taken in your PR summaries. I know that you're always happy to explain the decisions you've taken when we ask you about them, but it's much harder for a second or third reviewer to get up to speed on a PR if they have to read the whole discussion thread in order to understand the PR, rather than just reading the summary in the PR description.

I haven't been explaining my chain of thoughts because I wanted to know your unbiased opinions. I think of my PRs as prototypes on which discussions are built; only through trials can they become true contributions. If I say I considered an option and chose not to follow it, there's a chance, however slightly, you [might not](https://en.wikipedia.org/wiki/Groupthink) ask me to reconsider.

I think that, as a contributor, I'm supposed to present the best I can do, while you, as reviewers, are to give me your <em>harshest</em> comments on how to improve it. Compliments are nice to hear, but criticism even more so, as that proves the PR can still be better.

I do, however, understand that intentions are better expressed in prose. I'll include explanations for each PR from now on. This also means that I'll be counting on you to be as blunt as you could, if you don't mind.

---

Regarding this PR, if I'm not mistaken, the conclusion is to:

* Treat non-consecutive code blocks as different suites (i.e., allow multiple suites within one section)
* Support both ways to specify a path
* Revert the changes made to `exception/control_flow.md` and other files that should use implicit file names

Is that correct?

---

> I use GitHub for browsing through our code (including our tests) all the time. I think a lot of people browse code in this way.

This makes me wonder... should the tests be made into a static site? GFM has its own limit, after all. If they are to be read by people other than who involved with Red Knot's development, then having a pretty UI is perhaps something worth considering.

(I probably wouldn't be surprised if Astral's next goal were to be to revolutionize static site builders.)

---

_Comment by @carljm on 2025-01-25 00:19_

> * Treat non-consecutive code blocks as different suites

I _think_ we (at least it seems Alex and I) prefer @dcreager's suggestion to instead just have the default filename be a counter (`test1.py` for the first unnamed file, `test2.py` for the second, etc), which allows tests with multiple "anonymous" files (when they don't need to import each other), without requiring any other changes. I think this is both easier to implement, and more intuitive.

> * Support both ways to specify a path

I would still strongly prefer to remove the old way. The only arguments I've seen for keeping the old way were 1) to not require updating all tests, and 2) to support tests with "explicit but irrelevant" filenames. (1) is not relevant if this PR does update all tests, and (2) is no longer needed if we make the change to support multiple anonymous files in a single test.

> * Revert the changes made to `exception/control_flow.md` and other files that should use implicit file names

Rather than revert the changes, just remove the unnecessary file names and allow these tests to use the new support for multiple anonymous file names.

The other open question is the preferred new format for the names, which it doesn't seem we have a consensus preference on. Personally my feeling is that in the absence of strong objective arguments or consensus, working code wins (the painter of the bikeshed chooses the color.) But then, I also happen to be happy with the chosen color :) We could also choose another form of resolution, like a poll of team members' preferences.

---

_Comment by @AlexWaygood on 2025-01-25 18:37_

> I do, however, understand that intentions are better expressed in prose. I'll include explanations for each PR from now on. This also means that I'll be counting on you to be as blunt as you could, if you don't mind.

Thank you! I really appreciate it. And thanks again for this PR!

Carl gave a good summary regarding what we do and don't have consensus on currently here

---

_Comment by @sharkdp on 2025-01-25 19:05_

> > * Treat non-consecutive code blocks as different suites
> 
> I _think_ we (at least it seems Alex and I) prefer @dcreager's suggestion to instead just have the default filename be a counter (`test1.py` for the first unnamed file, `test2.py` for the second, etc), which allows tests with multiple "anonymous" files (when they don't need to import each other), without requiring any other changes. I think this is both easier to implement, and more intuitive.

I would actually favor a third option here, which would *concatenate* all code blocks without an explicit path in a single section. This would probably come with some additional checks to avoid confusion for authors of tests. For example, we could forbid multiple code blocks without a path in the presence of code paths *with* paths. And we could forbid multiple code blocks without a path in the presence of code blocks with other languages (pyi, toml, …).

But overall, I think this would allow us to write *actual* literal tests where the prose is simply interposed in the otherwise normal flow of this specific test (which is still identified by a section heading). For example, I recently wrote this:

````markdown
### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals,
`… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are
also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py path=true_and_false.py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```
````

It would be really nice if I didn't have to come up with am irrelevant file name, and if I didn't have to repeat all my includes:

````markdown
### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals,
`… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are
also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py
static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```
````

which would then render like this:

---

### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals, `… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py path=true_and_false.py
static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```

---

I know that @dcreager recently argued that he likes tests that are self-contained, but I think this is a reasonable deviation from this practice. Everything that belongs to a single test is still contained under one and the same heading.

---

_Comment by @AlexWaygood on 2025-01-25 19:36_

I'd also be okay with @sharkdp's idea for how to tackle mdtests where the filenames are irrelevant. I can see arguments both ways -- test isolation is definitely nice, but I also agree that the boilerplate of all the repeated imports can get tiresome. No strong preference from me between @dcreager's proposal and @sharkdp's.

---

_Comment by @InSyncWithFoo on 2025-01-25 21:01_

How about this, then?

`````markdown
## Test file declaration

This block declares a file implicitly named `test_1.py`:

```py
a = 1
```

This block declares a file explicitly named `foo.py`:

`foo.py`:

```py
b = 2
```

This block's path is `foo.py`, but it does not declare a file.
Instead, its content is appended to that file:

```py path=foo.py
c = 3
```

This block declares another file named `foo.py`, which is invalid:

`foo.py`:

```py
d = 4
```

This block declares a file named `bar.py` but with an extraneous `path=` config,
which is also invalid:

`bar.py`:

```py path=bar.py
e = 5
```

This block declares a file implicitly named `test_2.py`:

```py
f = 6
```
`````

----

Rendered:

> ## Test file declaration
> 
> This block declares a file implicitly named `test_1.py`:
> 
> ```py
> a = 1
> ```
> 
> This block declares a file explicitly named `foo.py`:
> 
> `foo.py`:
> 
> ```py
> b = 2
> ```
> 
> This block's path is `foo.py`, but it does not declare a file.
> Instead, its content is appended to that file:
> 
> ```py path=foo.py
> c = 3
> ```
> 
> This block declares another file named `foo.py`, which is invalid:
> 
> `foo.py`:
> 
> ```py
> d = 4
> ```
> 
> This block declares a file named `bar.py` but with an extraneous `path=` config,
> which is also invalid:
> 
> `bar.py`:
> 
> ```py path=bar.py
> e = 5
> ```
> 
> This block declares a file implicitly named `test_2.py`:
> 
> ```py
> f = 6
> ```

---

Authors will be expected to write their tests as readable as possible. For example, these are valid but discouraged:

`````markdown
## Section

```py
a = 1
```

Relies on automatic numbering:

```py path=test_1.py
b = 2
```
`````

`````markdown
## Section

`foo.py`:

```py
from lorem import ipsum

a = 1
```

`bar.py`:

```py
b = 2
```

```py path=foo.py
from bar import b

ipsum(a)  # Where does this come from?
```
`````

---

Rendered:

> ## Section
> 
> ```py
> a = 1
> ```
> 
> Relies on automatic numbering:
> 
> ```py path=test_1.py
> b = 2
> ```
> 
> ## Section
> 
> `foo.py`:
> 
> ```py
> from lorem import ipsum
> 
> a = 1
> ```
> 
> `bar.py`:
> 
> ```py
> b = 2
> ```
> 
> ```py path=foo.py
> from bar import b
> 
> ipsum(a)  # Where does this come from?
> ```

---

_Comment by @carljm on 2025-01-26 01:24_

I also like @sharkdp 's extension of @dcreager's idea.

I agree with @sharkdp that we should not allow this multi-code-block extension of a single file if there are any explicitly-named files in the test (in that case we should still error if there is more than one anonymous code block), as then it becomes too confusing to quickly visually process which code blocks are part of one split file and which are separate files.

For the same reason, I _don't_ think we should implement the generalization suggested by @InSyncWithFoo, which would use the old non-rendering path syntax to allow extending the contents of any file. I still think we should just remove support for the tag-string path-specification syntax. I don't see any good use for that syntax that doesn't make the rendered file (where the filename disappears) too confusing.

(I could see a case for allowing one or more named files, followed by a single possibly-split anonymous file, in that order. I don't think that would be too confusing to read. But I'm also not sure if we have any tests today that would need that, which suggests we maybe don't need it.)

---

_Comment by @MichaReiser on 2025-02-02 18:57_

@InSyncWithFoo, do you need more information, or is it clear to you how to move this PR forward?

---

_Comment by @InSyncWithFoo on 2025-02-02 23:49_

@MichaReiser I think I got it. I'm closing this and submitting #15890 as a rework.

---

_Closed by @InSyncWithFoo on 2025-02-02 23:49_

---

_Branch deleted on 2025-02-02 23:49_

---

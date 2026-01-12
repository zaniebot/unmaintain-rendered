```yaml
number: 9003
title: add support for formatting reStructuredText code snippets
type: pull_request
state: merged
author: BurntSushi
labels:
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/rest
created_at: 2023-12-05T00:11:01Z
updated_at: 2023-12-05T23:34:34Z
url: https://github.com/astral-sh/ruff/pull/9003
synced_at: 2026-01-10T23:40:55Z
```

# add support for formatting reStructuredText code snippets

---

_Pull request opened by @BurntSushi on 2023-12-05 00:11_

(This is not possible to actually use until https://github.com/astral-sh/ruff/pull/8854 is merged.)

ruff_python_formatter: add reStructuredText docstring formatting support

This commit makes use of the refactoring done in prior commits to slot
in reStructuredText support. Essentially, we add a new type of code
example and look for *both* literal blocks and code block directives.
Literal blocks are treated as Python by default because it seems to be a
[common practice](https://github.com/adamchainz/blacken-docs/issues/195).

That is, literal blocks like this:

```
def example():
    """
    Here's an example::

        foo( 1 )

    All done.
    """
    pass
```

Will get reformatted. And code blocks (via reStructuredText directives)
will also get reformatted:


```
def example():
    """
    Here's an example:

    .. code-block:: python

        foo( 1 )

    All done.
    """
    pass
```

When looking for a code block, it is possible for it to become invalid.
In which case, we back out of looking for a code example and print the
lines out as they are. As with doctest formatting, if reformatting the
code would result in invalid Python or if the code collected from the
block is invalid, then formatting is also skipped.

A number of tests have been added to check both the formatting and
resetting behavior. Mixed indentation is also tested a fair bit, since
one of my initial attempts at dealing with mixed indentation ended up
not working.

I recommend working through this PR commit-by-commit. There is in
particular a somewhat gnarly refactoring before reST support is added.

Closes #8859


---

_Label `docstring` added by @BurntSushi on 2023-12-05 00:11_

---

_Label `formatter` added by @BurntSushi on 2023-12-05 00:11_

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-05 00:11_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-05 00:11_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-12-05 00:13_

---

_Comment by @github-actions[bot] on 2023-12-05 00:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:523 on 2023-12-05 03:04_

Interesting. We'll need to consider this when deciding on how to format docstrings when using `indent_style = 'tab'`. See https://github.com/astral-sh/ruff/issues/8430

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:625 on 2023-12-05 03:14_

The comment here now seems outdated considering that the function no longer returns an action.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:348 on 2023-12-05 03:16_

I may have missed it, but in case there's no such test. Could we add a test that tests mixing restructured text and markdown exmaples in a single docstring? Including when we have an unclosed markdown test.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:676 on 2023-12-05 03:17_

The documentation needs updating, considering that the function no longer returns the unchanged line. 



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:685 on 2023-12-05 03:20_

Does this mean that we'll push to the `VecDeque` even in case where the docstring contains no code exampels? I would be interested in understanding the performance implication of allocating here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:316 on 2023-12-05 03:21_

Do you mean `queue` here?

```suggestion
                        // of the queue to get processed before any other action.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:467 on 2023-12-05 03:26_

I would be interested in your opinion on https://github.com/astral-sh/ruff/issues/8430 after dealing with tab/space indentation in code examples. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:834 on 2023-12-05 03:31_

Nit: I believe we use newlines between fields for most of our code base but I'm okay leaving it as is, considering that this isn't enforced by a formatter. Is it your preference to omit new lines between fields (I also noticed that you omit newlines between control flow blocks).

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:875 on 2023-12-05 03:33_

What's the unit in which the `indent` is measured? Is it the byte length of the indentation string, meaning that `\t` and ` ` space both count as one?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:899 on 2023-12-05 03:34_

This sentence seems incomplete

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:956 on 2023-12-05 03:36_

If I remember correctly, the Regex multithreading performance cost has been mitigated, right? 

We could otherwise use our `Cursor` implementation for parsing out the string. Although skipping the whitespace could be somewhat annoying

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:959 on 2023-12-05 03:38_

I find it unexpected that a function called `code` mutates the lines. It may take me a while to figure out where `self.lines` gets changed due to it. Could we trim the lines already when adding it or is this not possible because we don't know the `min_indent` yet?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1002 on 2023-12-05 03:40_

What happens with empty lines at the end of a docstring (with a unclosed example above). Does it get trimmed by the formatter?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1302 on 2023-12-05 03:46_

Python only supports `\t` and ` ` (space) as valid indentation characters (and `\f` form feeds that reset the indentation). It's unclear to me if supporting all white space is okay because we're inside a docstring or should limit it to Python-whitespace only (the same applies for `indent_with_suffix`, we have `trim_whitespace_start` helper to do so)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1263 on 2023-12-05 03:49_

Nit: I had to think about whether assigning to `line` here is/should impact the `line.chars()` call at the top of the loop. It does not, but I would find it easier to understand if we assign the trimmed result to a new variable instead of mutating `line` in place (by adding a `let mut trimmed = line` at the top)

---

_@MichaReiser approved on 2023-12-05 03:54_

Nice work. 

My only concern is that I find it difficult to assess how the changes introducing the action queue impact performance. 

I think we can get a good answer to this by simply enabling docstring formatting for our benchmarks until we find the time to add dedicated docstring tests. This gives us at least an idea on how expensive the feature is for docstring not containing code examples.

---

_Comment by @MichaReiser on 2023-12-05 03:55_

@zanieb should we remove the formatter label from the PR, considering that the functionality isn't available yet or will we remove the changelog entry manually?

---

_Comment by @charliermarsh on 2023-12-05 03:59_

@MichaReiser - We clean the changelogs manually, so no issue keeping the label on there.

---

_@BurntSushi reviewed on 2023-12-05 13:33_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:523 on 2023-12-05 13:33_

Yeah there may be some reshuffling needed here.

We could also choose not to support reformatting code examples with mixed indentation. That might be simplify some things. Not quite sure.

---

_@BurntSushi reviewed on 2023-12-05 13:42_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:348 on 2023-12-05 13:42_

We don't have Markdown support yet (that's next), but that's a good idea. I'll add those mixed tests when I add Markdown.

However, your suggestion prompted me to add tests that mix doctests and reStructuredText blocks. I think things work there how we'd want them to (doctests inside of literal blocks get skipped, but doctests inside of a `pycon` code block get formatted), but I could also see user feedback leading us to change it.

---

_@BurntSushi reviewed on 2023-12-05 14:04_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:685 on 2023-12-05 14:04_

Yeah it will. Although it will push to the queue for every line, it should only allocate the first time. Even in the case where a code example is present, I don't expect the queue to ever grow beyond more than a couple elements, so the allocation should be amortized. But it's only amortized within each docstring.

It does indeed look like the happy path in the status quo does not allocate. I still perceive this to likely be a marginal cost, but I'll see about adding some benchmarks to this PR.

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:316 on 2023-12-05 14:09_

Yes. I'll swap it out with "queue." I was using "line" as a synonym for "queue" hah. But it is quite confusable in this context. (And it may also be an American meaning? Not sure.)

---

_@BurntSushi reviewed on 2023-12-05 14:09_

---

_@BurntSushi reviewed on 2023-12-05 14:12_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:467 on 2023-12-05 14:12_

Aye. I left a comment. To me it feels like one of the issues here is hard-coding tabwidth=8. So long as that's there, it will be tough for us to cleanly solve the problem.

---

_@BurntSushi reviewed on 2023-12-05 14:18_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:834 on 2023-12-05 14:18_

I do indeed omit empty lines between fields in type definitions. I think I've just perceived that to be the common style, but looking a bit, the standard library seems to use a wild mix haha.

As for control flow blocks, I have less of a consistent style there. I tend to use empty lines when logical groupings make sense. But that's just somewhat of a judgment call. Looking a bit, probably some empty lines would help in places. But no, my preference wouldn't be to always insert an empty line.

With that said, I like consistent styling, so I'll add some line breaks here and try to remember to do so in the future. But yeah, without the formatter doing this for us, it's bound to get out-of-whack.

---

_@BurntSushi reviewed on 2023-12-05 14:48_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:875 on 2023-12-05 14:48_

No, it's computed by `indentation_length`:

```rust
/// For docstring indentation, black counts spaces as 1 and tabs by increasing the indentation up
/// to the next multiple of 8. This is effectively a port of
/// [`str.expandtabs`](https://docs.python.org/3/library/stdtypes.html#str.expandtabs),
/// which black [calls with the default tab width of 8](https://github.com/psf/black/blob/c36e468794f9256d5e922c399240d49782ba04f1/src/black/strings.py#L61).
fn indentation_length(line: &str) -> TextSize {
    let mut indentation = 0u32;
    for char in line.chars() {
        if char == '\t' {
            // Pad to the next multiple of tab_width
            indentation += 8 - (indentation.rem_euclid(8));
        } else if char.is_whitespace() {
            indentation += u32::from(char.text_len());
        } else {
            break;
        }
    }
    TextSize::new(indentation)
}
```

I added a clarifying comment to the docs here.

---

_@BurntSushi reviewed on 2023-12-05 14:51_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:956 on 2023-12-05 14:51_

Mostly mitigated, yeah. And this only runs on lines that start with `.. `. So it should be very infrequently called.

---

_@BurntSushi reviewed on 2023-12-05 14:56_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:959 on 2023-12-05 14:56_

Correct. We don't know the `min_indent`.

I agree that the naming here is not ideal. I don't like it either. I would have liked to define an `into_code` that consumes `self` and spits back the code, but then you lose other parts of `self` that are needed to print the reformatted lines. I could also define a `take_code` instead that plucks the code out of an `&mut self`, which might make the mutation a little less surprising, but then you're left with an empty code example whose other state is still useful. Which is weird. Another choice would be to add a new method, something like, `indent_lines` that does the mutation and then provide the `code` method as a simple accessor. I probably like that approach the best, but it means you can forget to run `indent_lines` and get a bug.

The benefit of this approach is that callers can't get it wrong, which is ultimately what I ended up favoring.

Still, the name isn't great. I changed it to `indented_code` and tweaked the docs a touch. Still not great, but maybe a little better.

---

_@BurntSushi reviewed on 2023-12-05 16:20_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1002 on 2023-12-05 16:20_

Nice test case. I added one:

```python
# If a literal block is never properly ended (via a non-empty unindented line),
# then the end of the block should be the last non-empty line. And subsequent
# empty lines should be preserved as-is.
def rst_literal_extra_blanks_at_end():
    """
    Do cool stuff::


        cool_stuff( 1 )



    """
    pass
```

In this case, just the code snippet should get reformatted to `cool_stuff(1)`, but the empty lines are preserved as they are not part of the code snippet. The reST renderer I'm using seems to agree.

Ah, but I figured you were noticing something awry, so I revisited the logic with this prompt in mind and indeed, a bug has been found! Namely, in this code:

```
# A literal block can contain many empty lines and it should not end the block
# if it continues.
def rst_literal_extra_blanks_in_snippet():
    """
    Do cool stuff::

        cool_stuff( 1 )


        cool_stuff( 2 )

    Done.
    """
    pass
```

`cool_stuff( 1 )` will get formatted, but `cool_stuff( 2 )` will not because the block is prematurely closed.

OK, fixed that while not regressing the first test.

---

_@BurntSushi reviewed on 2023-12-05 16:30_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1302 on 2023-12-05 16:30_

Hmmm. I see. I hadn't known about this or the `trim_whitespace_start` helper.

One possible issue I see here is that the docstring whitespace normalization uses just the bare `trim_start()` and `trim_end()` routines in places. Is that correct, or should those be switched to the Python specific definition too? Same deal with `indentation_length`. It uses `char::is_whitespace` from std, which covers all the Unicode forms of whitespace too.

In any case, I've updated my uses of whitespace to `trim_whitespace_start` and `is_python_whitespace`. (Probably that module should get re-tooled a bit. i.e., Add an extension trait for `char` and rename `trim_whitespace_start` and assorted methods to `trim_python_start`.)

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1263 on 2023-12-05 16:34_

Ah yeah you'd get a borrow checker error if it were otherwise. The key is that we're just re-binding a name.

In any case, I've added a new `trimmed` variable.

---

_@BurntSushi reviewed on 2023-12-05 16:34_

---

_Comment by @BurntSushi on 2023-12-05 19:12_

In lieu of micro-benchmarks, I decided to do a little ad hoc benchmarking with hyperfine on [dagster](https://github.com/dagster-io/dagster).

Using this branch with a cherry-pick of #8854, I ran formatting with the default config and formatting with `format-code-in-docstrings` enabled:

```
$ hyperfine \
    --warmup 3 \
    --prepare 'git reset --hard master' \
    --cleanup 'git reset --hard master' \
    'ruff format --config /tmp/emptyruff.toml ./' \
    'ruff format --config /tmp/ruff.toml ./'
Benchmark 1: ruff format --config /tmp/emptyruff.toml ./
  Time (mean ± σ):      84.4 ms ±   1.8 ms    [User: 1084.2 ms, System: 155.3 ms]
  Range (min … max):    80.8 ms …  88.2 ms    16 runs

Benchmark 2: ruff format --config /tmp/ruff.toml ./
  Time (mean ± σ):      85.1 ms ±   2.1 ms    [User: 1104.4 ms, System: 160.0 ms]
  Range (min … max):    82.1 ms …  89.5 ms    16 runs

Summary
  ruff format --config /tmp/emptyruff.toml ./ ran
    1.01 ± 0.03 times faster than ruff format --config /tmp/ruff.toml ./
```

Where:

```
$ git remote -v
origin  git@github.com:dagster-io/dagster (fetch)
origin  git@github.com:dagster-io/dagster (push)

$ git rev-parse HEAD
dbb064c2ddda74265b8174edd9775e1302ca6ba0

$ cat /tmp/emptyruff.toml
$ cat /tmp/ruff.toml
[format]
format-code-in-docstrings = true
```

I ran it multiple times, and the same result occurred. So there is just a very slight observable slow-down here. But, this particular pile of code does have a large number of reStructuredText code blocks. So you'd expect it to possibly run a little more slowly since it is doing more work.

On a profile, I can see `run_action_queue`, but it is barely a blip. To be sure, I tweaked the top-level entry point for code snippet formatting to do this:

```rust
if !self.code_example.kind.is_none()
    || CodeExampleDoctest::new(line).is_some()
    || CodeExampleRst::new(line).is_some()
{
    self.code_example.add(line, &mut self.action_queue);
    self.run_action_queue()
} else {
    self.print_one(&line.as_output())
}
```

So basically, as long as we weren't already collecting a code example and the current line didn't look like the start of one, then we could avoid the queue and just print the line directly. I then baked this off against what I had:

```
$ hyperfine \
    --warmup 3 \
    --prepare 'git reset --hard master' \
    --cleanup 'git reset --hard master' \
    'ruff-rst-formatting-with-queue format --config /tmp/emptyruff.toml ./' \
    'ruff-rst-formatting-with-queue format --config /tmp/ruff.toml ./' \
    'ruff-rst-formatting-with-fast-path format --config /tmp/ruff.toml ./'
Benchmark 1: ruff-rst-formatting-with-queue format --config /tmp/emptyruff.toml ./
  Time (mean ± σ):      84.9 ms ±   2.2 ms    [User: 1077.6 ms, System: 163.4 ms]
  Range (min … max):    80.1 ms …  89.4 ms    17 runs

Benchmark 2: ruff-rst-formatting-with-queue format --config /tmp/ruff.toml ./
  Time (mean ± σ):      85.2 ms ±   2.5 ms    [User: 1119.1 ms, System: 148.7 ms]
  Range (min … max):    81.7 ms …  92.3 ms    17 runs

Benchmark 3: ruff-rst-formatting-with-fast-path format --config /tmp/ruff.toml ./
  Time (mean ± σ):      86.0 ms ±   2.2 ms    [User: 1098.4 ms, System: 163.7 ms]
  Range (min … max):    81.4 ms …  89.7 ms    16 runs

Summary
  ruff-rst-formatting-with-queue format --config /tmp/emptyruff.toml ./ ran
    1.00 ± 0.04 times faster than ruff-rst-formatting-with-queue format --config /tmp/ruff.toml ./
    1.01 ± 0.04 times faster than ruff-rst-formatting-with-fast-path format --config /tmp/ruff.toml ./
```

I ran this a few times and sometimes the fast path would be a hair faster and other times it would be flipped. Running _without_ docstring formatting enabled at all was consistently faster by 1.00-1.01 times.

I think this satisfies me personally that the queue overhead is probably negligible, at least until we have some data suggesting otherwise. (I'm sure there are more synthetic benchmarks one could construct that might show a bigger difference.)

---

_Comment by @BurntSushi on 2023-12-05 19:14_

Going to bring this, but @MichaReiser feel free to leave more feedback and I'll address it in a follow-up PR. :-)

---

_Merged by @BurntSushi on 2023-12-05 19:14_

---

_Closed by @BurntSushi on 2023-12-05 19:14_

---

_Branch deleted on 2023-12-05 19:14_

---

_Comment by @MichaReiser on 2023-12-05 23:34_

Thanks for running the manual benchmark. This looks good to me.

---

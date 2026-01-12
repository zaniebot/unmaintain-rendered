```yaml
number: 3440
title: Add basic jupyter notebook support
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: jupyter-notebook-support
created_at: 2023-03-10T13:51:00Z
updated_at: 2023-03-20T14:41:02Z
url: https://github.com/astral-sh/ruff/pull/3440
synced_at: 2026-01-12T15:55:12Z
```

# Add basic jupyter notebook support

---

_@konstin_

This feature is disabled by default for now behind the `jupyter_notebook` feature.

Missing:
 * do something about autofixing
 * writing back notebooks while making sure we don't change them in the process (round trip test)

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/schema.rs`:7 on 2023-03-15 02:33_

What manual edits? Could we separate the manual edits into their own commit, so that even if the PR is squashed, we have access to the history here?

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:199 on 2023-03-15 02:33_

Cool - so this will go behind a feature flag as discussed synchronously?

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:63 on 2023-03-15 02:34_

Nit: we tend to use the expanded version everywhere (I don't know why, but it is the convention).

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:101 on 2023-03-15 02:35_

Should this be returning some kind of error diagnostic?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:790 on 2023-03-15 02:37_

How does nbqa format / convey these?

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:97 on 2023-03-15 02:38_

Can we add some tests for this? It looks unit-testable.

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:57 on 2023-03-15 02:40_

Nit: IMO we should capitalize as `Jupyter Notebook`, `JSON`, and `Python` for any user-facing text. (For "correctness", but also for consistency with other user-facing text.)

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:72 on 2023-03-15 02:41_

Mind adding a comment here to explain what format 4 is vs. other values that this might be?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:31 on 2023-03-15 02:42_

Rather than passing around the index... I'm wondering if we should instead change `Message` to have a more flexible location type, and store the location data on `Message` directly. This isn't a fully-formed idea, but it seems non-ideal that clients of `Diagnostics` need to do this translation, worry about what's a notebook, what isn't, etc. Could `Message` instead have a `Location` that's an enum or similar, and can be (e.g.) a Python file location or a Jupyter notebook location? What would be the tradeoffs?

In other words, this feels like exposing an implementation detail, since clients only care about the location, and not that translation step. What do you think?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:39 on 2023-03-15 02:42_

Any reason not to use `FxHashMap`?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:213 on 2023-03-15 02:44_

Do you mind using the `TODO(konsti)`-style TODOs? I like to have authors indicated in the TODOs, not as a sign that they're responsible for fixing them, but as a guidepost for who wrote it and has context on it.

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:54 on 2023-03-15 02:45_

Oh interesting, so this is checking that, in the event that we failed to parse, did the user accidentally save a Python file as a `.ipynb` file?

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:106 on 2023-03-15 02:46_

Is there any way to hint the size here? (Maybe not!)

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/jupyter.rs`:101 on 2023-03-15 02:47_

What's the motivation for 1-based-to-1-based, rather than 0-based to 0-based? Seems like it could potentially cause confusion to have this 0 padding.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:94 on 2023-03-15 02:49_

Idly wondering if we should tag files by dialect in the `resolver`, and pass around tagged files, rather than checking extensions on `Path` everywhere. Same goes for the `.pyi` files. (Maybe not actionable, just a rough thought.)

---

_@charliermarsh reviewed on 2023-03-15 02:51_

Sweet! Impressive that you were able to tease apart the existing codepaths to get this far. I know we chatted about a few changes here earlier (feature flag, maybe using diagnostics in lieu of some of the current error handling), but thought I'd drop some comments on current state :)

---

_@konstin reviewed on 2023-03-15 07:56_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:63 on 2023-03-15 07:56_

tbh i'm a big fan of indoc everywhere, but yes consistency

---

_@konstin reviewed on 2023-03-15 07:58_

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:101 on 2023-03-15 07:58_

updated the comment, this could e.g. be an R notebook which we want to just skip

---

_@konstin reviewed on 2023-03-15 08:54_

---

_Review comment by @konstin on `crates/ruff_cli/src/printer.rs`:790 on 2023-03-15 08:54_

```
$ nbqa ruff /home/konsti/ruff/crates/ruff/resources/test/fixtures/jupyter/
crates/ruff/resources/test/fixtures/jupyter/valid.ipynb:cell_1:2:5: F841 [*] Local variable `x` is assigned to but never used
Found 1 error.
[*] 1 potentially fixable with the --fix option.

nbQA failed to process /home/konsti/ruff/crates/ruff/resources/test/fixtures/jupyter/not_json.ipynb with exception "JSONDecodeError('Expecting value: line 1 column 1 (char 0)')"
nbQA failed to process /home/konsti/ruff/crates/ruff/resources/test/fixtures/jupyter/invalid_extension.ipynb with exception "JSONDecodeError('Expecting value: line 1 column 1 (char 0)')"
nbQA failed to process /home/konsti/ruff/crates/ruff/resources/test/fixtures/jupyter/wrong_schema.ipynb with exception "KeyError('cells')"

If you believe the notebook(s) to be valid, please report a bug at https://github.com/nbQA-dev/nbQA/issues 
```

Underscore instead of space but otherwise same format

---

_Review comment by @konstin on `crates/ruff/src/jupyter/jupyter.rs`:72 on 2023-03-15 08:56_

tbh i've only seen v4 in the wild

---

_@konstin reviewed on 2023-03-15 08:56_

---

_@JonathanPlasse reviewed on 2023-03-15 08:57_

---

_Review comment by @JonathanPlasse on `crates/ruff_cli/src/commands/run.rs`:213 on 2023-03-15 08:57_

What about git blame? Is it for the case of refactoring?

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:31 on 2023-03-15 09:02_

i'm not completely sure either; my current thought is that most code can just assume to see one consistent python file and not bother with everything else and a special case such as jupyter notebooks gets patched in the appropriate places.

making `location` and `end_location` an enum of `Location(Location)` and `JupyterLocation { cell: u32, row: u32, column: u32}` would definitely also be possible

---

_@konstin reviewed on 2023-03-15 09:02_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/jupyter.rs`:54 on 2023-03-15 09:04_

yes

---

_@konstin reviewed on 2023-03-15 09:04_

---

_@konstin reviewed on 2023-03-15 09:14_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/jupyter.rs`:106 on 2023-03-15 09:14_

i've also checked that `.join("\n")` does size hints internally

---

_Review comment by @konstin on `crates/ruff/src/jupyter/jupyter.rs`:101 on 2023-03-15 09:16_

mainly that everywhere else uses 1-based indices (current way means no conversions, i would have to introduce some otherwise)

---

_@konstin reviewed on 2023-03-15 09:16_

---

_@konstin reviewed on 2023-03-15 09:19_

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:94 on 2023-03-15 09:19_

maybe a  `enum FileInfo { Py, PyI, Jupyter { index: JupyterIndex }}`, we could also put that in a hash map `Path` (or string) -> `FileInfo` that we read when collecting the results

---

_@konstin reviewed on 2023-03-15 09:26_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/schema.rs`:7 on 2023-03-15 09:26_

i've added comments on what i changed

---

_@konstin reviewed on 2023-03-15 11:09_

---

_Review comment by @konstin on `crates/ruff/src/resolver.rs`:199 on 2023-03-15 11:09_

yes, in `is_jupyter_notebook`

---

_@konstin reviewed on 2023-03-15 11:43_

---

_Review comment by @konstin on `crates/ruff_cli/src/printer.rs`:1 on 2023-03-15 11:43_

this is a mess unfortunately

---

_Comment by @konstin on 2023-03-15 11:44_

Thank you for the detailed review!

---

_Marked ready for review by @konstin on 2023-03-15 11:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/schema.rs`:44 on 2023-03-15 12:38_

Is the difference between an empty `HashMap` (and an empty `Vec` for `outputs`) important or can we change the types to `HashMap<String<HashMap<String, SourceValue>>` and use `skip_if=HashMapp::is_empty`?

The same applies for other `Option<Collection>` usages in this file

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:26 on 2023-03-15 12:39_

Nit: We could use `u32` to reduce memory usage

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:134 on 2023-03-15 12:44_

Nit: I didn't expect that the function would filter out non-python notebooks from the name `read_jupyter_notebook`. Maybe add another function `read_python_jupyter_notebook()`

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:147 on 2023-03-15 12:45_

Nit: I expected this function to concat two notebooks. What do you think of adding a `index()` function on `JupyterNotebook`?

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:191 on 2023-03-15 12:46_

Would it cause much downstream issues to add a lifetime to Index` and use a `Cow` to store the content (to avoid cloning the source value?)

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:199 on 2023-03-15 12:48_

What's your preference of creating standalone functions vs adding them as methods to `JupyterNotebook`. E.g. 

```
let notebook = JupyterNotebook::try_from_path(...)?;
notebook.write(path);
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:106 on 2023-03-15 12:49_

Does  `Diagnostics` implement `Default` or can we add a `Default` implementation. This then becomes `Ok(Diagnostics::default())`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:119 on 2023-03-15 12:50_

You can simplify this if `Diagnostics` implements `Default`
```suggestion
                return Ok(Diagnostics {
                	..Diagnostics::default(),
                	messages: vec![Message::from_diagnostic(
                        *diagnostic,
                        path.to_string_lossy().to_string(),
                        None,
                        1,
                    )],
                });
```

---

_@MichaReiser approved on 2023-03-15 12:50_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:1 on 2023-03-15 12:51_

Yeah, I think we should prioritize reworking our Diagnostics representations. We're hitting rough edges in many places. 

---

_@MichaReiser reviewed on 2023-03-15 12:51_

---

_Review comment by @MichaReiser on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-03-15 12:52_

Did you profile if setting the `size_hint` outperforms iterating over the code cells?

---

_@MichaReiser approved on 2023-03-15 12:53_

Excellent work and thanks for taking the time to document the data structures. 

Could we add one or two jupyter notebooks to our benchmark so that we have good numbers from the beginning?

---

_@konstin reviewed on 2023-03-15 16:33_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/schema.rs`:44 on 2023-03-15 16:33_

afaik it saying the field to be missing vs. the field is present but empty. it believe it makes a difference for round-trip serialization and json schema compliance

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:26 on 2023-03-15 16:39_

is there are a smart way to convert usize -> u32? because clippy complains for e.g. for `string.lines().count() as u32`

> casting `usize` to `u32` may truncate the value on targets with 64-bit wide pointers

---

_@konstin reviewed on 2023-03-15 16:39_

---

_@konstin reviewed on 2023-03-15 17:14_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:147 on 2023-03-15 17:14_

good point, i renamed it `index_notebook`

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:191 on 2023-03-15 17:17_

the contents string itself we have to build anew and the index doesn't have any string contents. i'd have hoped that the current code is already compiler friendly enough that the content string building is fast enough not to show up in any flamegraphs

---

_@konstin reviewed on 2023-03-15 17:17_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-03-15 17:26_

no i did that based on charlie's comment above; i currently also don't have any large enough jupyter notebook repo to make relevant tests

---

_@konstin reviewed on 2023-03-15 17:26_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/notebook.rs`:199 on 2023-03-15 17:30_

didn't think about that, that is much nicer, thanks

---

_@konstin reviewed on 2023-03-15 17:30_

---

_Comment by @konstin on 2023-03-15 17:37_

> Could we add one or two jupyter notebooks to our benchmark so that we have good numbers from the beginning?

i'll add benchmarks in the next round, this is already large

---

_Comment by @konstin on 2023-03-15 17:37_

Thank to you too for the detailed review!

---

_Comment by @github-actions[bot] on 2023-03-15 18:02_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@charliermarsh reviewed on 2023-03-15 23:48_

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:65 on 2023-03-15 23:48_

(We need to do this for `logical_lines` too I think, not part of this PR, just a note to self...)

---

_@charliermarsh reviewed on 2023-03-15 23:50_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:222 on 2023-03-15 23:50_

Should we use `relativize_path` here for consistency with `fixed`, below?

---

_@charliermarsh reviewed on 2023-03-15 23:51_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:790 on 2023-03-15 23:51_

Should we use an underscore? Wdyt? It might play better with regex parsers that rely on the structure of this output.

---

_@charliermarsh reviewed on 2023-03-15 23:52_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:1 on 2023-03-15 23:52_

I'd def support a PR that improves the Diagnostic representation with this generalization in mind.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:72 on 2023-03-15 23:52_

Nit: `Python` for user-facing messages.

---

_@charliermarsh reviewed on 2023-03-15 23:52_

---

_@charliermarsh reviewed on 2023-03-15 23:54_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:94 on 2023-03-15 23:54_

Does this manifest itself in practice? Like, what path / usage hits this?

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/schema.rs`:8 on 2023-03-15 23:56_

For whatever reason I tend to use "we" instead of "I" in code comments, I don't know why :joy:

---

_@charliermarsh reviewed on 2023-03-15 23:56_

---

_@charliermarsh approved on 2023-03-15 23:56_

🎸 🎸 🎸 

---

_@charliermarsh reviewed on 2023-03-15 23:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/jupyter/schema.rs`:8 on 2023-03-15 23:56_

I guess to me it conveys that we're all responsible for whatever is being written / committed

---

_@konstin reviewed on 2023-03-20 10:15_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/run.rs`:94 on 2023-03-20 10:15_

not anymore since i switched to diagnostics, but i would keep it to avoid hiding source errors that some `?` might produce. with io errors i always found it helpful to have to have the chain to being able to tell what happened

---

_@konstin reviewed on 2023-03-20 10:23_

---

_Review comment by @konstin on `crates/ruff/src/jupyter/schema.rs`:8 on 2023-03-20 10:23_

i normally use we for general explanations and i when i did something opionated, but i agree with the ~~royal~~ [mathematical](https://math.stackexchange.com/a/447722) we over i. here i think we can get away with passive voice.

---

_Comment by @github-actions[bot] on 2023-03-20 10:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.4±0.04ms     3.0 MB/sec    1.00     13.3±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    481.1±1.01µs     6.1 MB/sec    1.00    480.7±1.06µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.01      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1608.0±4.39µs    10.4 MB/sec    1.00   1592.9±1.65µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±0.32µs    16.4 MB/sec    1.01    181.2±0.65µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.14ms     2.7 MB/sec    1.03     15.2±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.1 MB/sec    1.02      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    519.0±7.51µs     5.7 MB/sec    1.02    528.4±5.75µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.03      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     5.0 MB/sec    1.04      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1800.0±14.73µs     9.3 MB/sec    1.04  1863.1±11.28µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.9±2.50µs    14.7 MB/sec    1.05    210.1±5.84µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.04      3.9±0.04ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @konstin on 2023-03-20 11:06_

---

_Closed by @konstin on 2023-03-20 11:06_

---

_Branch deleted on 2023-03-20 11:06_

---

_Comment by @blink1073 on 2023-03-20 14:41_

Nice, thanks @konstin!

---

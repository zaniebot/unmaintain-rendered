```yaml
number: 20036
title: Improve diff rendering for notebooks
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/notebook-diff
created_at: 2025-08-21T23:39:49Z
updated_at: 2025-08-25T13:20:43Z
url: https://github.com/astral-sh/ruff/pull/20036
synced_at: 2026-01-10T17:46:21Z
```

# Improve diff rendering for notebooks

---

_Pull request opened by @ntBre on 2025-08-21 23:39_

## Summary

As noted in a code TODO, our `Diff` rendering code previously didn't have any
special handling for notebooks. This was particularly obvious when the diffs
were rendered right next to the corresponding diagnostic because the diagnostic
used cell-based line numbers, while the diff was still using line numbers from
the concatenated source. This PR updates the diff rendering to handle notebooks
too.

The main improvements shown in the example below are:

- Line numbers are now remapped to be relative to their cell
- Context lines from other cells are suppressed

```
error[unused-import][*]: `math` imported but unused                             
 --> notebook.ipynb:cell 2:2:8                                                  
  |                                                                             
1 | # cell 2                                                                    
2 | import math                                                                 
  |        ^^^^                                                                 
3 |                                                                             
4 | print('hello world')                                                        
  |                                                                             
help: Remove unused import: `math`                                              
                                                                                
ℹ Safe fix                                                                      
1 1 | # cell 2                                                                  
2   |-import math                                                               
3 2 |                                                                           
4 3 | print('hello world')                                                      
```

I tried a few different approaches here before finally just splitting the notebook into separate text ranges by cell and diffing each one separately. It seems to work and passes all of our tests, but I don't know if it's actually enforced anywhere that a single edit doesn't span cells. Such an edit would silently be dropped right now since it would fail the `contains_range` check. I also feel like I may have overlooked an existing way to partition a file into cells like this.

## Test Plan

Existing notebook tests, plus a new one in `ruff_db`


---

_Label `diagnostics` added by @ntBre on 2025-08-21 23:39_

---

_Label `internal` added by @ntBre on 2025-08-21 23:39_

---

_Comment by @github-actions[bot] on 2025-08-21 23:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2025-08-22 05:02_

> It seems to work and passes all of our tests, but I don't know if it's actually enforced anywhere that a single edit doesn't span cells.

It is enforced, just not in a good way ;)

Related issue: https://github.com/astral-sh/ruff/issues/14445

https://github.com/astral-sh/ruff/blob/7a44ea680e55f10af6e566eb5d9928244f79edd1/crates/ruff_notebook/src/notebook.rs#L278-L284

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:115 on 2025-08-22 07:46_

A few thoughts here:

* Iterating over all lines seems especially unfortunate if this isn't a notebook. Can't we just push a single entry if the notebook index is absent that spans the entire source code range instead of iterating over the lines?
* Are there cases where the notebook cells don't appear in source order? I'm asking because we don't need a `BTreeMap` if it's guaranteed that we iterate over the cells in order anyway
* This data structure seems very similar to `Notebook::cell_offsets`. 



---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:148 on 2025-08-22 07:49_

I wonder if we could reduce the overhead for non notebooks by extracing the shared logic for rendering fixes into a separate method and:

* We call that method directly if source isn't a notebook (in which case we can skip all/most of the operations?)
* For notebooks, do whatever mapping is necessary, call that method for each cell

---

_@MichaReiser reviewed on 2025-08-22 07:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__unused_variable.snap`:50 on 2025-08-22 07:53_

I think we have to show the cell number somewhere because I don't think it's guaranteed that the diagnostic cell is the same as the one for fixes.

I'm also curious on how the rendering looks if a diagnostic has fixes across multiple cells. We should add a test for this. 

The UP049 example that you used to evaluate the fixes rendering could be a good example here too, just extend it so that the fixes span multiple cells.

---

_@MichaReiser reviewed on 2025-08-22 07:53_

---

_@ntBre reviewed on 2025-08-22 12:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__unused_variable.snap`:50 on 2025-08-22 12:51_

Oh that's a good point, in all of these cases the cell number matched. What do you think about the header lines I removed in this commit: 80f9b55621ba0c42e66a659dc47c0c66c2e80229? I was trying to match the horizontal line we draw for different sections, but I ended up ripping them out because I thought they were too noisy, especially for the single-cell cases in our tests. Another option I considered was something like the `:::` in our diagnostics:

```
error[unused-import]: `os` imported but unused
 --> notebook.ipynb:cell 1:2:8                
  |                                           
1 | # cell 1                                  
2 | import os                                 
  |        ^^                                 
  |                                           
 ::: notebook.ipynb:cell 2:2:8                
  |                                           
1 | # cell 2                                  
2 | import math                               
  |        ---- second cell                   
3 |                                           
4 | print('hello world')                      
  |                                           
help: Remove unused import: `os`              
```

I'll work on a test for these. Unfortunately I don't think we can use UP049 because the PEP-695 generics are restricted to a single class or function body, which I don't think can span cells. I'm sure I can find another rule, though.

---

_@MichaReiser reviewed on 2025-08-22 13:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/snapshots/ruff_linter__linter__tests__unused_variable.snap`:50 on 2025-08-22 13:01_

I like `::: notebook.ipynb:cell 2:2:8` except that I would strip everything except the cell number because it feels redundant

```
--> notebook.ipynb:cell 1:2:8                
  |                                           
1 | # cell 1                                  
2 | import os                                 
  |        ^^                                 
  |                                           
 ::: cell 2
  |                                           
1 | # cell 2                                  
2 | import math                               
  |        ---- second cell                   
3 |                                           
4 | print('hello world')                      
  |                                           
help: Remove unused import: `os`              
```

---

_@ntBre reviewed on 2025-08-22 15:34_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/full.rs`:115 on 2025-08-22 15:34_

Ah thanks, `Notebook::cell_offsets` does seem like what I need. I looked into adding a `FileResolver` method to get a `CellOffsets`, but it only works, or at least works easily, for ty files. Ruff's `FileResolver` only has access to a `NotebookIndex`, not the `Notebook` itself. Should I try to pass that down through `Diagnostics`, and `EmitterContext`?

I don't think we want to clone a whole `Notebook`, so I'm thinking of adding a new struct wrapping only the index and the cell offsets to store in the (renamed) `Diagnostics::notebook_indexes` field.

https://github.com/astral-sh/ruff/blob/0b6ce1c788bceea13afeac63ec25894a5edd4435/crates/ruff/src/diagnostics.rs#L338-L342

I guess we could also considering moving the `CellOffsets` into the `NotebookIndex`, making it the wrapper struct. They're mostly accessed via `Notebook::cell_offsets` anyway.

But yes, we definitely don't need to iterate the lines for a normal file.

---

_@ntBre reviewed on 2025-08-22 15:41_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/full.rs`:115 on 2025-08-22 15:41_

Another idea, I think we can reconstruct something more efficient than looping over all the lines by only looking at the unique entries in `NotebookIndex::row_to_cell`. That's probably the easiest thing to do.

---

_Comment by @ntBre on 2025-08-22 22:54_

I've updated the output formatting, and I believe also resolved Micha's other suggestions. As I mentioned [here](https://github.com/astral-sh/ruff/pull/20036#discussion_r2294063712) it seems a bit tricky to get access to a `Notebook` to call `Notebook::cell_offsets` directly, but I think I've reconstructed something similar here.

I didn't extract any code as suggested [here](https://github.com/astral-sh/ruff/pull/20036#discussion_r2292995749), but I think most of the overhead for non-notebooks, and for notebooks too really, should be resolved by getting rid of the `BTreeMap` and the loop over all of the source lines.

---

_Marked ready for review by @ntBre on 2025-08-22 23:04_

---

_Review requested from @carljm by @ntBre on 2025-08-22 23:04_

---

_Review requested from @sharkdp by @ntBre on 2025-08-22 23:04_

---

_Review requested from @dcreager by @ntBre on 2025-08-22 23:04_

---

_Review requested from @dhruvmanila by @ntBre on 2025-08-22 23:04_

---

_Review request for @dcreager removed by @ntBre on 2025-08-22 23:04_

---

_Review request for @carljm removed by @ntBre on 2025-08-22 23:04_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-22 23:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:716 on 2025-08-23 12:47_

Not something we have to change as part of this PR. But I only just realized that our fix infrastructure always assumes that all changes belong to the file from the primary span. 

How would the rendered diagnostic look like if we had a sub-diagnostic with a span pointing to a different file? I don't think it's something we have to solve now but it might be worth documenting if the rendered output looks confusing.

---

_@MichaReiser approved on 2025-08-23 12:48_

Nice

---

_@ntBre reviewed on 2025-08-25 13:20_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/full.rs`:716 on 2025-08-25 13:20_

Opened an issue to track this here: https://github.com/astral-sh/ruff/issues/20080.

---

_Merged by @ntBre on 2025-08-25 13:20_

---

_Closed by @ntBre on 2025-08-25 13:20_

---

_Branch deleted on 2025-08-25 13:20_

---

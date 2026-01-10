```yaml
number: 19698
title: "[ty] Remap Jupyter notebook cell indices in `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: brent/full-jupyter
created_at: 2025-08-01T20:44:16Z
updated_at: 2025-08-05T18:10:37Z
url: https://github.com/astral-sh/ruff/pull/19698
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Remap Jupyter notebook cell indices in `ruff_db`

---

_Pull request opened by @ntBre on 2025-08-01 20:44_

## Summary

This PR remaps ranges in Jupyter notebooks from simple `row:column` indices in the concatenated source code to `cell:row:col` to match Ruff's output. This is probably not a likely change to land upstream in `annotate-snippets`, but I didn't see a good way around it.

The remapping logic is taken nearly verbatim from here:

https://github.com/astral-sh/ruff/blob/cd6bf1457d667dd17058e07fa4198dae4bfb75f6/crates/ruff_linter/src/message/text.rs#L212-L222


## Test Plan

New `full` rendering test for a notebook

I was mainly focused on Ruff, but in local tests this also works for ty:

```
error[invalid-assignment]: Object of type `Literal[1]` is not assignable to `str`
 --> Untitled.ipynb:cell 1:3:1
  |
1 | import math
2 |
3 | x: str = 1
  | ^
  |
info: rule `invalid-assignment` is enabled by default

error[invalid-assignment]: Object of type `Literal[1]` is not assignable to `str`
 --> Untitled.ipynb:cell 2:3:1
  |
1 | import math
2 |
3 | x: str = 1
  | ^
  |
info: rule `invalid-assignment` is enabled by default
```

This isn't a duplicate diagnostic, just an unimaginative example:

```py
# cell 1
import math

x: str = 1
# cell 2
import math

x: str = 1
```


---

_Label `ty` added by @ntBre on 2025-08-01 20:44_

---

_Label `diagnostics` added by @ntBre on 2025-08-01 20:44_

---

_@ntBre reviewed on 2025-08-01 20:46_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:899 on 2025-08-01 20:46_

I guess another option here is

```rust
struct Position {
    row: usize,
    col: usize,
    cell: Option<usize>,
}
```

or even just adding the `Option<usize>` to the tuple we had before. That could deduplicate a little code in the `match` above.

---

_Comment by @github-actions[bot] on 2025-08-01 20:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-08-01 21:23_

---

_Review requested from @carljm by @ntBre on 2025-08-01 21:23_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-01 21:23_

---

_Review requested from @AlexWaygood by @ntBre on 2025-08-01 21:23_

---

_Review requested from @sharkdp by @ntBre on 2025-08-01 21:23_

---

_Review requested from @dcreager by @ntBre on 2025-08-01 21:23_

---

_Review requested from @BurntSushi by @ntBre on 2025-08-01 21:23_

---

_Review request for @dcreager removed by @ntBre on 2025-08-01 21:23_

---

_Review request for @carljm removed by @ntBre on 2025-08-01 21:23_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-01 21:23_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-08-01 21:23_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:899 on 2025-08-01 21:32_

I think I'd prefer using an `Option` with named fields. 

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-01 21:34_

Hmm, we might need to add this to the `Annotation` because a `Diagnostic` with multiple ranges may span multiple cells. Or is this already what snippet represents. A single code frame?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:642 on 2025-08-02 08:54_

Can you extend the documentation to account for notebooks?

---

_@MichaReiser reviewed on 2025-08-02 08:55_

---

_Comment by @MichaReiser on 2025-08-02 08:55_

Thanks for working on this. Adding this to the rendering will also improve ty's notebook support :)

---

_@ntBre reviewed on 2025-08-03 20:41_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-03 20:41_

I believe this is what a snippet represents, based on our `RenderableSnippet::new` docs:

https://github.com/astral-sh/ruff/blob/5f149fcc15b3b71d3bfc817734f60333a2b09cb1/crates/ruff_db/src/diagnostic/render.rs#L578-L579

and this usage in the same function:

https://github.com/astral-sh/ruff/blob/5f149fcc15b3b71d3bfc817734f60333a2b09cb1/crates/ruff_db/src/diagnostic/render.rs#L593-L594

Oh, I guess this is documented just above this too:

https://github.com/astral-sh/ruff/blob/5f149fcc15b3b71d3bfc817734f60333a2b09cb1/crates/ruff_annotate_snippets/src/snippet.rs#L67-L70

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-04 06:25_

```
/// One `Snippet` is meant to represent a single, continuous, 
/// slice of source code that you want to annotate. 
```

I'm not sure this is sufficient. You could have one snippet with multiple annotations but there's no guarantee that all annotations belong to the same cell and this would also be hard to guarantee in the linter (or formatter) because they run on the concatenated notebook source. 

That's why I think that each annotation might need to have its own cell in addition to the snippet itself.


---

_@MichaReiser reviewed on 2025-08-04 06:25_

---

_@ntBre reviewed on 2025-08-04 12:53_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-04 12:53_

> in addition to the snippet itself

Ah, okay I think I see what you mean. It turns out we have an existing mechanism that helps here. If the context windows for the annotations don't overlap, the secondary annotation gets a small sub-header, which already works for our notebooks:

```
error[unused-import]: `os` imported but unused
 --> notebook.ipynb:cell 1:2:8                
  |                                           
1 | # cell 1                                  
2 | import os                                 
  |        ^^                                 
  |                                           
 ::: notebook.ipynb:cell 3:4:5                
  |                                           
2 | def foo():                                
3 |     print()                               
4 |     x = 1                                 
  |     - second cell                         
  |                                           
help: Remove unused import: `os`              
```

The `- second cell` line is the secondary annotation with a very short underline.

This doesn't currently trigger if the context windows overlap:

```
error[unused-import]: `os` imported but unused
 --> notebook.ipynb:cell 1:2:8                
  |                                           
1 | # cell 1                                  
2 | import os                                 
  |        ^^                                 
3 | # cell 2                                  
4 | import math                               
  |        ---- second cell                   
5 |                                           
6 | print('hello world')                      
  |                                           
help: Remove unused import: `os`              
```

So there may be a very neat solution here without having to modify the `Snippet` or `Annotation` representation. I might just need better bookkeeping of the cell indices when computing context windows. We already truncate the first cell's context in the first example to fit within the cell, so I think this is all that's missing.

---

_@MichaReiser reviewed on 2025-08-04 13:46_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-04 13:46_

Oh I see. And we also never print the cell if they do overlap. So maybe that's fine as is? But it probably makes sense to add a test demonstrating the behavior at least

---

_@ntBre reviewed on 2025-08-04 13:49_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-04 13:49_

I think we should render something like this in the second case or the line numbers are a bit misleading:

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

That's the test I'm working on passing now.

---

_Review comment by @BurntSushi on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:899 on 2025-08-04 15:51_

Yeah I like the `Option` better here as well. Or the very least, enum variants with named fields. (i.e., Which `usize` is which?)

---

_@BurntSushi reviewed on 2025-08-04 15:52_

---

_@ntBre reviewed on 2025-08-04 15:59_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:899 on 2025-08-04 15:59_

That sounds good. I _think_ the old code had the names backwards even with only two fields in the tuple, so named fields will be helpful.

https://github.com/astral-sh/ruff/blob/af8587eabf83fa38fad153bfe25f714524542c07/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L252-L257

---

_Comment by @ntBre on 2025-08-04 16:36_

This should be ready for another look. We now always render additional annotations in different cells with their own sub-headings, even if the concatenated source lines are adjacent:

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

in this concatenated source:

```py
# cell 1            
import os           
# cell 2            
import math         
                    
print('hello world')
```

I double-checked that we reuse the same snippet if they _are_ in the same cell too:

```
error[test-diagnostic]: main diagnostic message
 --> notebook.ipynb:cell 2:2:8                 
  |                                            
1 | # cell 2                                   
2 | import math                                
  |        ^^^^ second cell                    
3 |                                            
4 | print('hello world')                       
  | ----- print statement                      
  |                                            
help: Remove `print` statement                 
```

I also updated the docs and `Position` representation, as suggested!

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/snippet.rs`:81 on 2025-08-04 16:53_

Oh, that makes sense!

---

_@MichaReiser reviewed on 2025-08-04 16:53_

---

_@BurntSushi approved on 2025-08-04 17:45_

I buy what you're selling!

---

_@MichaReiser approved on 2025-08-05 08:00_

Nice, this is great!

---

_Closed by @ntBre on 2025-08-05 16:27_

---

_Reopened by @ntBre on 2025-08-05 16:27_

---

_Merged by @ntBre on 2025-08-05 18:10_

---

_Closed by @ntBre on 2025-08-05 18:10_

---

_Branch deleted on 2025-08-05 18:10_

---

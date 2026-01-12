```yaml
number: 6140
title: Add support for Quarto notebooks 
type: issue
state: open
author: mjkanji
labels:
  - needs-decision
  - notebook
assignees: []
created_at: 2023-07-28T01:25:32Z
updated_at: 2025-10-25T13:05:54Z
url: https://github.com/astral-sh/ruff/issues/6140
synced_at: 2026-01-12T15:54:45Z
```

# Add support for Quarto notebooks 

---

_@mjkanji_

Copy-pasting my comment from #3792 below. Since Ruff already supports Jupyter notebooks, and Quarto is a plaintext equivalent of notebooks, it should hopefully be (relatively) easy to add support. The holy grail would be if the VS Code extension can highlight linting errors in the code blocks in an open `.qmd` file, but even just CLI support would be great to have as a start.

---

Chiming in here to ask if the development of this feature can also take Quarto (.qmd) into consideration. It's a great project aimed at providing a plaintext equivalent of Jupyter notebooks.

Hopefully, adding support shouldn't be too complicated because, like (vanilla) Markdown, .qmd files denote Python blocks explicitly, albeit with a slightly different syntax:

````md
# This is a Markdown heading

This is regular text.

```{python}
#| echo: false

# This is a Python comment
print("Hello, Ruff!")
```
````

The `#| echo: false` at the top is a Quarto-specific comment that controls its behaviour. See [here](https://quarto.org/docs/computations/execution-options.html#output-options). I don't imagine Ruff necessarily cares about specially formatted comments, though maybe it'll become relevant if you add formatting as a core functionality, a la #1904. 

Unlike Markdown, and like Jupyter notebooks, Quarto documents "carry the context throughout the file" -- so adding support should hopefully just be a matter of treating the code blocks as a single Python script, while ignoring any Markdown outside of those blocks. 

---

_Label `needs-decision` added by @charliermarsh on 2023-07-28 04:30_

---

_Comment by @wiraki on 2023-08-30 14:21_

Just wanted to add my +1 to this request :)

---

_Comment by @JanPalasek on 2023-12-13 07:46_

@mjkanji You cannot process the notebook code only by concatenating code cells and running the analysis on the whole code. This is due to jupyter specific features like cell-level magics. 

However, it is pretty much the same as processing Jupyter notebooks. It only requires a different parser for cells.

---

_Comment by @mjkanji on 2023-12-13 17:31_

> @mjkanji You cannot process the notebook code only by concatenating code cells and running the analysis on the whole code. This is due to jupyter specific features like cell-level magics. 
> 
> However, it is pretty much the same as processing Jupyter notebooks. It only requires a different parser for cells.

Awesome! Hopefully, this can be implemented soon!

---

_Comment by @tvatter on 2024-01-25 09:41_

+1 that would be really awesome, and probably something like that would also close #8800.

---

_Label `notebook` added by @dhruvmanila on 2024-02-12 06:15_

---

_Comment by @danieltomasz on 2024-06-18 18:32_

As this thread seems relevant does anyone know a way to disable rule for formatting  comments in Jupyter code cell withing `.ipynb` notebooks?(I am using notebooks via vscode extension), I can move it to new issue, but this one seems relevant (altough my request is different in scope)  

Quarto is not only a format of markdown based notebooks, but open source technical publishing  system.
With Quarto I can have Quarto `.ipynb` document/notebook  with python code cells annotated via "comment pipe" on top it. 
```
#| echo: false
import pandas as pd
import mne
```

Applying ruff formating is breaking it by adding space after `#`. How I can disable this behaviour ?
`# fmt: skip` like below
```
#| echo: false         # fmt: skip
```
doesn't seem to work.


More discussion is here  https://github.com/quarto-dev/quarto-cli/discussions/9137

---

_Comment by @chamaoskurumi on 2024-09-05 08:56_

> +1 that would be really awesome

I second that...

---

_Comment by @yeelauren on 2024-09-25 02:30_

My current workaround is this:

```
quarto convert your_doc.qmd 
jupyter nbconvert --to python your_doc.ipynb   
```

And you can run ruff on Jupyter or the python file directly but not ideal :melting_face: 

---

_Comment by @SaintRod on 2024-10-20 16:11_

+1 :)

---

_Comment by @so-rose on 2024-10-21 13:07_

Big +1 from here!

---

_Comment by @ntluong95 on 2024-12-12 21:51_

I strongly vote for this feature. As Quarto notebook is more and more common over Jupyter notebook, support for it is very much needed

---

_Comment by @danieltomasz on 2024-12-13 20:21_

So basically, here are mentioned 2 separate issues:
1) Add support for `.qmd` files
2) Disable formatting of the quarto pipe comments `#| settings: values`, which currently are formatted when someone is using `quarto` within Python `.ipynb` file 

---

_Comment by @aeturrell on 2024-12-16 15:25_

Massive +1 for this!

---

_Comment by @mohelm on 2025-03-04 15:47_

Hey is there an update on the support for `qmd` files?

---

_Comment by @malcolmbarrett on 2025-04-11 19:37_

I have experimented with a few approaches (including converting to Jupyter and back and the linked nbqa PR), and I think the tricks to make Ruff work with it leave too many artifacts in the Quarto document (particularly in the metadata, but other issues as well). I thought maybe these approaches might be sufficient, but I believe the best approach is going to be native Ruff support for formatting `.qmd` files.

---

_Comment by @gmermoud on 2025-04-18 13:43_

+1 that would be really useful!

---

_Comment by @myoung3 on 2025-10-25 13:05_

https://github.com/quarto-dev/quarto-markdown is an in-development parser for qmd files from the Quarto team that can parse into json. Once this is released it might be easy to use this to extract all the python code blocks from qmd files.

---

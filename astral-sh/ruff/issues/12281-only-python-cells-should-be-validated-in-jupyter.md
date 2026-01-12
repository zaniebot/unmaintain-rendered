```yaml
number: 12281
title: Only Python cells should be validated in Jupyter notebooks
type: issue
state: closed
author: stewartadam
labels:
  - notebook
assignees: []
created_at: 2024-07-10T19:07:57Z
updated_at: 2024-10-17T17:22:11Z
url: https://github.com/astral-sh/ruff/issues/12281
synced_at: 2026-01-12T15:54:51Z
```

# Only Python cells should be validated in Jupyter notebooks

---

_@stewartadam_

## Description
When using creating a multi-language notebook (e.g. using the [Polyglot Notebooks](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-interactive-vscode) extension), Ruff continues to validate all cells producing many linting errors on non-Python code.

## Context

In our case we were creating a C# notebook with .NET Interactive kernel and Ruff produced many linting errors. As a workaround, we have a top-level code cell with the contents `# ruff: noqa` to disable this, but it would be great if Ruff examined the cell language and only validated Python code.

#10504 mentions notebooks language is validated in the metadata, however Ruff is still linting the C# code even with this metadata present in the notebook:
```
"metadata": {
  "kernelspec": {
   "display_name": ".NET (C#)",
   "language": "C#",
   "name": ".net-csharp"
  },
  "polyglot_notebook": {
   "kernelInfo": {
    "defaultKernelName": "csharp",
    "items": [
     {
      "aliases": [],
      "name": "csharp"
     }
    ]
   }
  }
```

One thing to note with polyglot notebooks is each cell does have a language associated to it - it would be great if Ruff could parse this per-cell metadata and only validate Python code cells.

## Additional Details
ruff 0.4.10

configuration (pyproejct.toml):
```
[tool.ruff]
extend-include = ["*.ipynb"]
lint.extend-select = ["W"]
line-length = 140
```

Example output of linting errors produced on C# code:
```
$ pdm run ruff check
error: Failed to parse docs/getting-started-notebooks/api-example.ipynb:4:4:7: Simple statements must be separated by newlines or semicolons
docs/getting-started-notebooks/api-example.ipynb:cell 4:4:7: E999 SyntaxError: Simple statements must be separated by newlines or semicolons
docs/getting-started-notebooks/api-example.ipynb:cell 4:4:22: E703 [*] Statement ends with an unnecessary semicolon
docs/getting-started-notebooks/api-example.ipynb:cell 4:5:7: E999 SyntaxError: Simple statements must be separated by newlines or semicolons
docs/getting-started-notebooks/api-example.ipynb:cell 4:5:27: E703 [*] Statement ends with an unnecessary semicolon
docs/getting-started-notebooks/api-example.ipynb:cell 4:6:7: E999 SyntaxError: Simple statements must be separated by newlines or semicolons
docs/getting-started-notebooks/api-example.ipynb:cell 4:6:30: E703 [*] Statement ends with an unnecessary semicolon
docs/getting-started-notebooks/api-example.ipynb:cell 4:7:7: E999 SyntaxError: Simple statements must be separated by newlines or semicolons
docs/getting-started-notebooks/api-example.ipynb:cell 4:7:37: E703 [*] Statement ends with an unnecessary semicolon
docs/getting-started-notebooks/api-example.ipynb:cell 4:8:7: E999 SyntaxError: Simple statements must be separated by newline
...
```

---

_Comment by @charliermarsh on 2024-07-10 19:43_

I thought we filtered based on this already but can't find it. \cc @dhruvmanila 

---

_Comment by @MichaReiser on 2024-07-10 19:57_

I'm not sure if we filter based on the file's metadata. I thought we filter based on the cell metdata.

---

_Comment by @stewartadam on 2024-07-10 21:52_

If I'm understanding right it looks like it is validated here: https://github.com/astral-sh/ruff/blob/bbb9fe169207091e899dda15383f0ef16beb00b7/crates/ruff_linter/src/source_kind.rs#L50

However the metadata format I see in my polyglot or Python notebooks looks to be in a slightly different structure, so it's just returning the default `true`.

---

_Comment by @dhruvmanila on 2024-07-11 02:59_

@stewartadam Can you provide a full Notebook? I can look at where that metadata is coming from. Is it coming from the cell or from the Notebook itself? We verify that it's a Python notebook by looking at the notebook-level metadata which is what the function that you've linked does:

https://github.com/astral-sh/ruff/blob/880c31d1642a6b4d967a20f9affe9772a3722b85/crates/ruff_notebook/src/notebook.rs#L391-L398

---

_Label `notebook` added by @dhruvmanila on 2024-07-11 03:00_

---

_Comment by @stewartadam on 2024-07-11 18:21_

Here are two notebooks: https://gist.github.com/stewartadam/528689d9bb917715a4c16a6ff9282de3

It looks like the extensions have a habit of making a mess of the metadata though. Creating a new notebook defaults to Python and after switching it to .NET Interactive, it gets the .NET Interactive metadata layered on top of the original Python language metadata.

Similarly, saving a copy of the .NET notebook as python.ipynb and then changing the kernel+cells to Python left all the .NET metadata in place (look at the first revision in the Gist).

I suspect we'll want to introspect a mix of the notebook language, cell type, and kernel.

---

_Comment by @dhruvmanila on 2024-07-12 03:52_

This is confusing.

The `dotnet-polyglot.ipynb` has a cell with the following metadata which doesn't mention any language:
```json
   "metadata": {
    "vscode": {
     "languageId": "polyglot-notebook"
    }
   },
```

While, the `language_info` is still "python":
```json
  "language_info": {
   "name": "python"
  },
```

And, the `python.ipynb` notebook has a JavaScript cell with the following metadata:
```json
   "metadata": {
    "vscode": {
     "languageId": "javascript"
    }
   },
```

But, the `language_info` is still "python".

Which extensions are all involved here? Is it just https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.dotnet-interactive-vscode or are there any other extension? I'll probably need to look at the metadata schema these extensions have and encode it accordingly. I'm not sure how much effort it is to support this, it's not a priority right now with other important things going on.

---

_Comment by @MichaReiser on 2024-07-22 07:58_

Related, the open AI notebooks fail parsing because they contain "code" cells where only the `vscode.languageId` is set. I think we should start respecting `vscode.langaugeId` 

https://github.com/openai/openai-cookbook/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb?short_path=65a7845

---

_Comment by @dhruvmanila on 2024-07-22 08:57_

> Related, the open AI notebooks fail parsing because they contain "code" cells where only the `vscode.languageId` is set. I think we should start respecting `vscode.langaugeId`
> 
> [openai/openai-cookbook@`main`/examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb?short_path=65a7845](https://github.com/openai/openai-cookbook/blob/main/examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb?short_path=65a7845&rgh-link-date=2024-07-22T07%3A58%3A18Z)

Yeah, this seems reasonable. I'll want to check for any official documentation on this field and might require to look at the source code if it doesn't exists.

---

_Comment by @dhruvmanila on 2024-08-13 16:48_

@stewartadam So, #12864 should actually also fix the Polyglot notebooks case as well, at least it does for the linked notebooks:
```console
❯ ./target/debug/ruff check ~/Downloads/notebooks/dotnet-polyglot.ipynb --no-cache
All checks passed!

❯ ./target/debug/ruff check ~/Downloads/notebooks/python.ipynb --no-cache 
All checks passed!
```

> One thing to note with polyglot notebooks is each cell does have a language associated to it - it would be great if Ruff could parse this per-cell metadata and only validate Python code cells.

If this metadata is of the form `vscode.languageId`, then we'll start using it from next version.

Do you have an example Polyglot notebook which resembles the one in the PR description? I could test it out.

---

_Comment by @dhruvmanila on 2024-08-13 16:53_

Looking at the metadata you've provided, I think it might be useful to use the `kernelspec.language` as a fallback if there's no `language_info`. VS Code does the same: https://github.com/microsoft/vscode/blob/1c31e758985efe11bc0453a45ea0bb6887e670a4/extensions/ipynb/src/deserializers.ts#L20-L22

---

_Comment by @stewartadam on 2024-08-13 17:04_

This great to hear!

Agreed kernelspec.language as a fallback given the metadata captured from the notebooks generaetd with VSCode extension.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-08-13 17:38_

---

_Closed by @dhruvmanila on 2024-08-14 07:06_

---

_Closed by @dhruvmanila on 2024-08-14 07:06_

---

_Comment by @tigerhawkvok on 2024-10-16 18:37_

I'll chime in and say that magic commands for Databricks environments should have those cells disabled:

https://docs.databricks.com/en/notebooks/notebooks-code.html#mix-languages

eg, cells that start with the pattern `/^%[a-z]{2,}\s*$/` should be ignored (or, ignored unless it's `%python`)

---

_Comment by @dhruvmanila on 2024-10-17 05:12_

This seems like a special case for Databricks environment specifically, I'm not sure how to detect that. My main hesitation is that a single percent sign magic command like `%python` is a [line magic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#line-magics) which only considers the text on the same line where the magic command is present while a double percent sign magic command like `%%python` is a [cell magic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cell-magics) which considers all the content of the cell where the command is defined in. Ruff considers certain cells that contains cell magic commands because the following lines will have Python code:

https://github.com/astral-sh/ruff/blob/c6b311c54643c039d793fb836caf94d22f03cbb4/crates/ruff_notebook/src/cell.rs#L243-L254

Ruff will only ignore the line where a **line magic** is defined but will ignore the entire cell for **cell magics** except for the above cell magic commands.

---

_Comment by @tigerhawkvok on 2024-10-17 17:22_

That's fair enough. Databricks is an enormous platform used by a ton of big shops, so it seems like a decent carve-out, but it is nevertheless at least somewhat niche.

The language support is limited, so potentially a match _fail_ on `%sql`, `%scala`, `%sh`, `%fs`, `%md`, `%r` is easier to support, but I was hesitant to suggest an exclude list rather than an include list initially.

---

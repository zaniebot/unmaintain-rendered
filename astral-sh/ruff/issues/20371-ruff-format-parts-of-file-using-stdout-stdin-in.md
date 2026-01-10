```yaml
number: 20371
title: "Ruff format parts of file using stdout/stdin (in the style of vim's filter command)"
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-09-12T20:54:00Z
updated_at: 2025-09-30T07:15:07Z
url: https://github.com/astral-sh/ruff/issues/20371
synced_at: 2026-01-10T11:09:59Z
```

# Ruff format parts of file using stdout/stdin (in the style of vim's filter command)

---

_Issue opened by @kaddkaka on 2025-09-12 20:54_

### Question

Vim has a builtin command that let's you pipe any selected text through an external tool, for example

```vim
:%!sort -u      " use an external program to sort all lines
```

or select a few lines and do `:!tac` to reverse the line order.

This can be used to easily format code with ruff, example code:
```py
class HasAnA:                                                                                                                      
 def __init__(self, a):   
                                   self._a = a
```

select all the text above and `:!ruff format -` transforms it into:

```py
class HasAnA:                                                                                                                      
    def __init__(self, a):
        self._a = a
```

### The issue
If I want to format just a function, the first line of the function does not necessarily start at column 0. WHen selecting this part from the example above:
```                                                                                                       
 def __init__(self, a):   
                                   self._a = a
```

filtering with `ruff format -` just returns an error code 2 and this error:
```
error: Failed to parse at 1:1: Unexpected indentation 
```

## Suggestion / Wish
Ruff should just accept the indentation of the first line and treat the start column as column 0, when formatting from stdin.

Do you think this could be ok/doable?

## The awesomeness: Better vim integration

### Version
ruff 0.13.0

---

_Label `question` added by @kaddkaka on 2025-09-12 20:54_

---

_Comment by @MichaReiser on 2025-09-15 13:01_

> Ruff should just accept the indentation of the first line and treat the start column as column 0, when formatting from stdin.

It's a bit trickier than this because the intentation needs to be preserved when replacing the unformatted content with the formatted content. 

Have you looked at Ruff's range formatting functionality? I think it provides what you want: 

```
ruff format --range 2:10
```

where `2` is the start line and `10` the end.



---

_Comment by @kaddkaka on 2025-09-15 18:05_

> > Ruff should just accept the indentation of the first line and treat the start column as column 0, when formatting from stdin.
> 
> It's a bit trickier than this because the intentation needs to be preserved when replacing the unformatted content with the formatted content.
> 
> Have you looked at Ruff's range formatting functionality? I think it provides what you want:
> 
> ```
> ruff format --range 2:10
> ```
> 
> where `2` is the start line and `10` the end.

The problem with this is that it doesn't integrate with how vim's filter feature works:
1. Select some text
2. Only this selected text gets sent to an external tools stdin in
3. The output on stdout is used to replace the selected text.

Yes, retaining the indentation would probably be the most complicated part of this feature.

---

_Comment by @MichaReiser on 2025-09-16 06:53_

I see. I think your best approach here is to use Ruff's LSP instead of vim's filter feature. E.g. https://github.com/prabirshrestha/vim-lsp does support format range (which calls `ruff format --range` for the selected text range).

See https://docs.astral.sh/ruff/editors/setup/#vim for how to setup Ruff's LSP with vim

---

_Closed by @MichaReiser on 2025-09-30 07:15_

---

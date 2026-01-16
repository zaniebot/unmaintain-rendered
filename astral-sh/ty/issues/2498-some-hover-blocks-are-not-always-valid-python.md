```yaml
number: 2498
title: Some hover blocks are not always valid Python source
type: issue
state: open
author: rchl
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-14T19:27:10Z
updated_at: 2026-01-16T15:50:12Z
url: https://github.com/astral-sh/ty/issues/2498
synced_at: 2026-01-16T15:58:04Z
```

# Some hover blocks are not always valid Python source

---

_@rchl_

### Summary

Code snippet displayed in the hover popups is not always a valid python source code. Some editors have more lenient syntax highlighting that makes it not really an issue, other have more strict highlighting that will introduce red error highlighting to indicate that the code is not valid. For example in ST we get something like this in some cases:

<img width="698" height="215" alt="Image" src="https://github.com/user-attachments/assets/08f69ac5-f2da-4fe9-a3d5-ddcbb75070d3" />


Perhaps `ty` can tell whether the snippet is a valid code or not and based on that return either a python code block or plain text code block?

### Version

0.0.10

---

_Label `server` added by @AlexWaygood on 2026-01-14 20:45_

---

_Comment by @dhruvmanila on 2026-01-15 05:15_

I think we use the `xml` language tag for such code blocks, I see the following hover content in Neovim:

````
```xml
<module 'collections.abc'>
```
````

Is it that your editor's syntax highlighting doesn't support XML tags correctly? Can you try opening a XML file with the above content and see how does your editor highlight that?

---

_Comment by @MichaReiser on 2026-01-15 07:51_

The issue is that an attribute without a name isn't valid in XML. So the editor's highlighting is correct. It's also not valid in any other XML like dialect that I'm aware of (HTML, JSX). 

---

_Label `bug` added by @MichaReiser on 2026-01-15 07:51_

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-15 07:51_

---

_Comment by @rchl on 2026-01-15 07:57_

Right, I've assumed the part about using python code block. But yeah, still not a valid source for chosen code block language.

---

_Comment by @MichaReiser on 2026-01-15 08:00_

It's unfortunate that an LSP can't ship a custom syntax (at least to my knowledge)

---

_Comment by @carljm on 2026-01-15 16:37_

We can also change our display for these types. I don't think a lot of deep thought went into these displays, we just sort of copied a style the runtime repr sometimes uses. I'm not sure the extra punctuation is all that helpful.

---

_Comment by @AlexWaygood on 2026-01-15 16:44_

the issue is that `<module collections.abc>` would be valid XML but `<module 'collections.abc'>` is not? I very weakly prefer it with the quotes, but yeah, if that breaks syntax highlighting then we can definitely remove them.

---

_Comment by @MichaReiser on 2026-01-15 16:54_

`<module collections.abc>` is also not valid xml, the attribute msut be followed by an `=` and it also requires a closing tag.

---

_Comment by @MichaReiser on 2026-01-15 16:56_

I'd have to take a closer look at what pylance does but it seems to use a simple code block for modules

https://github.com/user-attachments/assets/9ee8eb51-9441-4b6a-bcae-c861fea816b5


---

_Comment by @MichaReiser on 2026-01-16 15:50_

@Gankra, you'll find this interesting.

---

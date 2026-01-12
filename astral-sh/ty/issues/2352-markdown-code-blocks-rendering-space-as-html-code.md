```yaml
number: 2352
title: Markdown code blocks rendering space as HTML code on Docstrings
type: issue
state: closed
author: gabriel-abn
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-05T18:13:03Z
updated_at: 2026-01-06T15:44:33Z
url: https://github.com/astral-sh/ty/issues/2352
synced_at: 2026-01-12T15:54:26Z
```

# Markdown code blocks rendering space as HTML code on Docstrings

---

_@gabriel-abn_

I'm using tabs on my code instead of spaces. Really don't know if this can be a cause.

Every Markdown tag I used works nice but code blocks are not rendering as I expected. They're rendering all spaces as HTML tags.

<img width="943" height="899" alt="Image" src="https://github.com/user-attachments/assets/720f8441-bd60-4066-a0fd-ad598c2a1dc5" />

**OBS**: Math blocks (with $$ --- $$) does not render as well. I see that is not a commonly used block but they're very nice to document some calculation and formulas.

---

_Comment by @Gankra on 2026-01-05 18:14_

Dang, guess I gotta fix this hack -- what IDE is this?

---

_Comment by @gabriel-abn on 2026-01-05 18:19_

VSCode.

<img width="517" height="262" alt="Image" src="https://github.com/user-attachments/assets/8d19d5e0-69ee-4505-bf2c-0b1baa4b2a96" />

---

_Label `bug` added by @MichaReiser on 2026-01-05 18:26_

---

_Label `server` added by @MichaReiser on 2026-01-05 18:26_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-05 18:26_

---

_Comment by @Gankra on 2026-01-05 18:52_

So I'll note that markdown isn't exactly happy about this input. Given the following:

------------

     Returns:
         Union...
         ```
         {
            id: ....
         }
         ```
     

-------------

We get this rendering:

---------

Returns:
     Union...
     ```
     {
        id: ....
     }
     ```

---------

I honestly don't even understand what on earth is happening in the markdown grammar here, but, suffice it to say it's Bad.

I can however munge the output to faithfully render it by eating the indent on the \`\`\` lines:

 Returns:
&nbsp;&nbsp;&nbsp;&nbsp;Union...
```
     {
        id: ....
     }
```

--------

Which looks even better in vscode

<img width="175" height="137" alt="Image" src="https://github.com/user-attachments/assets/9e0d62e9-a21b-43c1-9708-1c473d1cca7f" />

---

_Comment by @gabriel-abn on 2026-01-05 19:02_

Nice!! Just tested myself and this fixes for the highlight (even the html tags are highlighted lol).

This code block behavior is very similar to Obsidian. When adding a tab to a line it turns into a code like highlight.

<img width="698" height="406" alt="Image" src="https://github.com/user-attachments/assets/118e8fab-0fca-4832-863e-d2efc474c417" />

---

_Closed by @Gankra on 2026-01-06 15:44_

---

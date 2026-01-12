```yaml
number: 825
title: (🎁) use markup for diagnostics in lsp to codify backtics
type: issue
state: open
author: KotlinIsland
labels:
  - server
assignees: []
created_at: 2025-07-15T09:24:58Z
updated_at: 2025-09-22T08:14:42Z
url: https://github.com/astral-sh/ty/issues/825
synced_at: 2026-01-12T15:54:24Z
```

# (🎁) use markup for diagnostics in lsp to codify backtics

---

_@KotlinIsland_

### Summary

i think it would be cool to render backtics from messages as code blocks inside the ide, but i can't do it due to the dynamic nature of messages:
```py
reveal_type("`")  # Revealed type: `Literal["`"]`
```

lsp 3.18 supports markup in diagnostics: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.18/specification/#diagnostic

---

_Comment by @MichaReiser on 2025-07-15 09:28_

What API are we speaking about here? 

---

_Comment by @KotlinIsland on 2025-07-15 10:16_

lsp API, can't imagine there would be a need for the CLI API for use with ide integration 

---

_Comment by @MichaReiser on 2025-07-15 11:08_

We currently use the same interface for the LSP and CLI and I was asking because we also need to think about if we want this behavior in a JSON output format or not. 

this issue is also not specific to reveal type, it exists everywhere where we render a type in a diagnostic. 

Escaping the backtick doesn't sound unreasonable but it might be confusing in the CLI or other output formats. That's why it spend some time investing what other tools to in these situations.

---

_Renamed from "(🎁) provide an option for escaped backtics in messages" to "(🎁) provide an option for escaped backticks in messages" by @AlexWaygood on 2025-07-15 13:38_

---

_Comment by @InSyncWithFoo on 2025-07-16 01:52_

I don't think escaping backticks is the way to go here, since it may just not work. For example:

```markdown
`Literal["\`"]`
```

> `Literal["\`"]`

That is, <code>&lt;code>Literal[&amp;quot;\\&lt;/code>&amp;quot;]`</code> in HTML ([same for CommonMark](https://spec.commonmark.org/dingus/?text=%60Literal%5B%22%5C%60%22%5D%60)).

----

A better approach is to find the shortest sequence of backticks that the message itself does not contain:

```markdown
``Literal["`"]``
```Literal["``"]```
```

> ``Literal["`"]`` \
> ```Literal["``"]```


---

_Comment by @KotlinIsland on 2025-07-16 02:00_

> We currently use the same interface for the LSP and CLI 

i suppose it wouldn't affect anything if the effect was globally applied, as i would imagine this would be an argument sent when initializing the server

> this issue is also not specific to reveal type, it exists everywhere where we render a type in a diagnostic.

i understand, this would be desired behaviour. rendering all messages with code blocks would be a good feature

> Escaping the backtick doesn't sound unreasonable but it might be confusing in the CLI or other output formats. That's why it spend some time investing what other tools to in these situations.

maybe i didn't explain properly sorry. i was imagining that this feature would be off by default, only being enabled when a client (PyCharm or w/e) opts into the behavior

> Escaping the backtick doesn't sound unreasonable but it might be confusing in the CLI or other output formats. That's why it spend some time investing what other tools to in these situations.

if we wanted to make it even easier for the client, then the message could forgoe backtics entirely and use html escaping and `<code>` tags instead, then the parsing of the message would be simpler for the client

i.e.

```py
reveal_type("<code>")
```
would report `Revealed type is <code>Literal[&quot;&lt;code&gt;&quot;]</code>`

---

_Comment by @KotlinIsland on 2025-07-16 02:04_

> since it may just not work

there would need to be custom parsing involved for escaped backtics to work

---

_Comment by @KotlinIsland on 2025-09-21 23:50_

actaully, lsp 3.18 supports markup in diagnostics: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.18/specification/#diagnostic

so this issue should be changed to use markup for diagnostic messages in lsp

---

_Renamed from "(🎁) provide an option for escaped backticks in messages" to "(🎁) use markup for diagnostics in lsp to codify backtics" by @KotlinIsland on 2025-09-21 23:50_

---

_Label `server` added by @dhruvmanila on 2025-09-22 04:55_

---

_Comment by @MichaReiser on 2025-09-22 08:14_

3.17 is the most recent lsp specification and whether we can use markup also depends on how many editors support it. We may need to implement a fallback for editors that don't support it yet.

---

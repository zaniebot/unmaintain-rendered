```yaml
number: 299
title: Option flag to automatically escape regex
type: issue
state: closed
author: AnthIste
labels: []
assignees: []
created_at: 2017-01-03T08:57:35Z
updated_at: 2017-08-11T21:41:28Z
url: https://github.com/BurntSushi/ripgrep/issues/299
synced_at: 2026-01-12T16:13:21Z
```

# Option flag to automatically escape regex

---

_@AnthIste_

I mainly use ripgrep to search for stuff in a C# project. By default the search string is a regex which means that namespace separators, method calls and strings must all be escaped with `\`. These are my primary searches and all of them need copious backslashes:

- `Project.Namespace.Whatever` -> `rg "Project\.Namespace\.Whatever"`
- `object.Method()` -> `rg "object\.Method\(\)"`
- `"string"` -> `rg "\"string\""`

Would it make sense to add an argument that treats the search query as a "raw" string and automatically convert it to a "dumb" escaped regex?

---

_Comment by @BurntSushi on 2017-01-03 10:39_

It already exists. It is the -F flag, which is identical to the same flag
in grep.

How could the documentation be improved such that this would have been
clearer to you?

On Jan 3, 2017 3:57 AM, "AnthIste" <notifications@github.com> wrote:

> I mainly use ripgrep to search for stuff in a C# project. By default the
> search string is a regex which means that namespace separators, method
> calls and strings must all be escaped with \. These are my primary
> searches and all of them need copious backslashes:
>
>    - Project.Namespace.Whatever -> rg "Project\.Namespace\.Whatever"
>    - object.Method() -> rg "object\.Method\(\)"
>    - "string" -> rg "\"string\""
>
> Would it make sense to add an argument that treats the search query as a
> "raw" string and automatically convert it to a "dumb" escaped regex?
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/299>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34ssllTsnW0TCi7o-V0V7JQR0u_cKks5rOg1_gaJpZM4LZbr0>
> .
>


---

_Comment by @AnthIste on 2017-01-03 12:19_

> How could the documentation be improved such that this would have been
clearer to you?

No really, it can't be more obvious. I just looked through `ripgrep --help` again and didn't see it. When I copy-pasted the output it jumped out on me.

```
    -F, --fixed-strings        Treat the pattern as a literal string instead of
                               a regular expression.
```

Yup, that looks like what I need. I honestly have no idea how I missed that - I even started scratching through the source code before opening an issue.

Thanks for the help!

---

_Comment by @BurntSushi on 2017-01-03 12:22_

No problem!

---

_Closed by @BurntSushi on 2017-01-03 12:22_

---

_Comment by @AnthIste on 2017-01-03 12:23_

Just another question not directly related to this issue:

Is there an option to specify file extensions to search without using the `--type-add` system? I find myself wanting to do ad-hoc searches on specific combinations of filetypes without necessarily designating a category for them.

---

_Comment by @BurntSushi on 2017-01-03 12:23_

@AnthIste Why not just use the `-g/--glob` flag? e.g., `rg -g '*.{foo,bar,baz}' pattern` will only search files with an extension `foo`, `bar` or `baz`.

---

_Comment by @AnthIste on 2017-01-03 12:25_

That makes sense :+1: I'll try that in future

---

_Comment by @partounian on 2017-08-11 21:04_

I tried to use `-F` but it didn't work when searched for `rg '->price'`

---

_Comment by @okdana on 2017-08-11 21:41_

You need to use `--` to tell `rg` that a pattern beginning with a hyphen isn't supposed to be a command-line option:

```
rg -- '->price'
```

---

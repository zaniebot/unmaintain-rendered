```yaml
number: 1101
title: typos in man page for --multiline.
type: issue
state: closed
author: anntzer
labels:
  - bug
assignees: []
created_at: 2018-11-04T11:20:51Z
updated_at: 2018-11-06T12:00:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1101
synced_at: 2026-01-12T16:13:22Z
```

# typos in man page for --multiline.

---

_@anntzer_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Arch Linux distro package.

#### What operating system are you using ripgrep on?

Arch Linux (rolling).

#### Describe your question, feature request, or bug.

The following section of the man page (`man rg`) contains... incomplete(?) sentences (emphasis mine):

> -U, --multiline
Enable matching across multiple lines.

> When multiline mode is enabled, ripgrep will lift the restriction that a match
cannot include a line terminator. For example, **when multiline mode is not codepoint
other than \n**. Similarly, the regex \n is explicitly forbidden, and if you try to
use it, ripgrep will return an error. However, **when multiline and regexes like \n**
are permitted.

---

_Comment by @BurntSushi on 2018-11-05 12:14_

This is a strange one... The docs are fine in `src/app.rs` (the ultimate source of truth), and they even appear to be fine in the auto-generated ASCIIDoc format:

```
When multiline mode is enabled, ripgrep will lift the restriction that a match
cannot include a line terminator. For example, when multiline mode is not
enabled (the default), then the regex '\p{any}' will match any Unicode
codepoint other than '\n'. Similarly, the regex '\n' is explicitly forbidden,
and if you try to use it, ripgrep will return an error. However, when multiline
mode is enabled, '\p{any}' will match any Unicode codepoint, including '\n',
and regexes like '\n' are permitted.
```

But when it gets translated to the man page format, some text gets clipped:

```
When multiline mode is enabled, ripgrep will lift the restriction that a match cannot include a line terminator\&. For example, when multiline mode is not codepoint other than
\fI\en\fR\&. Similarly, the regex
\fI\en\fR
is explicitly forbidden, and if you try to use it, ripgrep will return an error\&. However, when multiline and regexes like
```

From my perspective, this is a bug in the asciidoc converter, unless we are unknowingly using some asciidoc syntax incorrectly. For example, it looks like every line that contains `'\\p{any}'` gets cut out. So that's likely the issue.

---

_Label `bug` added by @BurntSushi on 2018-11-05 12:14_

---

_Comment by @okdana on 2018-11-06 00:56_

I'm pretty sure it's because `{any}` is interpreted as an attribute reference. Some old documentation i found says:

>An attribute reference is an attribute name (possibly followed by an additional parameters) enclosed in curly braces. When an attribute reference is encountered it is evaluated and replaced by its corresponding text value. If the attribute is undefined the line containing the attribute is dropped.

Aside from manually escaping those (which would affect the `--help` output), there's supposed to be a way you can tell the converter to leave them alone by wrapping content in a block and removing `attributes` from its `subs` list, so the template would be something like this:

```
[subs="specialcharacters,quotes,specialwords,replacements,macros,replacements2"]
--
{OPTIONS}
--
```

But i've been trying to get that to work for a while and it just doesn't, i don't know why. The documentation for AsciiDoc is extremely unhelpful

One thing that *does* seem to work is to pull out all of the `\{(\w+)\}` from the formatted options and then call `a2x` with this for each one: `-a$1=&#123;$1}`. That way, if it tries to substitute anything, it just gets replaced by the literal version of itself. Feels a bit silly though

*Asciidoctor* seems to pass it through without any additional changes (even though the documentation says it's 'considered an error'), but it requires Ruby

---

_Comment by @BurntSushi on 2018-11-06 11:57_

> If the attribute is undefined the line containing the attribute is dropped.

Oof. What a weird failure mode. I'm just going to replace `{` and `}` with their escaped forms in the build script. We're already doing a few touchups to the plain text to "convert" it to asciidoc. That seems to fix things. Thanks for the research @okdana! :)

---

_Closed by @BurntSushi on 2018-11-06 12:00_

---

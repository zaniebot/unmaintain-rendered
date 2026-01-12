```yaml
number: 2911
title: ripgrep matching on and printing part of the URL in osc8 hyperlinked text
type: issue
state: closed
author: gdrapala
labels:
  - question
assignees: []
created_at: 2024-10-15T14:41:38Z
updated_at: 2024-10-15T23:01:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2911
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep matching on and printing part of the URL in osc8 hyperlinked text

---

_@gdrapala_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

; rg --version
ripgrep 14.1.1

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.43 is available (JIT is available)


### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macos

### Describe your bug.

When using ripgrep on text with osc8-style hyperlinks, rg seems to match on the URL itself and include the matched part of the URL in the outputted text.

Since OSC8 URLs are not printed in a terminal, I expected rg to neither match on nor print the URL contents.

Let me know if there's a different hyperlink-format mode to enable this functionality.

### What are the steps to reproduce the behavior?

```
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n'
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg --hyperlink-format=default _a_link
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg --hyperlink-format=default _a_
```

### What is the actual behavior?

Here's the above commands with the actual output. The last command produces an unexpected output:
```
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n'
This_is_a_link
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg --hyperlink-format=default _a_link
This_is_a_link
; printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg --hyperlink-format=default _a_
_a_pageThis_is_a_link
```

### What is the expected behavior?

The expected output for the last command is "This_is_a_link", same as the previous two commands.

---

_Comment by @BurntSushi on 2024-10-15 16:29_

So firstly, I'm getting different output in my terminal for the initial basic command:

```
$ printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n'
This_is_a_link
'This_is_a_link
'This_is_a_link
```

I don't know why it's different from yours. I don't understand why `This_is_a_link` is repeated three times, with the second two times starting with a `'`.

Otherwise, I don't seem to be able to reproduce your problem:

```
$ printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg --hyperlink-format=default _a_
This_is_a_link
' | rg --hyperlink-format=default _a_This_is_a_link
' | rg --hyperlink-format=default _a__a_pageThis_is_a_link
$ printf '\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n' | rg _a_
This_is_a_link
' | rg _a_This_is_a_link
' | rg _a__a_pageThis_is_a_link
```

In general, mixing ANSI escape sequences from an input source with ANSI escape sequences with the output from another tool (like what ripgrep does) usually doesn't lead to good results. And there really isn't anything to be done about it. We aren't dealing with semantic markup here.

---

_Comment by @gdrapala on 2024-10-15 17:28_

Hmm, I'm also not sure why printf is behaving differently on your system.  You might try a different shell (e.g., tcsh) or  python as shown below.
```
; python -c "print('\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n', end='')"
This_is_a_link
```

But I see your point about terminal text not being a full-fledged markup format.

One minor point is that grep seems to behave as desired on this example (i.e., grep on the visible text, preserve the URL, not insert any of the URL in the output):
```
; python -c "print('\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n', end='')" | /opt/homebrew/bin/rg --hyperlink-format=default _a_
0m_a_pageThis_is_a_link

; python -c "print('\033]8;;http://example.com/this_is_a_page\033\\This_is_a_link\033]8;;\033\\\n', end='')" | /usr/bin/grep _a_
This_is_a_link

; /usr/bin/grep --version
grep (BSD grep, GNU compatible) 2.6.0-FreeBSD
```

To compare with ANSI colors though, some weirdness is expected. For example:
```
; python -c 'import click; print(click.style("abc", fg="red") + click.style("def", fg="blue"))'
abcdef
; python -c 'import click; print(click.style("abc", fg="red") + click.style("def", fg="blue"))' | rg cd
<prints nothing>
```
Even though "cd" are printed back-to-back in the terminal, ripgrep for "cd" doesn't find any matching lines because of the non-printed ANSI color sequences involved.  With ANSI color, upstream tools are expected to either detect if stdout is not tty and disable ANSI or give users the ability to forcibly disable ANSI color output.

I suppose the same is expected of OSC8 ANSI sequences generated by upstream tools.

But, in practice, grep/ripgrep happen to work well on ANSI colorized text when the search pattern is fully enclosed in a single ANSI color block.

Anyhoo, that's a long winded way to say that a ripgrep update to ignore the non-visible URL parts is a potential enhancement.  No idea if it's generally feasible, but if it is we would be able to leave osc8 ON most of the time when piping output through rg and would only have to turn osc8 OFF when rg'ing for text that spans a URL boundary (which is similar to ansi color today).

Thanks!

---

_Comment by @ltrzesniewski on 2024-10-15 18:45_

I think there's something unclear here:

> When using ripgrep on text with osc8-style hyperlinks, rg seems to match on the URL itself and include the matched part of the URL in the outputted text.

Yes, that's because ripgrep doesn't filter its input text. Generally, you should provide input text without any ANSI/OSC8 codes, as ripgrep will output any matching sequence, and insert its own codes over the matching input. If anything from an escape sequence matches, the result can be garbled text.

> Let me know if there's a different hyperlink-format mode to enable this functionality.

That's where your confusion comes from I suppose. The `--hyperlink-format` parameter controls ripgrep's *output*, it doesn't filter its input. It sets the URL format you want ripgrep to use to display the matches it found.

For example, when using `--hyperlink-format=vscode`, ripgrep will wrap the line numbers in its output in hyperlinks which will open the matching file in VS Code and place the cursor at the matching location.

> With ANSI color, upstream tools are expected to either detect if stdout is not tty and disable ANSI or give users the ability to forcibly disable ANSI color output.
>
> I suppose the same is expected of OSC8 ANSI sequences generated by upstream tools.

Yes, exactly.

> Anyhoo, that's a long winded way to say that a ripgrep update to ignore the non-visible URL parts is a potential enhancement.

I think that would be better done by an external filter. I found `ansi2txt` for instance (but haven't tested it, there should be other ones if necessary):

```
your-upstream-tool | ansi2txt | rg your-pattern
```

---

_Comment by @gdrapala on 2024-10-15 22:50_

> That's where your confusion comes from I suppose. The --hyperlink-format parameter controls ripgrep's output, it doesn't filter its input. It sets the URL format you want ripgrep to use to display the matches it found.

Thank you for clearing up my confusion. I was not clear on the exact details of that parameter.

> Generally, you should provide input text without any ANSI/OSC8 codes, as ripgrep will output any matching sequence, and insert its own codes over the matching input. If anything from an escape sequence matches, the result can be garbled text.

I better understand that ripgrep is capable of producing OSC8, which is awesome. Based on my experience with other programs before and after OSC8 support (mostly "less" pre and post v566), I was expecting OSC8 compatibility to mean at both input and output, without having to externally strip the OSC8 tags, but looking for feedback on that expectation.

---

_Comment by @BurntSushi on 2024-10-15 23:00_

ripgrep definitely does not do any ANSI input sanitization and never will. It would tank performance. ripgrep searches what you give it. That is its purpose. It doesn't understand the semantic structure of documents other than newlines.

---

_Closed by @BurntSushi on 2024-10-15 23:00_

---

_Label `question` added by @BurntSushi on 2024-10-15 23:01_

---

```yaml
number: 2471
title: Add a flag to show patterns alongside matches
type: issue
state: closed
author: gofri
labels: []
assignees: []
created_at: 2023-03-22T15:10:30Z
updated_at: 2023-03-29T12:54:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2471
synced_at: 2026-01-12T16:13:24Z
```

# Add a flag to show patterns alongside matches

---

_@gofri_

Hey, thanks for that fantastic tool!

My use case is the following:
Running:
```rg -f PATTERNS_FILE directory```
Where PATTERNS_FILE has many patterns, the directory has many subdirectories and files.

Currently, I can extract from the output:
list of matches where each match is {file, line, text}.

My goal is to map the patterns that led to this match.
To do that, the output should include patterns that match each result.
For backward compatibility, it makes sense only to add this output if a new flag is added.

I'm not familiar enough with the internals of ripgrep to tell if it's possible technically, i.e., in case it doesn't play well with the concurrency model.
I assume that the first match is written to the stream, and later matches are filtered out, so there's no way to "list" the patterns without breaking the streaming.

So, as a second-best solution, it'd be helpful to output only the "first" pattern that matches, where the order is determined by the order of the patterns in the inputs (in the -f case, the first line that matches).
I guess this would be problematic for the same reasons (the concurrency model is unaware of any such order).
So as an alternative, I think it'd be helpful to have the same match shown multiple times in the output, each time with the relevant pattern. That'll keep the streaming going, but allow me to parse the output and merge the list myself.

p.s.
Obviously, I can run rg repeatedly, providing only a single pattern at a time, but the time difference is huge (dealing with ~100 patterns and lots of files)


---

_Comment by @BurntSushi on 2023-03-22 15:54_

I think the fundamental problem with your suggestion is that you've assumed that ripgrep even knows which pattern matched. It doesn't. If you pass a file like

```
foo
bar
quux
```

to ripgrep's `-f/--file` flag, then that's just equivalent to `rg -e foo -e bar -e quux`. And _that_ is just equivalent to `rg 'foo|bar|quux'`. The regex engine doesn't report which branch matches. No regex engine does.

In the future, with some ongoing improvements to ripgrep's underlying regex engine, it will be possible to construct a multi-pattern regex that _is_ aware of which pattern matches. But whether and how that information is exposed through ripgrep is not clear to me. And as you've demonstrated, it is a feature that begets more features.

> So, as a second-best solution, it'd be helpful to output only the "first" pattern that matches, where the order is determined by the order of the patterns in the inputs (in the -f case, the first line that matches).

That's what happens today. It just doesn't tell you _which_ pattern it is.

---

_Comment by @gofri on 2023-03-22 16:04_

Ahh, I see. Makes sense.

For now I think I'll revert to:
1. run ripgrep as mentioned 
2. map the matches (file, line, match)
3. for each match, run each pattern individually to create the mapping

Assuming the result set is much smaller than the original text that we search,
it should be much faster than any alternative I can think of anyway.
I'd be glad to hear any better ideas to solve it (given ripgrep's current status).

Thank you for your response! you can close the issue afaic

---

_Comment by @BurntSushi on 2023-03-22 16:33_

That makes sense. When the regex engine changes land, you'll be able to write a program that does it in one pass. You'll want to watch https://github.com/rust-lang/regex/issues/656 for status updates on those changes. They should be landing in the next few months.

Now, you'd have to write a Rust program in that case. I don't know when, if ever, that sort of "which pattern matched" functionality will be exposed in ripgrep.

---

_Closed by @BurntSushi on 2023-03-22 16:33_

---

_Comment by @gofri on 2023-03-29 12:42_

@BurntSushi - following directly from this issue - 
> to ripgrep's -f/--file flag, then that's just equivalent to rg -e foo -e bar -e quux. And that is just equivalent to rg 'foo|bar|quux'.

I just found out an unexpected behavior for me:
Indeed, using multiple patterns e.g.:
```
-e foo -e bar -e quux
```
translates directly to
```
foo|bar|quux
```
rather than
```
(?:foo)|(?:bar)|(?:quux)
```

as a result, the following outputs a match:
```
echo 'MyText' | rg -e '(?i)notintext' -e 'text'
```
where each of the following isn't:
```
echo 'MyText' | rg -e '(?i)notintext'
echo 'MyText' | rg -e 'text'
```
The following would work as expected:
```
echo 'MyText' | rg -e '(?i:notintext)' -e 'text'
```

and the following too:
```
echo 'MyText' | rg -e '(?:(?i:notintext))' -e '(?:text)'
```


but I wonder if that's intentional behavior.
otherwise, I'd open a bug for it

---

_Comment by @BurntSushi on 2023-03-29 12:54_

Yeah open a bug please. I'll comment more about it and why it's tricky to fix in all cases there.

---

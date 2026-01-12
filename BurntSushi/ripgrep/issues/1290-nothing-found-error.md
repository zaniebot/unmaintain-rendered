```yaml
number: 1290
title: "Nothing found = error?!"
type: issue
state: closed
author: fbruetting
labels: []
assignees: []
created_at: 2019-05-31T00:07:30Z
updated_at: 2021-01-15T21:32:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1290
synced_at: 2026-01-12T16:13:23Z
```

# Nothing found = error?!

---

_@fbruetting_

Why does `rg` acutally return an exit status of 1 when nothing was found? Exit status 1 means “error”, but when `rg` just found nothing, this is no error at all! That also makes it hard to script… -.-

When nothing is found, `rg` should therefore return an exit status of 0, which is WAY more correct and logical.

---

_Comment by @BurntSushi on 2019-05-31 00:22_

This is standard grep behavior, and makes scripting easier. For example, you can write things like this:

```
if rg -q foo file; then
  echo matched
fi
```

---

_Closed by @BurntSushi on 2019-05-31 00:22_

---

_Comment by @fbruetting on 2019-05-31 00:25_

So there is no distinction made between “error” and “nothing found”?

---

_Comment by @BurntSushi on 2019-05-31 02:28_

There is. Read the man page. The exit status is documented.

---

_Comment by @mohkale on 2021-01-15 19:56_

@BurntSushi 

Perhaps this should be in a new issue, but there doesn't appear to be a distinction between "error" and "nothing found".  There's `--quiet` but that disables showing any search results altogether. For the situations in which we're embedding ripgrep in external tools (eg. emacs to grep through a project) the lack of any results shouldn't be treated as an error and results should still be shown. This is because we're incrementally building up a pattern to search and expect for it to not match anything until we're finished. Labelling it as an error implies there's a fault in the syntax or command line arguments, rather than just a general lack of matches. I'd appreciate at least a flag to disable this behaviour.

---

_Comment by @BurntSushi on 2021-01-15 20:04_

> but there doesn't appear to be a distinction between "error" and "nothing found"

There is. And as I mentioned above, it's documented in the man page. Here's a simple example demonstrating it:

```
$ pwd
/home/andrew/rust/ripgrep
$ rg NOTFOUND
$ echo $?
1
$ rg 'badregex{'
regex parse error:
    badregex{
            ^
error: unclosed counted repetition
$ echo $?
2
```

> the lack of any results shouldn't be treated as an error and results should still be shown. This is because we're incrementally building up a pattern to search and expect for it to not match anything until we're finished. Labelling it as an error implies there's a fault in the syntax or command line arguments, rather than just a general lack of matches. I'd appreciate at least a flag to disable this behaviour.

Whatever you're using to spawn a ripgrep process should easily enable you to implement this functionality by inspecting the exit status. I see no reason for a flag in ripgrep for this.

---

_Comment by @mohkale on 2021-01-15 20:10_

> There is. And as I mentioned above, it's documented in the man page. Here's a simple example demonstrating it

Oh sorry, I meant there doesn't seem to be a flag that differentiates `errors` from `nothing found`, or to put it another way disregards "nothing found" while keeping the current error behaviour.

> Whatever you're using to spawn a ripgrep process should easily enable you to implement this functionality by inspecting the exit status. I see no reason for a flag in ripgrep for this.

Yes, but in my use-case it requires specific workarounds for this. I'm using an external package which groups together a bunch of commands including ripgrep, git-grep, grep, etc. Adding specific logic to say don't consider 1 an error, but treat every other none-zero exit code as one feels out of scope there. I'll try to just use a subshell and check the exit code doesn't equal 1 as you suggest. I didn't initially want to cause it'd require an extra process call and escaping for something that could just as well take a single flag, but better than nothing I suppose :smile:.

Thanks for your help.

EDIT:

I'll also have to add platform specific logic for `cmd` on windows compared to `sh` everywhere else :cry:.

---

_Comment by @BurntSushi on 2021-01-15 20:28_

This is standard behavior for grep tools. GNU grep should have the same behavior. And it looks like `git grep` does as well. ripgrep's behavior was literally modelled after how GNU grep behaves.

I don't understand why an additional process call is required here. But it is not inherently required. Anything that spawns a process should give you some kind of API to inspect the exit status. If you're using some package that wraps these tools and that package doesn't understand how these tools use the exit status, then that's probably missing functionality in that external package. (Or perhaps even a bug.)

---

_Comment by @mohkale on 2021-01-15 21:23_

@BurntSushi 

> This is standard behavior for grep tools. GNU grep should have the same behavior. And it looks like git grep does as well. ripgrep's behavior was literally modelled after how GNU grep behaves.

Yes, I'm aware, thanks for pointing that out. From what I understand that behaviour makes scripting easier which is why it's like that. The justification for an option like originally asked for in this issue to disable this is for when you're plugging ripgrep, grep, git-grep in external tools. In these situations a lack of any output is equivalent to having no matches so a dedicated exit code felels redundant. Although I totally understand why it behaves like this and your reluctance to add an option to disable this behaviour.

For example detecting no matches would get really ugly without a failed exit code:

```sh
out=$(grep -q foo file)
if [ "$?" -ne 0 ] && [ -z "$out" ]; then
  echo "No matches friend"
elif [ "$?" -ne 0 ]; then
  echo "Error friend"
else
  echo "Found some results"
fi
```

S.N. I didn't notice git-grep was also like this, but truth be told I rarely use either grep or git-grep when ripgrep is an option just cause I find ripgrep to be the best :smile:.

> I don't understand why an additional process call is required here. 

It's something to the affect of multiple layers of abstraction wrapping around a common interface. The interface prompts users for an input and it populates a command template (eg. `"ripgrep -e %s"`) which it then runs and presents the output for selection from the user.

This is a gross oversimplification but hopefully you get the high level idea. In this case the procedure that runs the command isn't the same as the one that provides it (if that makes any sense). And so we could probably add a hook argument to the interface function which the ripgrep and other grep variants can fail gracefully on a 1 exit code. But rather than adding an API changing parameter, I feel a subshell is a better option.

Of course all of this is completely irrelevent to ripgrep, just thought I'd explain myself fully :smile:.

Incidentally I've changed the command for ripgrep from

```lisp
 '("rg" "--null" "--line-buffered" "--color=always" "--max-columns=500" "--no-heading" "--line-number" "." "-e")
```

to

```lisp
'("sh" "-c"
  "rg --null --line-buffered --color=always --max-columns=500 --no-heading --line-number . -e \"$@\"
               if [ $? -eq 0 ] || [ $? -eq 1 ]; then
                 exit 0
               else
                 exit $?
               fi" "rg")
```

so you can probably guess why I'd have preferred an option :rofl:.

---

_Comment by @BurntSushi on 2021-01-15 21:32_

Thanks for explaining. I understand now. Yes, some kind of sub-shell is probably the best option.

---

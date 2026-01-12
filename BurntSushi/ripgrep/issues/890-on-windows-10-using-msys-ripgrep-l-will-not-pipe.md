```yaml
number: 890
title: On Windows 10 using MSYS ripgrep -l will not pipe files properly
type: issue
state: closed
author: Zyst
labels:
  - question
assignees: []
created_at: 2018-04-20T16:10:00Z
updated_at: 2018-07-22T16:19:07Z
url: https://github.com/BurntSushi/ripgrep/issues/890
synced_at: 2026-01-12T16:13:22Z
```

# On Windows 10 using MSYS ripgrep -l will not pipe files properly

---

_@Zyst_

#### What version of ripgrep are you using?

```bash
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

On Windows 10, using MSYS ([Git bash for Windows](https://git-scm.com/downloads), in my case) `ripgrep -l` will not pipe the files in a way that actually allows them to be used as an argument for another item.

#### If this is a bug, what are the steps to reproduce the behavior?

Reproduction:

```bash
~/dev/ripgrep$ rg fast -l

CHANGELOG.md
appveyor.yml
FAQ.md
GUIDE.md
README.md
...
```

#### If this is a bug, what is the actual behavior?

`ripgrep -l` output cannot be piped to stuff like sed.

```bash
~/dev/ripgrep$ rg fast -l | xargs sed s/fast/FAST/g -i

sed: can't read appveyor.yml: No such file or directory
sed: can't read CHANGELOG.md: No such file or directory
sed: can't read FAQ.md: No such file or directory
sed: can't read GUIDE.md: No such file or directory
sed: can't read README.md: No such file or directory
...
```

#### If this is a bug, what is the expected behavior?

I have confirmed this is an MSYS specific issue, doing the same thing in WSL will work as expected:

![image](https://user-images.githubusercontent.com/3832380/39061610-828a3550-448a-11e8-9627-6635daf26c2f.png)

This is not a huge problem for me, but it'd still be nice to have this work.

Sorry if I am stupidly missing something obvious. And also sorry if this is a MSYS issue rather than a `rg` issue.

---

_Comment by @Zyst on 2018-04-20 16:13_

This is how this works in normal grep under MSYS, as a side note.

![image](https://user-images.githubusercontent.com/3832380/39062016-d7aeed0e-448b-11e8-920b-f6bfb0dc79ec.png)


---

_Comment by @BurntSushi on 2018-04-20 16:19_

Sorry, but I don't understand the problem. Could you please investigate what's different about grep that allows it to work?

---

_Label `question` added by @BurntSushi on 2018-04-20 16:19_

---

_Comment by @Zyst on 2018-04-20 16:22_

If I had to guess it's probably the leading `./`, but I'm not too clear on MSYS differences.

---

_Comment by @BurntSushi on 2018-04-20 16:27_

Could you please test that theory by using `rg fast -l ././`? (Yes, the repeated `./` is intentional.)

---

_Comment by @Zyst on 2018-04-20 17:40_

That didn't seem to do anything.

```bash
~/dev/ripgrep$ rg fast -l ././

appveyor.yml
CHANGELOG.md
FAQ.md
GUIDE.md
README.md
...
```

```bash
~/dev/ripgrep$ rg fast -l ././ | xargs sed s/fast/FAST/g -i

sed: can't read appveyor.yml: No such file or directory
sed: can't read CHANGELOG.md: No such file or directory
sed: can't read FAQ.md: No such file or directory
sed: can't read GUIDE.md: No such file or directory
sed: can't read README.md: No such file or directory
...
```

---

_Comment by @BurntSushi on 2018-04-24 15:31_

@Zyst Could you please provide more instructions on how to reproduce this? What terminal do I need to install on Windows? Could you provide a link please?

I cannot reproduce this in my cygwin terminal, which I thought was MSYS based.

---

_Comment by @Zyst on 2018-04-24 16:26_

The link to install this is https://git-scm.com/downloads

As far as I know just installing that, and using the included terminal emulation tool (Git Bash) should be enough.

You don't need to use the terminal emulation tool per se, it just ensures you are running that environment. I can also replicate this in VS Code using git bash.

---

_Comment by @BurntSushi on 2018-04-24 16:47_

OK, I installed git bash and ran this:

```
$ rg 'faster than' -l | xargs sed 's/fast/FAST/' | rg fast
sed: can't read srcpathutil.rs: No such file or directory
sed: can't read srcsearch_buffer.rs: No such file or directory
sed: can't read globsetsrcglob.rs: No such file or directory
[FASTmod](https://github.com/facebookincubator/fastmod)
like `FASTer` will. `faste` would also match!
In this case, we used `FAST\w*` for our pattern instead of `fast\w+`. The `*`
`FAST\s+(\w+)`, which matches `fast`, followed by any amount of whitespace,
$ rg 'FAST\s+(\w+)' README.md -r 'fast-$1'
Our replacement string here, `FAST-$1`, consists of `fast-` followed by the
$ rg 'FAST\s+(?P<word>\w+)' README.md -r 'fast-$word'
  That is `rg -i FAST` matches `fast`, `fASt`, `FAST`, etc.
```

It looks like your environment doesn't like standard Windows paths. You can replace `\` with `/` though easily enough:

```
$ rg 'faster than' -l --path-separator '\x2F' | xargs sed 's/fast/FAST/' | rg fast
[FASTmod](https://github.com/facebookincubator/fastmod)
like `FASTer` will. `faste` would also match!
In this case, we used `FAST\w*` for our pattern instead of `fast\w+`. The `*`
`FAST\s+(\w+)`, which matches `fast`, followed by any amount of whitespace,
$ rg 'FAST\s+(\w+)' README.md -r 'fast-$1'
Our replacement string here, `FAST-$1`, consists of `fast-` followed by the
$ rg 'FAST\s+(?P<word>\w+)' README.md -r 'fast-$word'
  That is `rg -i FAST` matches `fast`, `fASt`, `FAST`, etc.
```

And that seems to work.

---

_Comment by @Zyst on 2018-04-24 18:08_

Right, in my case it's not a huge issue I normally fire up WSL, and just use zsh to do my find/grep/sed stuff since the tab autocomplete is much better there.

I just figured I'd report it in case this is the kind of stuff that can be "fixed".

Seems like it might not be fixable unless we can detect the environment, so feel free to close this issue at any time.

---

_Comment by @BurntSushi on 2018-04-24 18:09_

Yeah, at the end of the day, this is a result of trying to shoehorn Unix-style paths into a Windows environment. Fixing that is beyond my capacity.

---

_Closed by @BurntSushi on 2018-04-24 18:09_

---

_Comment by @cyrossignol on 2018-06-07 19:14_

The problem here seems to be that rg isn't detecting the pipe in an MSYS environment, so it outputs the ANSI color escape sequences. This works correctly in Cygwin.

Try: 

```
rg -l ... | od -c
rg --color never -l ... | od -c
```

We can see the escape sequences in the output of the first command.

Sidenote: Unlike Cygwin, MSYS performs path-mangling by default to rewrite Windows-style paths to Unix-style with MSYS's filesystem prefix if needed&mdash;even for piped output&mdash;so I don't think this is another path conversion issue.

For Cygwin specifically (or for MSYS with path-conversion turned off) we can resolve paths between `rg -l` and Cygwin/MSYS programs by streaming the output of rg through `cygpath`:

```
rg -l ... | cygpath --file -
```

---

_Comment by @BatmanAoD on 2018-06-28 17:16_

My experience matches @cyrossignol's when using Git Bash. Making an alias like `rgplain` or something will probably be my interim solution, but I'd be interested to know why `rg` isn't able to detect the output pipe in this case.

---

_Comment by @BurntSushi on 2018-06-28 17:33_

Hmmm, I don't remember reading @cyrossignol's comment, but that looks like a nice catch. This warrants a closer look. My guess at this point is that the `atty` crate will need some tweaking in how it detects the pipe (which is basically one giant hack on Windows), and it isn't surprising at all that there are some cases where it fails.

---

_Reopened by @BurntSushi on 2018-06-28 17:33_

---

_Comment by @BatmanAoD on 2018-06-28 18:44_

I'm guessing this is related to #19 and [this comment on #94](https://github.com/BurntSushi/ripgrep/issues/94#issuecomment-261745480). It sounds like it's much more trouble to fix than it's worth.

---

_Comment by @BatmanAoD on 2018-06-28 18:53_

...except your last comments in #94 indicate that pipe detection did start working. Maybe it only works for *input* pipes, and the same logic just needs to be used for *output* pipes?

---

_Comment by @BatmanAoD on 2018-06-28 18:55_

(Also, I like to think that there's a light at the end of the tunnel here for Windows; the Linux Subsystem for Windows seems like a promising start in the direction of Windows users not needing to use Cygwin/MSYS anymore. Some day, some day...)

---

_Comment by @BurntSushi on 2018-06-28 20:52_

> Maybe it only works for input pipes, and the same logic just needs to be used for output pipes?

In theory it should be. You can see the code for that here, there isn't much of it: https://github.com/softprops/atty/blob/master/src/lib.rs

In particular, the msys hack is dependent on implementation specific strings. So it might just be that those need to be updated.

This is something that I'll look into next time I make the Windows rounds for ripgrep. But otherwise, anyone else can do this as well. What I do is:

* Checkout ripgrep.
* Checkout atty.
* Modify `atty = "..."` in ripgrep's `Cargo.toml` to `atty = { version = "*", path = "path/to/atty/checkout" }`.
* Do printf-debugging in `path/to/atty/checkout/src/lib.rs` to see what [the value of `name`](https://github.com/softprops/atty/blob/4a7e706c0275cc94f7f34b5097ef229d428d83c2/src/lib.rs#L136) is.
* If nothing is printed, then the problem has likely been misdiagnosed and more debugging is required. Otherwise, if the string printed doesn't line up with [the test check](https://github.com/softprops/atty/blob/4a7e706c0275cc94f7f34b5097ef229d428d83c2/src/lib.rs#L144-L146), then fix the check and submit a PR to atty. :-)

---

_Comment by @BatmanAoD on 2018-06-28 21:21_

That sounds like something I should be able to manage over the weekend!

---

_Comment by @cyrossignol on 2018-06-28 22:46_

I actually played around with this a few days ago. It seems like the version of atty (0.2.10) that ripgrep depends on at the tip of master fixes this problem. 

I can reproduce the issue when rebuilding ripgrep 0.8.1 (atty 0.2.2). The problem disappears when rebuilding 0.8.1 against the newer atty version.

I wanted to find the change before commenting, but got sidetracked. Looking back now, it's softprops/atty#24 (released with 0.2.7) that fixes the issue. 

In short: this should be fixed with the next release of ripgrep.

---

_Comment by @BurntSushi on 2018-06-28 22:55_

@cyrossignol Oh wow, that'd be great! I forgot about that. Note that I think https://github.com/softprops/atty/pull/24 introduced a bug that I later fixed in https://github.com/softprops/atty/pull/25. But that was only for cygwin I think. It's hard to keep this stuff straight. :-)

---

_Comment by @BurntSushi on 2018-07-22 16:19_

OK, I've tried this out myself and this appears to be working as well as one might hope. I still needed to either set the path separator or use `cygpath` in my MSYS terminal in order to get the OP's original command to work, but AFAIK, this is just another instance of weird path mangling that is probably never going to get fixed. Either `cygpath` or `--path-separator` are the intended work arounds.

---

_Closed by @BurntSushi on 2018-07-22 16:19_

---

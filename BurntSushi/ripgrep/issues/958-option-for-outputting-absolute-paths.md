```yaml
number: 958
title: Option for outputting absolute paths
type: issue
state: closed
author: SigmundVik
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-06-21T11:39:08Z
updated_at: 2025-02-14T07:09:19Z
url: https://github.com/BurntSushi/ripgrep/issues/958
synced_at: 2026-01-12T16:13:22Z
```

# Option for outputting absolute paths

---

_@SigmundVik_

It would be great if ripgrep had an option that would change the output file paths to be absolute instead of relative.

---

_Comment by @BurntSushi on 2018-06-21 11:44_

Why? What problem are you trying to solve?

---

_Label `question` added by @BurntSushi on 2018-06-21 11:45_

---

_Comment by @SigmundVik on 2018-07-03 06:02_

I have a macro in the text editor that I'm using (SlickEdit) that interprets the content of the clipboard as a file path (and line number if present) and opens the file with that path (and jumps to the line number).

For this macro to work either the path needs to be relative to SlickEdit's current working directory, or it needs to be an absolute path.

To support this I have added a TCC alias (TCC is part Take Command, a Windows CMD replacement):

```
alias rg `(%@if[%TREE_ROOT==none,%DEFAULT_TREE_ROOT%,%TREE_ROOT%]\base-00\exe\rg ^ 
    %$ -n --no-ignore-parent --color never --no-heading . ^
    | b sed 's/^^/%@replace[\,\\\,%_CWD]\\\/')`
```

The `b` alias expands to a Cygwin bin directory, so this alias uses sed to turn the relative paths into absolute paths.  This works fine except that it makes grepping slower and also requires command grouping when redirecting the output:

```
[Windows] 07:40 0:00:00.34 332514M c:\users\viks\source\repos\consoleapplication1>rg main
C:\Users\VikS\source\repos\ConsoleApplication1\ConsoleApplication1\ConsoleApplication1.cpp:7:int main()
```

```
[Windows] 07:46 0:00:01.23 332513M c:\users\viks\source\repos\consoleapplication1>(rg main) > out.txt 
```

The latter command hangs when `rg main` is not enclosed in parentheses.

So long story short, the problem I am trying to solve is:
 - Better performance when outputting absolute paths for ripgrep (and easier aliases)
 - Avoid the need of TCC command grouping when redirecting output from my rg alias

---

_Comment by @BurntSushi on 2018-07-03 10:53_

@SiggyBar Thanks for elaborating! Unfortunately, I don't think this is a good fit for ripgrep. It sounds like you have a decent enough work around for what sounds like a pretty niche problem.

---

_Closed by @BurntSushi on 2018-07-03 10:53_

---

_Label `wontfix` added by @BurntSushi on 2018-07-03 10:53_

---

_Comment by @wpcarro on 2018-07-11 21:42_

Would love an absolute path option as well... I have an iTerm integration that'll open files in Emacs. It only works with absolute paths though.

Unfortunately piping to `realpath` isn't an option. I could pipe to `awk` and do some conditional piping to `realpath`, but this feels dirty.

@BurntSushi is supporting absolute paths totally out of the question? 

---

_Comment by @BurntSushi on 2018-07-11 22:02_

@wpcarro Yes. I'm sure you can build a suitable work-around.

---

_Comment by @timotheecour on 2018-07-11 23:56_



@BurntSushi 
I also would like an option for outputting absolute paths. I don't see how that's a niche problem, judging from others mentioning here they have the with the same issue.
in my case, I want to be able to open the file in sublime text (subl) or in fact any editor, and I need the file being shown, eg:
```
rg -t nim --no-heading format foo*.nim
# no file is shown !! unusable...
2204:  ## It uses the format ``yyyy-MM-dd'T'HH-mm-sszzz``.
2208:  result = format(dt, "yyyy-MM-dd'T'HH:mm:sszzz")
... long list
2212:  ## time zone and use the format ``yyyy-MM-dd'T'HH-mm-sszzz``.
```
subl 2212: # doesn't work

with proposed --fullPath:
```
rg -t nim --no-heading --fullPath format foo*.nim
/abs/path/to/foo.nim:2204:  ## It uses the format ``yyyy-MM-dd'T'HH-mm-sszzz``.
...
```

subl /abs/path/to/foo.nim:2204: #works


> Yes. I'm sure you can build a suitable work-around.

not possible in lots of use cases, eg when using rg through a command that cd's to another directory first (so `pwd` is not the correct thing), or when (as in my example above), no file name is shown at all


The workarounds are massively more complex than adding in the option at the source (say with --fullPath)

---

_Comment by @BurntSushi on 2018-07-12 10:47_

> I don't see how that's a niche problem, judging from others mentioning here they have the with the same issue.

I guess I need to stop using the word "niche" because you aren't the first to see it as an invitation to pick at my use of it. The problem is that the word "niche" bundles up quite a bit, and re-litigating this every single time I say "no" to something is _seriously_ tiring. To elaborate, what I'm saying here is that "this feature, in balance doesn't carry its weight." Namely, that the number of folks clamoring for it, in the context of the problems it solves and in the context of available work arounds, **in my opinion**, do not sufficiently counter balance the work required to implement it, maintain it and the time required to respond to future feature requests and bug reports related to this feature. This is an inherently **subjective** assessment on my part as the maintainer, since the only data I have is my ability to guess based on responses in the issue tracker. Moreover, I find it very difficult to, on occasion, trust the perspective of end users implicitly because they often have tunnel vision and unfairly weight _their pet features_ above all other costs that are, frankly, invisible to them but _highly_ visible to me.

The end result is a delicate balance. As such, I have frequently reversed course on features I was previously opposed to as my calculus changes due to updated data. That could happen for this feature too, but three people asking for it to service use cases that I find down right strange ain't gunna do it. Sorry.

For example:

> I want to be able to open the file in sublime text (subl) or in fact any editor

This just doesn't make any sense to me. I do things like this all the time:

```
$ vim $(rg -l pattern)
```

and it _just works_.

If, for some reason, your particular editor is incapable of dealing with relative file paths (which seems crazy to me, but hey, maybe it's a thing), then I really don't see why the onus should be on ripgrep to solve that problem. Moreover, ripgrep can already emit absolute file paths because the file paths it does emit are based directly on the file path you give it in the first place. Compare:

```
$ pwd
/home/andrew/rust/ripgrep

$ rg 'fn is_empty' | cat
globset/src/lib.rs:    pub fn is_empty(&self) -> bool {
grep-matcher/src/lib.rs:    pub fn is_empty(&self) -> bool {
grep-matcher/src/lib.rs:    fn is_empty(&self) -> bool {
termcolor/src/lib.rs:    pub fn is_empty(&self) -> bool {
ignore/src/gitignore.rs:    pub fn is_empty(&self) -> bool {
ignore/src/types.rs:    pub fn is_empty(&self) -> bool {
ignore/src/overrides.rs:    pub fn is_empty(&self) -> bool {

$ rg 'fn is_empty' $(pwd) | cat
/home/andrew/rust/ripgrep/globset/src/lib.rs:    pub fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/grep-matcher/src/lib.rs:    pub fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/grep-matcher/src/lib.rs:    fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/termcolor/src/lib.rs:    pub fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/ignore/src/gitignore.rs:    pub fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/ignore/src/types.rs:    pub fn is_empty(&self) -> bool {
/home/andrew/rust/ripgrep/ignore/src/overrides.rs:    pub fn is_empty(&self) -> bool {
```

> eg when using rg through a command that cd's to another directory first (so pwd is not the correct thing)

Sorry, but I don't understand this. If you run `rg foo` without an explicit path, then that's equivalent to `rg foo ./` which is in turn equivalent to `rg foo $(pwd)` in terms of the directory that is searched.

> or when (as in my example above), no file name is shown at all

This is entirely expected and matches the standard behavior of `grep`. If ripgrep only searches one file given by explicit arguments (in this case, your glob must have matched exactly one file), then it does not print the file name by default. You can force the issue with the `--with-filename` flag. Case in point:

```
$ rg 'fn is_empty' ignore/src/git*.rs
165:    pub fn is_empty(&self) -> bool {

$ rg 'fn is_empty' ignore/src/git*.rs --with-filename
ignore/src/gitignore.rs
165:    pub fn is_empty(&self) -> bool {
```

Describing this behavior as "unusable" is seriously unproductive.

---

_Comment by @SigmundVik on 2018-07-12 14:30_

Thank you for making it clear that ripgrep already can emit absolute file paths.  It is a bit embarrassing that I did not realize this myself..

I changed my alias from
```
alias rg `%@if[%TREE_ROOT==none,%DEFAULT_TREE_ROOT%,%TREE_ROOT%]\base-00\exe\rg ^
    %$ -n --no-ignore-parent --color never --no-heading . ^
    | b sed 's/^^/%@replace[\,\\\,%_CWD]\\\/')`
```
to
```
alias rg `%@if[%TREE_ROOT==none,%DEFAULT_TREE_ROOT%,%TREE_ROOT%]\base-00\exe\rg ^
    %$ -n --no-ignore-parent --color never --no-heading %_CWD`
```

Now it works exacly like I want, and I have no need for the option that I initially asked for.

Thank you for making ripgrep, it is a great tool!

---

_Comment by @alloc33 on 2025-02-14 07:09_

rg -l -0 my_text | xargs -0 realpath

---

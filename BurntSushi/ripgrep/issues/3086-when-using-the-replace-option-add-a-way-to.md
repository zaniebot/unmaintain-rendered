```yaml
number: 3086
title: "When using the `--replace` option, add a way to display the pre-replacement text but in a different colour."
type: issue
state: closed
author: IsaacOscar
labels:
  - wontfix
assignees: []
created_at: 2025-07-04T07:35:57Z
updated_at: 2025-07-04T10:07:03Z
url: https://github.com/BurntSushi/ripgrep/issues/3086
synced_at: 2026-01-12T16:13:25Z
```

# When using the `--replace` option, add a way to display the pre-replacement text but in a different colour.

---

_@IsaacOscar_

Consider the following example file:
```
$ cat file
hello
world
```

And the following ripgrep command:
```
$rg --colors match:fg:green --multiline  --replace $'O\nW'  $'o\nw'  file
1:hellO
2:World
```
Basically it capitalises the 'o' and the end of hello and the 'W' at the start of 'World', and highlights them in *green*. (I'm using multiline search and replacement strings as colouring them nicely isn't easy)

What I want is to also show the original match but highlight it in a different colour. For example, something like this:
```
$  rg --multiline  --replace $'O\nW' $'o\nw' file | git diff --color-words=. file -
diff --git a/file b/-
index 94954ab..0000000 100644
--- a/file
+++ b/-
@@ -1,2 +1,2 @@
hello
wO
World
```

Where the lowercase 'o and w' are *red*, and the uppercase O and W are *green*. 
(Obviously I don't want ripgrep to output all the diff format stuff like 'diff - ... and index')

I found a hacky way to do this with a bash function `colour`, where instead of `--replace <replacement>` you use `--replace "$(colour <replacement>)"`:
```
$ function colour {
	NL=$'\n'
	GREEN=$'\e[32m'
	NORM=$'\e[m'

	# In the following, the \$0 will output the entire match (prior to replacement)
	# the $GREEN ensures the first line of the displayed replacement text is green
	# and the ${1//$NL/$NL$GREEN} outputs the replacement text, but ensuring that each newline is also green
	# finally, $NORM restores the colour
	printf "\$0$GREEN${1//$NL/$NL$GREEN}$NORM"
}
$ rg  --multiline --replace "$(colour $'O\nW')" $'o\nw' file
1:hello
2:wO
3:World
```
Where the lowercase 'o and w' are *red*, and the uppercase O and W are *green*. 

So perhaps `--colors` can be extended to support something like `--colors=original:fg:red` that does the same thing? (and if you don't provide a `original:` format it doesn't output the original text at all).

It would probably better display the output in two lines like this:
```
1:helloO
3:wWorld
```
But I don't think I can do that using a bash function.


---

_Label `wontfix` added by @BurntSushi on 2025-07-04 10:07_

---

_Comment by @BurntSushi on 2025-07-04 10:07_

I don't think this is a good fit for ripgrep. Its replacement functionality is meant to be simple and stay simple. I have resisted extending it with more options, and this is the kind of thing that is likely to want more configurability.

---

_Closed by @BurntSushi on 2025-07-04 10:07_

---

```yaml
number: 390
title: Add space after linenumber
type: issue
state: closed
author: zwieberl
labels: []
assignees: []
created_at: 2017-03-02T08:33:54Z
updated_at: 2021-11-29T21:49:03Z
url: https://github.com/BurntSushi/ripgrep/issues/390
synced_at: 2026-01-12T16:13:21Z
```

# Add space after linenumber

---

_@zwieberl_

Hi, 
is it possible to add a space after the line-number? 

The reason for this is the following:
If I do for example a ```git blame SomeFile | rg some_interesting_function```, I get output like this:
```137:bdabecea    The Author    some_interesting_function(abc)```
To get more information, I usually do a doubleclick on the commit-hash and then copy it (e.g. for a ```git show```), but with the line-number directly glued to the hash, it gets selected as well and I have to manually delete the line-number (otherwise I would have ```git blame 137:bdabecea```). 

I think some shells are intelligent enough to only select the hash, but mine selects everything between whitespaces. So an added space would fix this issue for me.
Is that possible or would that break some output-parsing-compatibility?

---

_Comment by @BurntSushi on 2017-03-02 11:30_

ripgrep should emit the contents exactly as they are in the file. This is what every other search tool does and ripgrep is not going to break from it. You'd have the same problem with grep.

> I usually do a doubleclick on the commit-hash

Either don't doubleclick (click-and-drag instead), write the commit hash (it's only 6 letters) or disable line numbers with `-N`.

---

_Closed by @BurntSushi on 2017-03-02 11:30_

---

_Comment by @Volker-Weissmann on 2021-03-13 15:14_

Can we add this as an optional flag? 
If you Ctrl+Click on "filename:123 otherstuff" in vscode it opens the file and goes to line 123. But if rg outputs "filename:123otherstuff" vscode tries to find a file named "filename:123otherstuff".

---

_Comment by @Volker-Weissmann on 2021-05-27 15:13_

Just in case anyone has the same issue as me.

This is more or less fixed in the newest version of ripgrep, because now it prints `filename:123:otherstuff` (notice the second ':') (at least if you set the --no-heading flag). 
If anyone wants to implement zwieberl's request, take a loot at the `separator_field_match` variable of the `struct Config`. it is usually ":".
`fn write_prelude` of `struct StandardImpl` is what prints filename, linenumber and match with the colons in between.

---

_Comment by @BurntSushi on 2021-05-27 15:14_

@Volker-Weissmann The `filename:123:otherstuff` format has been available since the first release of ripgrep. And there have been no changes to it.

---

_Comment by @Volker-Weissmann on 2021-05-27 15:22_

> @Volker-Weissmann The `filename:123:otherstuff` format has been available since the first release of ripgrep. And there have been no changes to it.

Yes, I also just realised that I was talking BS. 
For myself, I now solved it using the one-line patch below (applies on 94e4b8e301302097dad48b292560ce135c4d4926). 
If anyone else wants this, I can make a proper PR with a proper flag.


```
diff --git a/crates/printer/src/standard.rs b/crates/printer/src/standard.rs
index 943bd79..14284d4 100644
--- a/crates/printer/src/standard.rs
+++ b/crates/printer/src/standard.rs
@@ -1145,6 +1145,7 @@ impl<'a, M: Matcher, W: WriteColor> StandardImpl<'a, M, W> {
                 self.write_column_number(n, sep)?;
             }
         }
+        self.write(b" ")?;
         if self.config().byte_offset {
             self.write_byte_offset(absolute_byte_offset, sep)?;
         }
```

---

_Comment by @NQNStudios on 2021-11-29 18:11_

@Volker-Weissmann Your patch would help me out, because ~~in Visual Studio Code's terminal, you can control-click tokens in the syntax `<file>:<line>:` or `<file>:<line>:<column>:` to open the file at that line number in the text editor. Tokens with the syntax `<file>:<line>:<randomToken>`  break this feature for me.~~ of the same reason you outlined and I skimmed over so I wrote it again twice. 😅 

---

_Comment by @Volker-Weissmann on 2021-11-29 18:26_

> @Volker-Weissmann Your patch would help me out

What do you mean, it *would* help you out, either it did or it did not.



---

_Comment by @NQNStudios on 2021-11-29 18:41_

Oh--judging by this thread I thought you made the patch on your own fork but didn't PR it.

---

_Comment by @Volker-Weissmann on 2021-11-29 18:43_

No, I just use 94e4b8e301302097dad48b292560ce135c4d4926 with the patch I posted.

But creating a PR that adds a flag for this would be useful.

---

_Comment by @NQNStudios on 2021-11-29 18:46_

Yeah, I would like for there to be a PR adding a flag.

---

_Comment by @Volker-Weissmann on 2021-11-29 18:47_

Do you want to write that or should I write that?

---

_Comment by @NQNStudios on 2021-11-29 18:57_

If you have time, it would be excellent if you could. Let's check with @BurntSushi, would this PR be welcome?

---

_Comment by @BurntSushi on 2021-11-29 20:06_

I would recommend you instead lobby tools to fix their "click" functionality. Not all tools get this wrong and it's an incredibly common format. (This is the traditional grep format.)

The problem with "just add another flag" is that to you it looks like a triviality, but if I don't say no to things, the number of flags grows and grows and grows. The number growing is a problem unto itself, but really only a problem that is visible to me through other end user complaints.

---

_Comment by @NQNStudios on 2021-11-29 20:26_

That makes sense. I'll see if there's an open issue for this on VSCode, or if we need to make a new one.

---

_Comment by @Volker-Weissmann on 2021-11-29 21:49_

> I would recommend you instead lobby tools to fix their "click" functionality. Not all tools get this wrong and it's an incredibly common format. (This is the traditional grep format.)
> 
> The problem with "just add another flag" is that to you it looks like a triviality, but if I don't say no to things, the number of flags grows and grows and grows. The number growing is a problem unto itself, but really only a problem that is visible to me through other end user complaints.

I know that I was the guy that proposed the PR, but I really like both of your arguments.



---

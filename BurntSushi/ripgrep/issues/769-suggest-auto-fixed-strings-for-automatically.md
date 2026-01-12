```yaml
number: 769
title: Suggest --auto-fixed-strings for automatically switch to fixed-string mode
type: issue
state: closed
author: wsdjeg
labels:
  - question
assignees: []
created_at: 2018-02-01T14:27:11Z
updated_at: 2018-02-28T16:16:27Z
url: https://github.com/BurntSushi/ripgrep/issues/769
synced_at: 2026-01-12T16:13:22Z
```

# Suggest --auto-fixed-strings for automatically switch to fixed-string mode

---

_@wsdjeg_

when using this project as a backgroud tools, I hope there is an option for auto fix string. that means using the pattern as expression all the time. when it contains errors, treat it as a fixed string.

Edit: sorry for long title.

---

_Comment by @BurntSushi on 2018-02-01 14:50_

This feels very similar to how "smart" case works. Smart case works by looking at the pattern, and if it contains all lowercase letters, then it enables case insensitive search. If there are any uppercase letters though, then case sensitive search is used. The silver searcher enables this by default. ripgrep has it, but is not enabled by default.

It kind of feels like a `--smart-fixed-strings` flag would be consistent with how `--smart-case` works.

With that said, I'm not sure what the use case is here. It doesn't really seem like you'd want this enabled by default, which means you'd need to selectively enable it. At that point, you might as well just use the `-F` flag. This feels different from `--smart-case` in that I imagine the folks that use it probably embed it into an alias. But I don't think that's the case for `--smart-fixed-strings`?

Could you elaborate more on the use case here? I know you posted a gif in the other thread and talked about building a tool, but that doesn't really tell me much. Why do you specifically need an "auto" or "smart" variant of `--fixed-strings`? Why doesn't just using `--fixed-strings` work as is?

---

_Label `question` added by @BurntSushi on 2018-02-01 14:50_

---

_Comment by @wsdjeg on 2018-02-03 12:55_

@BurntSushi in my plugin flygrep.vim, I use a while loop to run `rg` commend with the input from user. here is the main logic:

```
let pattern = ''

while 1
   let pattern .= nr2char(getchar())
   call jobstart(['rg', '--hidden', '--no-heading', '--vimgrep', '-S', pattern])
endwhile
```

I call this kind of plugin is fly on grep, that means it will update result when user input a char.

but when the pattern is not a expression, it will cause error. so I hope there is an option.
then I can use `call jobstart(['rg', '--hidden', '--no-heading', '--vimgrep', '-S',  '--auto-fixed-strings', pattern])`, if the pattern is not a expression, just treat it as a fixed string, otherwise use it as expression.

and another error is, if the pattern is just same as an option of rg, for example if the patten is `--hidden`, then this comman also run failed. maybe I should use
`call jobstart(['rg', '--hidden', '--no-heading', '--vimgrep', '-S', '--', pattern])`, just add an separator before pattern.

---

_Comment by @BurntSushi on 2018-02-03 14:04_

> and another error is, if the pattern is just same as an option of rg, for example if the patten is --hidden, then this comman also run failed

To get the easy one out of the way, yes, using `--` is what you need to do. Alternatively, you can use the `-e/--regexp` flag followed by the pattern. This is documented in the output of `rg --help`.

> I call this kind of plugin is fly on grep, that means it will update result when user input a char.

Oh OK, I understand now. You want to use ripgrep in a context where it is very responsive to user input and don't want ripgrep to fail if the pattern the user has typed is incomplete.

Unfortunately, I'm not sure I agree with your proposed fix. For example, consider the following sequence of characters that I type. Each additional input is labeled with a number so that I refer to it later:

```
1: a
2: a+
3: a+(
4: a+(z
5: a+(z)
```

If ripgrep is invoked with each of these inputs using a hypothetical `--auto-fixed-strings` or `--smart-fixed-strings` flag, then there are definitely user visible inconsistencies here that I think I would consider disorienting. For example, (2) would show matches consisting of one or more `a` characters, but (3) would show matches consisting only of the literal string `a+(`. Similarly for (4). Once the user hits (5), then you revert back to one or more `a` characters followed by a single `z` character.

The result is that these search queries are very different and will lead to very different results, which IMO would make the incremental search quite disorienting!

Honestly, I think that you, as the author of flygrep, need to handle this case yourself in a way that is not disorienting to end users. Of course, I agree that showing an error instead of results because of an incomplete pattern isn't great. You might instead consider that if ripgrep reports a syntax error, then flygrep shouldn't update the result window and instead keep the previous results. You will also need to handle the case when the user has completed the pattern but committed a real syntax error. I'm not sure how to handle that, but perhaps a timeout of some sort or some other user feedback (like highlighting the pattern in red or something) would be appropriate.

---

_Comment by @wsdjeg on 2018-02-03 15:14_

Thank you for your very detailed answer. I think I have found the solution.

> would make the incremental search quite disorienting!

and yes,  the number of searching results would look just like electrocardiography.  :) , and I agree with you, this should not be what the user want.

I will use a `timer` to run rg command, and the result will be shown 200ms after user input. that means if user quick input, this command will not run until the user stop input.



---

_Closed by @wsdjeg on 2018-02-03 15:14_

---

_Comment by @devlinzed on 2018-02-28 15:41_

What about using fixed strings when the string is purely alphanumeric?

---

_Comment by @BurntSushi on 2018-02-28 15:48_

@devlinzed ... why?

---

_Comment by @BurntSushi on 2018-02-28 15:49_

There is **exactly one** purpose for `-F/--fixed-strings`: to avoid needing to escape regex meta characters. If you don't need to escape meta characters, then there is no need to enable `-F/--fixed-strings`.

---

_Comment by @devlinzed on 2018-02-28 15:59_

Sorry, that's not the case with grep and I didn't consider ripgrep or the regex library might already optimize that case.

---

_Comment by @BurntSushi on 2018-02-28 16:16_

@devlinzed For GNU grep, it certainly is the case as well. It has the same type of optimization as ripgrep. BSD grep might be a different story.

---

```yaml
number: 8360
title: "Formatter: Specify indentation for function parameters"
type: issue
state: open
author: LeonarddeR
labels:
  - formatter
  - style
assignees: []
created_at: 2023-10-30T15:58:08Z
updated_at: 2026-01-10T22:23:42Z
url: https://github.com/astral-sh/ruff/issues/8360
synced_at: 2026-01-11T00:46:05Z
```

# Formatter: Specify indentation for function parameters

---

_Issue opened by @LeonarddeR on 2023-10-30 15:58_

The current indentation behavior for function parameters is: 4-space indentation at function definitions, like this:

```
def my_function(
    my_function_arg_1,
    my_function_arg_2,
    my_function_arg_3,
    my_function_arg_4,
):
    pass
```

This leads to reduced readability, because the function name and the argument names become harder to distinguish.

Adding one more level of indentation greatly improves readability:

```
def my_function(
        my_function_arg_1,
        my_function_arg_2,
        my_function_arg_3,
        my_function_arg_4,
):
    pass
```

PEP8 also has the same suggestion: https://peps.python.org/pep-0008/#indentation

_Originally posted by @jsh9 in https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7128822_

---

_Comment by @T-256 on 2023-10-30 21:20_

> ```
> def my_function(
>     my_function_arg_1,
>     my_function_arg_2,
>     my_function_arg_3,
>     my_function_arg_4,
> ):
>     pass
> ```
> 
> 
>     
>   
> 
> This leads to reduced readability, because the function name and the argument names become harder to distinguish.

I don't agree with this because it's rare in the wild to **_all_** parameters starts with **_fullname_** of function/class name.
BTW, in my view it's not reducing readability (this may differ for you).


---

_Label `formatter` added by @charliermarsh on 2023-10-30 23:32_

---

_Label `style` added by @charliermarsh on 2023-10-30 23:32_

---

_Comment by @MichaReiser on 2023-10-31 00:55_

I think this is the same as https://github.com/astral-sh/ruff/issues/8360

---

_Closed by @MichaReiser on 2023-10-31 00:55_

---

_Comment by @dhruvmanila on 2023-10-31 01:39_

> I think this is the same as #8360

@MichaReiser Can you update the comment to link the correct issue? Currently, it's referring to itself ;)

---

_Comment by @MichaReiser on 2023-10-31 01:59_

Why do I always do this... 🤦 I'm trying to find the issue again but can't for some reason. I saw it before. I'll re-open this in the meantime.

---

_Reopened by @MichaReiser on 2023-10-31 01:59_

---

_Comment by @rwightman on 2023-10-31 04:42_

I don't see why there is any need to downvote this, this a contentious issue based on numerous heated issues in black, etc that you can look up. The question is not whether you like it but whether there are enough who do.

The proposal is not to change the default, or force one way or another, but hopefully have a configuration option so that those of use who much prefer combining the PEP-8 recommendation of an extra indent level for function args with sad-face can do so.

Also, it is not just about readability in the contrived case where the args happen to match the function name, I feel the different alignment aids readbility regardless.



---

_Comment by @jsh9 on 2023-10-31 04:50_

> > ```
> > def my_function(
> >     my_function_arg_1,
> >     my_function_arg_2,
> >     my_function_arg_3,
> >     my_function_arg_4,
> > ):
> >     pass
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > This leads to reduced readability, because the function name and the argument names become harder to distinguish.
> 
> I don't agree with this because it's rare in the wild to **_all_** parameters starts with **_fullname_** of function/class name. BTW, in my view it's not reducing readability (this may differ for you).

@T-256 you might be missing my main point there.

Even with different names, it's still not very readable:

```
def perform_some_tasks(
    data: List[float],
    arg2: Union[int, str, bool],
    hello: Optional[int],
    good: Dict[str, Any],
    morning: float,
) -> None:
    ...
```

Look at this instead (much more readable):

```
def perform_some_tasks(
        data: List[float],
        arg2: Union[int, str, bool],
        hello: Optional[int],
        good: Dict[str, Any],
        morning: float,
) -> None:
    ...
```

---

_Comment by @LeonarddeR on 2023-10-31 07:00_

@rwightman wrote:

> The proposal is not to change the default, or force one way or another, but hopefully have a configuration option so that those of use who much prefer combining the PEP-8 recommendation of an extra indent level for function args with sad-face can do so.

Thanks for highlighting this. Indeed, it was not my intention to advocate for this to become the default in any way.

---

_Comment by @jankatins on 2023-10-31 13:29_

Just FYI: I downvoted this issue because I find any config for the format a negative thing, because it leads to longwinded bikesheding discussions and I really love the way black handled it: have some guiding principles (make diffs as short as possible, which means the indent should be as short as possible to not result in line breaks) and then take a decision and be done with it. The amount of time spent on discussing and/or tweaking styles (both on these issues/PRs or your own projects when you change some config) will (in my opinion) never be outweighed by any gains you have because some formatting is a tiny bit more readable than what black or now ruff does.

(and yes: been there, done it, will never get the time back I spent on making intelij format SQL the way I wanted it to... And also not the time I spend on resolving merge conflicts because I thought aligned AS was a good thing...)

---

_Comment by @T-256 on 2023-10-31 13:45_

> Even with different names, it's still not very readable:

Still, it's readable into my eyes, I think it's a personalization choose but perhaps modern editors with syntax highlighting solve your problem:
```py
def perform_some_tasks(
    data: List[float],
    arg2: Union[int, str, bool],
    hello: Optional[int],
    good: Dict[str, Any],
    morning: float,
) -> None:
    ...

def perform_some_tasks(
        data: List[float],
        arg2: Union[int, str, bool],
        hello: Optional[int],
        good: Dict[str, Any],
        morning: float,
) -> None:
    ...
```

I disliked because I'd not like to encounter codebases with this option enabled, the reason is that going to show lines fat, and in syntax colorized editor it's uglier. (This is my taste, maybe it is different for youً!)

---

_Comment by @josephsl on 2023-10-31 14:16_

Hi,

While indenting function arguments one extra level may not be readable for some, it may help programmers who must use alternatives to sight to program - several screen reading technologies include a feature to announce indentation level (one of them provides an option to announce indentation with audio). For example:

def my_function():
    var1 = some_value

Versus:
def my_function(
    var1 = some_value,
):


To most of us, the presence of comma at the end makes it clear that we are passing a keyword argument to this function. However, for people relying on assistive technologies (especially screen readers), programmers must infer from surrounding code (or subtle changes to output by text-to-speech engine as configured by the screen reading technology), unless they listen to speech output carefully, they may not know where function body starts and ends. A counterargument could be that blind programmers can ask the screen reader to announce all punctuation, and some would cite productivity as a reason to not take up that alternative. Another alternative is to ask people to use type hints in function definitions, but not all developers may heed that advice (type hints do inform programmers that we are reading function signature, not the body itself, quite useful if arguments are written on the same indentation as rest of the function body).

I understand that what I wrote above opens up a can of possibly unnecessary discussions, but I think it is also important to think about readability from the perspective of those who may not see what we wrote and must rely on alternative modes to figure out what's going on.

Thanks.

---

_Comment by @rwightman on 2023-10-31 17:54_

> Still, it's readable into my eyes, I think it's a personalization choose but perhaps modern editors with syntax highlighting solve your problem:

I use a 'modern editor', PyCharm, and it defaults to the extra indent level (continuation indent 8) for formatting because it's following PEP-8. Again, the extra indent is PEP-8. Sadface is not, but I do prefer sadface in combo with this.

---

_Comment by @jsh9 on 2023-11-01 07:09_

> > Even with different names, it's still not very readable:
> 
> Still, it's readable into my eyes, I think it's a personalization choose but perhaps modern editors with syntax highlighting solve your problem:
> 
> ```python
> def perform_some_tasks(
>     data: List[float],
>     arg2: Union[int, str, bool],
>     hello: Optional[int],
>     good: Dict[str, Any],
>     morning: float,
> ) -> None:
>     ...
> 
> def perform_some_tasks(
>         data: List[float],
>         arg2: Union[int, str, bool],
>         hello: Optional[int],
>         good: Dict[str, Any],
>         morning: float,
> ) -> None:
>     ...
> ```
> 
> I disliked because I'd not like to encounter codebases with this option enabled, the reason is that going to show lines fat, and in syntax colorized editor it's uglier. (This is my taste, maybe it is different for youً!)

I never rely on syntax highlighting because in many cases there is no syntax highlighting.  Just to name a few:
* Error logs (especially for web applications)
* Stack traces (especially when running stuff in the terminal)
* Code searches in terminal
* Sharing code snippets with people in Slack
* ...

---

_Comment by @jsh9 on 2023-11-01 07:18_

> The amount of time spent on discussing and/or tweaking styles (both on these issues/PRs or your own projects when you change some config) will never be outweighed by any gains you have because some formatting is a tiny bit more readable than what black or now ruff does.

@jankatins: I would suggest that we all refrain from making absolute statements like this (with phrases like "will never be outweighed by...").

If you can find statistics that support your "will never" claim, please feel free to share it here.  (And note that the statistics need to be statistically rigorous.)

Otherwise, I would suggest **_not_** making design choices that hurt people's productivity.

By the way, "a tiny bit more readable" is your personal opinion, and there are a lot of people who do not agree with your opinion.

---

_Comment by @jankatins on 2023-11-01 11:43_

> I would suggest that we all refrain from making absolute statements like this 

Added a "In my opinion".

I'm looking forward to the statistics why this additional config improves readability and productivity and not decreases is because of the time lost doing bike shedding.

---

_Comment by @Avasam on 2023-11-07 04:27_

> I never rely on syntax highlighting because in many cases there is no syntax highlighting.  Just to name a few:
> * Error logs (especially for web applications)
> * Stack traces (especially when running stuff in the terminal)
> * Code searches in terminal
> * Sharing code snippets with people in Slack
> * ...

Whilst I'm not a fan of this style, and I'd rather rely on syntax highlighting + empty left side, this is the best counter-argument I've seen to "just use syntax highlights". Validating it for readability reasons for those scenarios.

Not something I'd use in mine. But I could also work with a codebase that uses it (if autoformatted) w/o affecting my productivity.

---

_Comment by @jsh9 on 2024-01-15 00:59_

Hi authors of Ruff, may I follow up on this issue?  Do you have plans to include this configurability into Ruff in the near future?  Thanks!

---

_Comment by @MichaReiser on 2024-01-15 07:04_

@jsh9 this is not on our near-time roadmap because there are other features with a wider reach that we want to work on.

---

_Comment by @jsh9 on 2024-01-15 07:07_

Thanks @MichaReiser !  Would you be willing to point to us (people who would like this feature) where this feature is implemented in the code?

I'm not familiar with Rust, so it's difficult for me to contribute.  But with your pointers maybe I (or other people) can take a stab at it.

---

_Comment by @MichaReiser on 2024-01-15 09:24_

The complexity of the requested feature isn't specifically implementing it, but:

* Deciding if ruff wants to be less opinionated and support configuration flags as the one mentioned above
* Decide on a formatting: I remember that there was some discussion around if "sad-face" should be on its own or the same line 
* Consider the style change holistically. What does it mean if we allow changing the indentation for function (and lambda?) parameters. Are there other places where it should be possible to customize the indentation? How does it fit into the larger formatting style?


The code change itself is probably rather straightforward (I say that often and am then surprised that formatter changes are always hard :laughing: ). It might be as simple as adding an extra indent level to the code in

https://github.com/astral-sh/ruff/blob/febc69ab48416dacdad2499ccb6512dd8e56cc7c/crates/ruff_python_formatter/src/other/parameters.rs#L249-L282


```rust
write!(
    f,
    [
        token("("),
        dangling_open_parenthesis_comments(parenthesis_dangling),
        soft_two_level_block_indent(&group(&format_inner)),
        token(")")
    ]
)
```

It would require a custom `soft_block_indent` builder that writes two indents instead of just one:

```rust
let snapshot = f.snapshot();

f.write_element(FormatElement::Tag(StartIndent));
f.write_element(FormatElement::Tag(StartIndent)); // <---- this

match self.mode {
    IndentMode::Soft => write!(f, [soft_line_break()])?,
    IndentMode::Block => write!(f, [hard_line_break()])?,
    IndentMode::SoftLineOrSpace | IndentMode::SoftSpace => {
        write!(f, [soft_line_break_or_space()])?;
    }
}

let is_empty = {
    let mut recording = f.start_recording();
    recording.write_fmt(Arguments::from(&self.content))?;
    recording.stop().is_empty()
};

if is_empty {
    f.restore_snapshot(snapshot);
    return Ok(());
}

f.write_element(FormatElement::Tag(EndIndent));
f.write_element(FormatElement::Tag(EndIndent)); // <---- and this

match self.mode {
    IndentMode::Soft => write!(f, [soft_line_break()]),
    IndentMode::Block => write!(f, [hard_line_break()]),
    IndentMode::SoftSpace => write!(f, [soft_line_break_or_space()]),
    IndentMode::SoftLineOrSpace => Ok(()),
}
```

---

_Comment by @stolenzc on 2024-04-02 01:10_

I am watching this issue more than 1 month, and I always wait for implement this function in ruff. our company have some code style rules, All other rules can be satisfied through configuration, only this one I can't. This style also follow pep8 style.  I like ruff, but I can't directly using it in my work and life.

---

_Comment by @coinflip112 on 2024-05-30 13:04_

I'd very much appreciate this feature. I'll get bullied into using Pycharm's formatter otherwise 😅

---

_Comment by @jsh9 on 2024-07-05 07:06_

Hi @MichaReiser , 

May I follow up on this topic here?  Have you and/or the Ruff team discuss these questions that you outlined [above](https://github.com/astral-sh/ruff/issues/8360#issuecomment-1891659341)?

> The complexity of the requested feature isn't specifically implementing it, but:
> * Deciding if ruff wants to be less opinionated and support configuration flags as the one mentioned above
> * Decide on a formatting: I remember that there was some discussion around if "sad-face" should be on its own or the same line
> * Consider the style change holistically. What does it mean if we allow changing the indentation for function (and lambda?) parameters. Are there other places where it should be possible to customize the indentation? How does it fit into the larger formatting style?

Thank you!

---

_Comment by @MichaReiser on 2024-07-05 08:03_

No, I'm sorry. We haven't decided on this yet

---

_Comment by @vvlutin on 2024-07-30 13:19_

I think that to increase readability it might be reasonable to indent infinite loops by 3 levels? You know, diversity and variety, all that stuff. And I would personally prefer indent __init__ by 4 levels, so it finally can be seen by elder people

---

_Comment by @vvlutin on 2024-07-30 13:33_

Brackets are sufficient for readability in both cases above. So if readability is the goal then the first option is newline character before closing bracket. And the second option is complication of indentation logic by double indentation levels specifically for arguments lists. If complicating logic considered pythonic option, then maybe pep8 should allow both cases explicitly.

---

_Comment by @tonycoco on 2024-10-16 21:30_

Google's [`.pylintrc`](https://google.github.io/styleguide/pylintrc) currently sets `indent-after-paren=4`. I'm writing some of our open-source solutions and would like to set up a Ruff config equivalent. Looking to see if there's been any progress on a config/flag for this behavior.

---

_Comment by @RoelantStegmann on 2024-11-07 14:33_

Yes, also following this. Hope it will become adopted. 

---

_Comment by @jsh9 on 2024-11-30 18:19_

Hi @MichaReiser , I assume there are no updates to this issue because you were still contemplate the 3 questions you mentioned earlier this year:

> The complexity of the requested feature isn't specifically implementing it, but:
> 
> * Deciding if ruff wants to be less opinionated and support configuration flags as the one mentioned above
> * Decide on a formatting: I remember that there was some discussion around if "sad-face" should be on its own or the same line
> * Consider the style change holistically. What does it mean if we allow changing the indentation for function (and lambda?) parameters. Are there other places where it should be possible to customize the indentation? How does it fit into the larger formatting style?

To quote Voltaire, _"perfect is the enemy of good"_.  So maybe it's better if we reach a "good enough" consensus and then start to implement a solution?

Here are my opinions on these 3 questions, which hopefully can help push this forward.

**1. Whether Ruff wants to be less opinionated about this**

If you add such a config flag (`--double-indent-for-function-def`, or a better name), I don't think you'll offend anyone. Here's my reasoning:

Suppose we separate users into 3 groups: (A) people who support double indentation under function definitions, (B) people who strongly oppose this, (C) people who don't care.

- People in Group A will be very happy with this flag and start to adopt Ruff with this flag on.  
- People in Group B are not affected since they can keep using Ruff with this flag off.
- People in Group C don't care.

**2. Whether "sad-face" should be on its own line**

This is out of the scope of our request, which is to "add a level of indentation for _**function arguments**_ in function signature".  Note that this request is limited to the **function arguments** only.

**3. Holistic style change**

I don't think the styles in other places are affected by this, because the only reason that we are requesting a double indentation is that `def` has 3 letters, making the function name and function arguments lined up in a way that's not very readable:

```
def my_function(
    data,
    identity,
    some_args,
    some_more_args,
):
    pass
```

At least to my current knowledge, other places of the Python language don't have this issue, so the current style works fine.


---

_Comment by @rwightman on 2024-11-30 19:43_

@jsh9 agree with the reasoning, no surprise :) 

For me the 3 char + space def alignment is not consequential, it's the mental parse btw function args and fn body that is the biggest win. Also, I'm a pycharm user and this has always been the PEP preferred approach and a default for the builtin formatting, so it's engrained in my existing code and preferred style.

Sadface does help with that parse, but as soon as you add sadface w/ return typing it once again gets less obvious without the extra arg indent level.


---

_Comment by @vitor-amartins on 2025-04-04 13:47_

Hi @jsh9, agree with all the points you brought. Also @rwightman has exact the same point that get me here, it's the approach used on Pycharm, and it keeps fighting with the ruff format, and only because of that I choose not to use ruff format. If it had a flag like mentioned by @jsh9, it would be the thing that would make me use format.

@MichaReiser do you have any updates based on the presented points?

---

_Comment by @jsh9 on 2025-08-14 22:42_

Hi everyone on this thread, I created a pull request to attempt to implement this feature, based on @MichaReiser 's hint above (https://github.com/astral-sh/ruff/issues/8360#issuecomment-1891659341).

---

_Comment by @jsh9 on 2025-09-01 01:03_

Hi everyone who are interested in getting this feature implemented:

My PR (https://github.com/astral-sh/ruff/pull/19921) was closed by @MichaReiser with reasons given in [this comment](https://github.com/astral-sh/ruff/pull/19921#issuecomment-3190761907).  I asked a follow-up question in that PR, but got no reply.

I therefore forked Ruff and implemented this feature on my own fork.  All in accordance with the MIT license of Ruff.

My fork is called **_Muff_**: https://github.com/jsh9/muff.  It offers this formatting style by default (this is the only difference in functionality between Muff and Ruff), and Muff is published to PyPI: https://pypi.org/project/muff/

As of August 31, 2025, I complied binaries for these 5 platforms:

- x86_64 macOS
- ARM macOS
- x86_64 Linux (glibc 2.17, which [Ruff also appears to be offering](https://pypi.org/project/ruff/#files))
- ARM Linux
- x86_64 Windows

(I don't have an ARM-based Windows computer, and AWS doesn't offer ARM Windows EC2, so unfortunately there's no ARM Windows binary for now.)

I really like Ruff as a tool and I sincerely hope that @charliermarsh and team will eventually implement this feature.  Until then, I'll try my best to keep my fork up to date with Ruff.

---

_Comment by @stolenzc on 2025-09-01 07:03_

> Hi everyone who are interested in getting this feature implemented:大家好，有兴趣实现此功能的人：
> 
> My PR ([#19921](https://github.com/astral-sh/ruff/pull/19921)) was closed by [@MichaReiser](https://github.com/MichaReiser) with reasons given in [this comment](https://github.com/astral-sh/ruff/pull/19921#issuecomment-3190761907). I asked a follow-up question in that PR, but got no reply.我的 PR ( [#19921](https://github.com/astral-sh/ruff/pull/19921) ) 已被 关闭，原因已在 中给出。我在该 PR 中提出了后续问题，但没有得到回复。
> 
> I therefore forked Ruff and implemented this feature on my own fork. All in accordance with the MIT license of Ruff.因此，我 fork 了 Ruff，并在自己的 fork 上实现了此功能。所有操作均遵循 Ruff 的 MIT 许可证。
> 
> My fork is called **_Muff_**: https://github.com/jsh9/muff. It offers this formatting style by default (this is the only difference in functionality between Muff and Ruff), and Muff is published to PyPI: https://pypi.org/project/muff/我的 fork 版本名为 **_Muff_** ： https://github.com/jsh9/muff 。它默认提供以下格式样式（这是 Muff 和 Ruff 之间唯一的功能区别），并且 Muff 已发布到 PyPI： https://pypi.org/project/muff/
> 
> As of August 31, 2025, I complied binaries for these 5 platforms:截至 2025 年 8 月 31 日，我已为以下 5 个平台编译了二进制文件：
> 
> * x86_64 macOS
> * ARM macOS
> * x86_64 Linux (glibc 2.17, which [Ruff also appears to be offering](https://pypi.org/project/ruff/#files))x86_64 Linux（glibc 2.17， [Ruff 似乎也提供该版本 ](https://pypi.org/project/ruff/#files)）
> * ARM Linux
> * x86_64 Windows
> 
> (I don't have an ARM-based Windows computer, and AWS doesn't offer ARM Windows EC2, so unfortunately there's no ARM Windows binary for now.)（我没有基于 ARM 的 Windows 计算机，并且 AWS 不提供 ARM Windows EC2，因此遗憾的是目前没有 ARM Windows 二进制文件。）
> 
> I really like Ruff as a tool and I sincerely hope that [@charliermarsh](https://github.com/charliermarsh) and team will eventually implement this feature. Until then, I'll try my best to keep my fork up to date with Ruff.我非常喜欢 Ruff 这款工具，并真诚地希望团队最终能够实现这个功能。在此之前，我会尽力让我的 fork 与 Ruff 保持同步更新。

I think I'll give it a try, this problem still bothers me and I've been waiting for this feature for a long time. Thanks for your efforts

---

_Comment by @thaibui on 2025-11-13 09:55_

@MichaReiser Could you just merge the PR https://github.com/astral-sh/ruff/pull/19921 so that we don't have to have another fork? I followed this thread and ended up using `muff` instead of `ruff` simply because there isn't an option to change the default.

Thanks for the work @jsh9 & @MichaReiser 

---

_Comment by @jsh9 on 2026-01-10 22:23_

Happy 2026 everyone.

As of January 2026, I'm still maintaining the Muff fork (https://github.com/jsh9/muff). 

I keep the [Muff vs Ruff file diff](https://github.com/astral-sh/ruff/compare/main...jsh9:muff:main) as minimal as possible; I align Muff with the upstream (Ruff) regularly, but I only publish new versions **once every 1-2 months** due to the inconvenience of doing so.  (Compiling Rust code into binaries seems to require more powerful computing resources that GitHub Actions free tier doesn't offer, so compiling them manually on my own computers (physical ones or AWS).

I sincerely hope the Ruff team will eventually consider implementing this option (which I implemented in https://github.com/astral-sh/ruff/pull/19921).

---

```yaml
number: 12488
title: How to disable rules for ruff format?
type: issue
state: closed
author: AntiSol
labels:
  - formatter
  - style
assignees: []
created_at: 2024-07-24T09:26:57Z
updated_at: 2025-04-16T08:14:17Z
url: https://github.com/astral-sh/ruff/issues/12488
synced_at: 2026-01-12T15:54:51Z
```

# How to disable rules for ruff format?

---

_@AntiSol_

Hello, I'm trying to have `ruff format` not "fix" W293 (empty lines with whitespace). I've tried various searches and options but nothing seems to make it do what I want.

I've tried:
* extend-ignore += W293
* extend-unsafe-fixes += W293
* <-- I think the above aren't working because they're linter settings, not formatter? But I don't see a way to tell the formatter to ignore rules?
* reading through the formatter dox ad nauseum
* searching various terms like "ruff format W293"

No luck so far.

I'm sure there's just an option I've missed to turn off application of this rule. If you could point me at that it would be muchly appreciated.

Thanks for your time!

---

_Label `question` added by @MichaReiser on 2024-07-24 12:55_

---

_Comment by @MichaReiser on 2024-07-24 13:00_

I'm sorry for the confusion and frustrating experience that you had. 

The formatter and linter are two separate tools:

* `ruff format` runs the formatter. The formatter formats your code according to black's style guide and it only supports  a [handful settings](https://docs.astral.sh/ruff/settings/#format) 
* `ruff check` runs the linter. The linter is highly configurable and you can enable individual rules with `ignore`, `select` (and their `extend` versions). 

The reason why enabling `W293` has no effect is because it enables a lint rule, but it doesn't change how the formatter formats the code. 

What's your motivation for enabling `W293`. Is there a case where the formatter doesn't remove whitespace in an empty line?

---

_Comment by @AntiSol on 2024-07-24 17:20_

Thanks for responding so quickly. 
That's ok, my apologies, I didn't mean to vent, and my frustration is not rooted in your work but in ~silly~ decisions (black/pep8 guidelines) you took no part in and have had to enforce for the sake of compatibility with other tools :)

It seems I have not been clear: I want to _disable_ this behaviour - ruff continues to remove space from blank lines that I want to keep, even when I apply the settings I mentioned.

As mentioned, I looked at the formatter settings pretty extensively before posting. So it seems this behaviour cannot be disabled then?

Perhaps this should be considered a feature request, then: for this behaviour to be optional. 

Indentation  is significant in python, so it seems semantically incorrect to me to remove indenting in the middle of a block of code. Further it makes both reading and editing code more difficult and introduces the possibility of human error as it strips away the indentation I put there intentionally, forcing me to put my indentation back (possibly incorrectly) in order to add new code. 

Here's an illustration:
![image](https://github.com/user-attachments/assets/3415fdd5-626c-4a35-beec-a4985e06da62)

W293 is a silly rule (but, importantly, not your fault!) and should be optional in the formatter.

---

_Comment by @MichaReiser on 2024-07-25 06:24_

Oh I see. I'm sorry but Ruff's formatter doesn't support this level of configurability. 

I gues the reason why I haven't run into this before is because I tend to go to the end of the previous line and then hit enter. The editor then inserts the right indentation for me, so that I have no need to preserve indentation on empty lines.

---

_Comment by @AntiSol on 2024-07-26 00:45_

yeah, that workaround is fine most of the time, but it doesn't address the visual issues this rule creates.

It also just seems generally clumsy to have to work around this rule every time you try to edit a file - sure it's only 3 keystrokes, but it's 3 keystrokes basically any time you want to edit a file, and those 3 keystrokes should be unnecessary. Another reason enforcement should be optional.

I'd appreciate if you'd consider it as a feature request.

Thanks again for taking the time to get back to me, and for all your work on ruff :) 

---

_Label `question` removed by @MichaReiser on 2024-07-26 06:15_

---

_Label `formatter` added by @MichaReiser on 2024-07-26 06:15_

---

_Label `style` added by @MichaReiser on 2024-07-26 06:15_

---

_Comment by @MichaReiser on 2024-07-26 06:41_

I'm sorry, but I thought about this more, and I don't think the style change fits into the formatter.

* The formatter aims to be PEP 8 compatible. It only diverges in very few cases.
* The formatter is intentionally opinionated with few options. The supported options change the formatting style for things where there's not a clear community standard (a large portion of the community wants a different formatting) or to support legacy projects. 

The formatter also aims to enforce consistent formatting. Preserving leading whitespace in empty lines would not accomplish that because you might preserve the whitespace where *I* would remove it. The project then ends up with inconsistently formatted code. 

I'm going to close this as not planned. 



---

_Closed by @MichaReiser on 2024-07-26 06:41_

---

_Comment by @AntiSol on 2024-07-26 07:05_

1. PEP8 does not say anything about stripping indentation from blank lines as far as I can see. It says "avoid trailing whitespace anywhere", but it provides no justification at all for removing indentation from a blank line. This rule is a misinterpretation of PEP8 _which makes code less consistent_. As I have also documented above, this is metadata that ruff is removing.
2. PEP8 is frequently incorrect, anyway.
3. If formatters refuse to provide options to support them, how is one supposed to improve the community standards? Your suggested workaround is for _every python coder on earth_ to work around this misinterpretation of the rules, _every time they edit a file_ after running ruff format on it. _Forever_.

> The project then ends up with inconsistently formatted code.

...not if the ruff settings are in pyproject.toml, and pyproject.toml is part of the project, as is standard practice. 

This same justification, if true, would apply for _any_ setting currently supported by ruff. E.g you might run ruff over a file using tabs, where I might use spaces.

---

_Comment by @MichaReiser on 2024-07-26 08:47_

Whitespace inside blank lines isn't indentation. It's just whitespace [spec](https://docs.python.org/3/reference/lexical_analysis.html#blank-lines). 

That's why any whitespace in a blank line is considered trailing whitespace. 

> PEP8 is frequently incorrect, anyway.

I agree that PEP8 isn't perfect and is why we sometimes deviate, but only if we have good reason and when there's wide community support around it. I don't think this is the case here.

> ...not if the ruff settings are in pyproject.toml, and pyproject.toml is part of the project, as is standard practice.

This isn't about the settings. It's about that `ruff format` would accept both these files if such a new setting is introduced

```python
def a():
	# note that the following line has trailing whitespace
    
    return 5
```

```python
def b():
     # note that the following line has no trailing whitespace

     return 5
```

This does not apply to e.g. using `indent-style=tab` because ruff then converts all space indentations to tabs. 


I'm sorry to disappoint you but this feature doesn't fit into the formatter as it is designed today. 

---

_Comment by @AntiSol on 2024-07-27 09:59_

>Whitespace inside blank lines isn't indentation. It's just whitespace [spec](https://docs.python.org/3/reference/lexical_analysis.html#blank-lines).
>That's why any whitespace in a blank line is considered trailing whitespace.

This is referring to a machine interpreter, not to a human reading the code. It makes sense to generalise a line containing only indentation to a blank line with trailing whitespace for a machine, but to a human reading the code, the indentation is significant metadata which enforcement of this rule destroys.

>I agree that PEP8 isn't perfect and is why we sometimes deviate, but only if we have good reason and when there's wide community support around it. I don't think this is the case here.

The very first section of PEP8 after the introduction is called [A Foolish Consistency is the Hobgoblin of Little Minds](https://peps.python.org/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds). It states:
```sometimes style guide recommendations just aren’t applicable. When in doubt, use your best judgment. Look at other examples and decide what looks best```

You talk again about community support, but didn't address this section of my previous post, so I'll just paste it again: If formatters refuse to provide options to support them, how is one supposed to improve the community standards or gather community support? Your suggested workaround is for _every python coder on earth_ to work around this misinterpretation of the rules, _every time they edit a file_ after running ruff format on it. _Forever_.

> This isn't about the settings. It's about that ruff format would accept both these files if such a new setting is introduced

Aah, I see. But this situation you describe, while not ideal as it does introduce the potential for inconsistency, that inconsistency is still preferable to the removal of this information.

If you wanted to be more consistent about it, the checker could throw a warning if a line contains no indentation in the middle of an indented block when the 'don't apply W293' setting is enabled (this would be my preference). Or the formatter could potentially add indentation in such a case (though knowing the correct level of indentation is not possible 100% of the time (e.g in the case of an empty blank line at the end of a nested if block), but still the case where it gets it wrong sometimes would be still preferable to the removal of this metadata.

You say that you don't want to introduce inconsistency, here's a question: would a file formatted by a ruff that doesn't enforce W293 be more or less consistent than one that uses no formatter at all? 

All I want is for this to be an option in order to make ruff acceptable to use in my projects. This is the only thing that I haven't been able to work around / find a setting for. I would prefer my code to be more consistent, but not at the cost of the loss of this indentation metadata. 

It doesn't have to be the default option. This can be added while not affecting anybody who doesn't enable the option.


---

_Comment by @EnouenJ on 2025-04-12 22:06_

Hello, bringing up this issue again, because I believe it is what is affecting my situation.  My junior is trying to choose a linter to add to our project and ruff seems like a good choice.  Unfortunately, I take rather major issue with the inability to keep these tabs on empty lines due to the fact that it requires tabbing multiple times when later adding a new line to the code.  My junior's perspective on this seems to be the same as @MichaReiser , which is that they will always use the previous line as the starting point to write a new line of code.  However, this unfortunately does not provide a resolution for me and others who will directly start at the place they would like to write the code.  As I have been thinking more about this concern, I have realized that this is heavily correlated with programmers who are using multiple blank lines of code as scaffolding for future developments.  For example, see the code by @AntiSol above on this post (below line 25 with the #TODO statement).  It is my personal perspective, that python coders who do use multiline spaces in this way should also be able to be supported by this linter, but it seems that is not the current goal for ruff or there is an abundance of people who code in the first way.  

To ruff maintainers:
I think if this could receive a second look by another maintainer who does use multiline scaffolding in this way or is more empathetic to this coding style, that would be greatly appreciated.  Code is after all, not a static piece of data, and is designed to be edited my many developers.  If I could get some explanation of why supporting this breaks other parts of ruff or why there is an aversion to this, it would be greatly appreciated.

To other ruff community members:
If I could get some explanation on how to remove this rule, then that would also be greatly appreciated.  Currently, this issue is blocking us from adopting ruff for our project, so if this could be resolved, it would allow us to use ruff in this way.  If there is a need to hack an implementation of ruff, I think that would be fine for our application.  As a reminder, I have not understood the .toml yet since I was not the one incorporating ruff, but based on my understanding of the conversation here, that should not matter because it sounds like this feature is not 'natively' supported by ruff.  I am also unfamiliar with the codebase, so it is not clear to me how to proceed on this.  It seems that the codebase is mainly done in rust, so I am not sure if there is some lack of flexibility in this language which makes it difficult to have a change like this.  I would guess that's not the issue for my case, because I could port my own version of ruff which always has this disabled.  Again, I am not particularly familiar, so any insight on whether this could be easy and how to do it and so on -so that I can make a decision to use this linter or look for another -would be very useful for me.


---

_Comment by @AntiSol on 2025-04-15 09:33_

> this issue is blocking us from adopting ruff for our project

Your best bet is to simply not adopt ruff in your project. It seems they're not interested in supporting our usage (or even continuing discussions / addressing questions raised about it). 

Unfortunately I was not able to find any formatter that does have the option, so my recommendation unfortunately remains: you're better off with no formatter.


> If I could get some explanation on how to remove this rule

`pip uninstall ruff`
Just don't install ruff, and don't allow your junior to use it because it will destroy your significant indentation. Do thorough PR reviews and use those to enforce consistent formatting and style.

If your junior wants a linter then they're welcome to find one that doesn't destroy indentation (let me know if you find one), or they can file a bug against their preferred linter explaining that they're unable to use it because it has been banned on the grounds that it destroys significant whitespace (my junior couldn't be bothered with that and has learned to work without depending on a formatter).



> I am not sure if there is some lack of flexibility in this language which makes it difficult to have a change like this

Yeah, I looked at the rust, too. It seems to be more of an issue with ruff's lexxer/engine, which doesn't really account for whitespace at all: IIUC ruff's formatter is effectively working by parsing the python code and spitting out a version of that code with it's preferred formatting and indentation, i.e ruff it is effectively destroying all whitespace/indentation and inserting its own. That's what makes having this option difficult - there's no way within that MO to support a workflow of the kind you and I have been using for decades.


TLDR: it seems your best bet is, like me, to not use ruff for now, and wait for a day when formatters are less primitive and terrible (not to single out ruff here, it's the whole class of software that is terrible, not necessarily ruff or its authors).

Alternatively if you have a large social media following and/or an abundance of free time you could maybe use that to build "wide community support" for the feature.

Sorry I don't have better news :/

---

_Comment by @EnouenJ on 2025-04-15 20:21_

> Your best bet is to simply not adopt

Hi AntiSol, thanks for an update on what happened in your situation.

> If your junior wants a linter then they're welcome to find one that doesn't destroy...

This is the situation we are also leaning towards.  I am having them look to see if they can find a linter which will fit this requirement, but for now they had decided that just writing PRs which obeys the other rules and not deleting the significant whitespace themselves is the best option.

> ...which doesn't really account for whitespace at all

I see, that indeed sounds like it makes it very difficult to handle.  To their credit, it is might be quite difficult to programmatically handle all white space cases like this regardless.  Moreover, to your point about wide community support, I am not sure how common this style is.  I have been trying to think about it more consciously in the last several days.  I think it is also very possible, based on discussing with my junior, that many programmers are already "locked into" using linters in this way, so then the usefulness of intentional whitespace is something they cannot even discover themselves.  Further, it is a habit which requires forming (which is simultaneously why you and I are unlikely to use ruff in the near future).

Thanks for the update

---

_Comment by @AntiSol on 2025-04-16 08:14_

> > If your junior wants a linter then they're welcome to find one that doesn't destroy...
> 
> This is the situation we are also leaning towards. I am having them look to see if they can find a linter which will fit this requirement, but for now they had decided that just writing PRs which obeys the other rules and not deleting the significant whitespace themselves is the best option.

Yeah, it's unfortunate that the developers of these tools don't really seem to be very interested in flexibility or supporting other ways of working 🤷

On the positive side, several people I've worked with who I've forced to live without these silly tools have come around and now dislike these tools, and have stopped using them on their own projects. In other words I think we're past "peak silly tools" and will see their usage become less prevalent as more and more people encounter intractable issues like this one and realise that these tools aren't fit for purpose.

> > ...which doesn't really account for whitespace at all
> 
> I see, that indeed sounds like it makes it very difficult to handle. To their credit, it is might be quite difficult to programmatically handle all white space cases like this regardless. 

Well, it's not actually that difficult, per se, but it does require some fundamental change to the way they're doing it. For instance their AST parser would need to keep track of whitespace, or they'd have to move to a different approach altogether (which would almost certainly cause issues with other things they're doing)

> Moreover, to your point about wide community support, I am not sure how common this style is. I have been trying to think about it more consciously in the last several days. 

Yeah, that's exactly my point: to get this added to ruff, the style would need to be something people want to widely adopt...

> I think it is also very possible, based on discussing with my junior, that many programmers are already "locked into" using linters in this way, so then the usefulness of intentional whitespace is something they cannot even discover themselves. Further, it is a habit which requires forming (which is simultaneously why you and I are unlikely to use ruff in the near future).

...but it's impossible for the people who might want to adopt it to ever know what this way of working is like while still using a linter, because the linter will destroy this information. And because these kids love their silly tools, they're unlikely to ever actually try that.

In other words, the developers don't want to implement this option because it doesn't have wide community support, but it's not possible to ever get wide community support until the option is implemented.

TLDR: everybody is just better off without ruff and similar tools :)

---

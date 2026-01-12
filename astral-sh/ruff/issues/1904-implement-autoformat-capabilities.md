```yaml
number: 1904
title: Implement autoformat capabilities
type: issue
state: closed
author: charliermarsh
labels:
  - core
  - formatter
assignees: []
created_at: 2023-01-16T03:02:39Z
updated_at: 2024-02-18T19:29:04Z
url: https://github.com/astral-sh/ruff/issues/1904
synced_at: 2026-01-12T15:54:41Z
```

# Implement autoformat capabilities

---

_@charliermarsh_

This issue is the public kickoff for Ruff's autoformatting effort.

A few comments on how I'm thinking about the desired end-state:

- The goal is to enable users to replace Black with Ruff. So, ideally, new projects could come in and replace pyflakes, pycodestyle, isort, and black with a single tool (Ruff).
- As with the rest of Ruff, autoformatting will be entirely optional. You can continue to use Ruff alongside Black. You can also use Ruff _just_ as an autoformatter.
- I'd like to adhere to Black's formatting choices as much as possible, but _exact_ 1:1 parity with Black is not a goal. I'm comfortable deviating when we can make a compelling case, but I'd like that case to be based on arguments that are unrelated to code style itself (e.g., simpler implementation).
- I'd like to enable _slightly_ more configuration than is possible with Black (see: #813), using [Prettier](https://prettier.io/docs/en/options.html) as a model. Prettier allows you to specify the line-length, tab width, quote style, trailing comma behavior, and one or two other stylistic features. I think it's reasonable to support at least line length, indentation, and quote style, so I plan to start there. (It's _not_ a goal to be as flexible as, say, Rustfmt.)

With big projects like this, I work best by taking some time to hack on possible implementations, so I'd like to do some free exploration before opening up the issue to other contributors. But I'd love to hear feedback on the above (and any tips you may have) as I start working on this.


---

_Label `core` added by @charliermarsh on 2023-01-16 03:02_

---

_Comment by @ichard26 on 2023-01-16 03:19_

While I'm not sure how to feel about seeing Black being replaced as one of its maintainers (I'm kinda attached TBH!) I'd be happy to answer any questions you might have about Black. Feel free to ask about its code style and its implementation.

I barely know anything about Ruff, but from what I'm hearing, it's probably going to be the future of code quality tooling. There's no way we (Black) can ever compete on speed or being an all-in-one solution. I'm not going to contribute code or whatever, but I do feel like I have a responsibility to our users to build a better formatter even if that means helping arguably a competitor.

Obviously I do have my own Black-influenced opinions on "good code style" so don't expect me to suggest significant deviations from Black's style. I couldn't do that to my project, that's too much of a betrayal :)

---

_Comment by @charliermarsh on 2023-01-16 03:36_

Thank you for the thoughtful message, and for the kind offer -- I really appreciate it.

To be honest, I was a little hesitant to post this Issue, since I too am a fan and long-time user of Black, and I don't want to send the wrong message by offering an alternative. But, it's just a very natural extension of what Ruff is already doing, and it's something that comes up constantly when talking to users.

(Also: while I _do_ want Ruff to be a viable all-in-one solution, I _also_ want it to be incrementally useable and incrementally adoptable -- so, e.g., even if we were to complete this, my intention is that it'd always be possible to use Black alongside Ruff. Ruff has the same relationship with isort right now.)

I'll keep your offer in mind as I get the ball rolling here :)


---

_Comment by @LefterisJP on 2023-01-16 12:48_

Hey @charliermarsh!

Can I chime in and ask for maybe something different? Black is really good at what it's doing and it's also fast (enough).  What I see as a really good way for ruff to go for auto-formatting would be the way of yapf .... but better. So faster and more customizable. Black exists and we don't really need another black.

What python lacks is a customizable auto-formatter that can be customized by each project to come as close to their preferred style as possile. Only thing close to that is yapf.

[yapf](https://github.com/google/yapf) is an auto-formatter tool for python based on the ideas behind clang format. So the idea is to make it as customizable as possible for projects to apply their rules via autoformat.

The current problems with yapf are:
1. It's not as customizable as it can be.
2. It's super slow for big projects. Takes 4-5 minutes for rotki.
3. I think it's not in a very active maintenance mode

I really believe ruff can shine here and would love to see it become this kind of fast and customizable auto-format tool.

---

_Comment by @warsaw on 2023-01-16 15:52_

Great to see your thoughts here, and I'll watch closely with a hope of contributing if I have the bandwidth, and at least testing things out when you have stuff to try.

Some thoughts: 
* Quote style is probably the number 1 reason why [blue](https://blue.readthedocs.io/en/latest/) even exists.  It's not good enough to just not change quote style; blue deliberately chooses single quotes over double quotes wherever possible, for reasons we think are good, but in keeping with the spirit of this ticket, I won't argue that right now.
* We think blue does a better job of maintaining spacing between code and any right hanging comments, although not a [perfect job](https://github.com/grantjenks/blue/issues/77).
* Blue is really difficult to maintain because it monkeypatches black (to gain from all the really great work that black and its maintainers have done!).  Monkeypatching of course is extremely fragile.  Without proper APIs in black (and I don't fault black for not supporting them), blue will always have a difficult time keeping up.
* Blue has (had?) [better configuration options](https://blue.readthedocs.io/en/latest/#how-do-i-use-blue) than black, but even there [it's problematic](https://github.com/grantjenks/blue/issues/78).  Having to keep up with the internal implementation details of *two* external projects now is pretty daunting.
* I personally don't care about the wide features of YAPF.  For me and my projects (both open source and for $work), black gets us almost all the way there.  I remember sending Łukasz a list of like 10-11 bullet points about black's choices back in the early days, and he refuted every one of them.  Which of course is perfectly okay! 😄 - in fact, I've accepted and/or come around on all of them but the few that I still feel blue makes better choices on.

All that to say that IMO, it's worth having *some* choice on formatting styles, blue tries to make that possible, but maintaining it is difficult, the black developers are doing a fantastic job, and I still would like to see `ruff` provide a good, fast alternative!

---

_Comment by @layday on 2023-01-16 17:07_

@warsaw 

> Quote style is probably the number 1 reason why [blue](https://blue.readthedocs.io/en/latest/) even exists. It's not good enough to just not change quote style; blue deliberately chooses single quotes over double quotes wherever possible, for reasons we think are good, but in keeping with the spirit of this ticket, I won't argue that right now.

Just wanna point out that you can accomplish this right now with `black -S` + `ruff --fix` and the appropriate `[tool.ruff.flake8-quotes]` config - it's what I use :)

(Will collapse this since it's OT.)

---

_Comment by @WhyNotHugo on 2023-01-17 13:16_

There's one thing that `black` won't do and I think `ruff` would be able to address: `E501`. When a line is too long, black will often ignore that. For example:

```py
hello = "This is a really long string that's really over 80 characters wide, and black won't do anything about it"
```

It would be kinda weird if ruff doesn't autoformat this **and** complains that it's over 88 characters long. The solution to the above should be:

```py
hello = (
    "This is a really long string that's really over 80 characters wide, and "
    "black won't do anything about it"
)
```

---

_Comment by @charliermarsh on 2023-01-18 13:38_

@WhyNotHugo - Black can fix that if you use `black --preview` :) Hopefully Ruff can support this too.


---

_Comment by @timabbott on 2023-01-20 21:16_

This project is very exciting for Zulip! To explain some of the practical benefit, one of the main practical limitations the size of a Python file is that Black takes about a 200ms per 1K line of code, which becomes a very noticeable lag when running that as an on-save hook in an editor. If that part was at Ruff speeds, I'd expect there to be essentially no autoformatter related lag when saving files in an editor, which would be a visible improvement to our Python development experience.

So being able to replace Black with Ruff, as we have with most of our other Python linters would be amazing. I support the design decision to support slightly more configurability than Black has -- while I don't recall the details, I remember our migration to Black being delayed for a long time because they didn't have the options to avoid some things that we considered style regressions.

---

_Comment by @charliermarsh on 2023-01-20 21:19_

@timabbott - That's great to hear and I appreciate you chiming in :)

---

_Comment by @charliermarsh on 2023-02-09 23:37_

Brief update: I'm continuing to work on this :)

It's going well! Several tests from the Black suite are passing, more are close-to-passing (I'd like to quantify this soon, but I haven't done string normalization yet, and that causes most tests to fail even if the rest of the formatting is correct).

Still a lot of work and cases left to handle. But my current thinking is that I'll OSS it as soon as I'm confident that it can handle all syntax including comment preservation, even if the style drifts a bit over a few releases as we improve compatibility.


---

_Comment by @ddahan on 2023-02-17 16:10_

I have to say I really love the idea of using a single tool for everything related to code quality. I actually never understood the border between linting and formatting.

On the other side, I think Black is amazing, and I'm not sure Python community needs another new formatter right now, especially if it is very similar to Black. 

So I was wondering, how to resolve this contradiction?

 This may be a naive idea, but could we include black inside Ruff as a dependency? Like every time a new black version is out, Charlie Marsh magic script can convert this black to "Rust Black" and embed it in the next Ruff.

This way, users only install a single tool, but behind the scene, they are using Black, with the exact same behaviour, and exact same config (but with Rust speed !)

I would love this option, but would totally understand if other users prefer something else.  Thanks for the amazing work anyway!

---

_Comment by @ofek on 2023-02-17 16:22_

> but with Rust speed

That won't happen magically without a new implementation in Rust

> On the other side, I think Black is amazing, and I'm not sure Python community needs another new formatter right now, especially if it is very similar to Black. 
> So I was wondering, how to resolve this contradiction?

Whenever this happens I'm assuming it would gradually replace Black in the long term

---

_Comment by @charliermarsh on 2023-02-17 23:26_

Current work-in-progress implementation has been merged into `main` -- now working off `main` as we iterate and increase test completion rate.

If you're interested in following along, https://github.com/charliermarsh/ruff/pull/2883 has some details on how it works and what it supports (and doesn't) thus far.

---

_Comment by @smackesey on 2023-03-11 17:41_

Excited for this, and chiming in here on two major benefits I see coming from this as someone working on a large monorepo ([Dagster](https://github.com/dagster-io/dagster):

- Speed: ruff is way faster than black on our ~2500 python file repo. Very noticeable when running from the command line.
- Config resolution flexibility: there are places where we are forced to duplicate black config because not every package in our codebase uses the same line length settings (we use shorter line length for code snippets that will be rendered to docs). Black doesn't support config inheritance. This also sometimes causes problems for us when running black in pre-commit due to the way black resolves config files.

---

_Comment by @ambv on 2023-03-15 22:51_

I don't think you need my opinion here but since I already started typing, here it goes!

First of all, it doesn't ruffle any feathers in Blackland that this effort here is happening. Ruff is a qualitative improvement over previous developer experience tooling for Python, one that we can't really touch with Python in terms of performance, so I'm really curious where this will go.

In fact, I like the requirements spelled out here. In particular, trying to follow the Black style while acknowledging that a 1:1 implementation is probably not feasible. Charlie, if you identify some differences that you think are clear improvements, we can also consider moving toward those in Black. That way migration from Black to Rufffmt (sorry, I had to 😅) would be more seamless.

I'm saying this because my initial urge to create the auto-formatter didn't come from a need to be the king of the Python formatting hill. Rather, I wanted a tool that formats things consistently with a set of rules that I can explain in prose, and recognize in the output. I contributed to YAPF before and tried to adopt it at Facebook where I worked at the time. This failed because the rich configurability and clever "dissatisfaction optimization" approach of YAPF caused its results to be sometimes hard to understand. No configuration file we tried saved us from very suboptimal edge cases, and when changing the weights solved a particular issue, other issues popped up somewhere else. Therefore, I just wanted a tool that does one thing and does it well.

I guess I'm softly advising against creating a fully configurable tool here. I'm not saying you shouldn't let people configure more things than Black. Our tool was created when YAPF was already a well-known and mature piece of software. The goal was to end bikeshedding and to a large extent we did. Clearly, normalizing string prefixes is still dividing people after 5 years of the tool existing so I have to conclude that going there was a mistake. I still stand behind the decision for Black not to indent in any other way than 4 spaces. It's literally the first concrete rule in PEP 8 and I think sticking to it made it easier for everybody to read and copy-paste Python code wherever it comes from.

Anyway, to re-iterate: if the end result of this effort is that the community will migrate to Rufffmt as a tool, I will be a happy man... as long as it doesn't reignite bikeshedding over formatting minutiae. The tools won't (and can't) be 1:1 interchangeable but as long as the broad strokes are same-ish, all is good. You see, not even Black is 1:1 the same between code style editions (**please run with `--preview`** if you don't mind some additional churn in exchange for getting better style faster). So I have no qualms about Ruffmt being an alternative "edition" of Black.

To be clear, Charlie, Ruff is your tool and it's ultimately your call to do whatever you want with it. I won't lose sleep over any decision. My comment here is mostly meant as encouragement and to extend the possibility of collaboration on the resulting style. I'm happy to see @ichard26 essentially said as much early on here. He's right. Don't be a stranger, we're happy to talk if you ever need anything!

---

_Comment by @zellyn on 2023-04-18 19:14_

I always saw `black` as inspired by `gofmt`: the appeal is in _not_ having configurable options, or as few as possible. (Although it sounds like it might be worth adding some configurable options internally, but not exposed in the commandline arguments or config files, for the `blue` folks to use, so they can stop monkey-patching!)

---

_Comment by @silverwind on 2023-04-18 19:43_

A 2-space indent option would be a requirement for me to use it. prettier also [has it](https://prettier.io/docs/en/options.html#tab-width).

---

_Comment by @thejcannon on 2023-04-19 11:05_

I'll throw in my 2c.

Line length, spaces, and quotes are ok to configure so long as the overall style remains the same as black (especially considering blacks style attempts to minimize diffs).

The important thing is that the Python community is slowly converging on one style, meaning code across organizations, repos, and geographical distance all looks the same. That's a huge win for developers, since it means less brain power parsing code stylistically and more power parsing it semantically.

---

_Comment by @veritas9872 on 2023-04-20 06:33_

Hello! Thank you for the great project. I would like to mention a little bugbear with the Black code style that I have.
Black currently formats functions such that arguments are at the same indentation as the function definition. 
```python
def func(
    arg1,
    ...,
)
```
However, this can be hard to read in many cases and is also different from what the PyCharm auto-formatter does.
Though it may use more space, the following is easier to read, especially when there is no IDE to color the function name.
```python
def func(
        arg1,
        ...,
)
```

I understand that this issue https://github.com/psf/black/issues/1178 in Black caused some very heated exchanges but I think  that having an option (or setting the default) to allow argument indentation would be a good option and be in accordance with PEP8.

---

_Comment by @veritas9872 on 2023-04-20 06:36_

Also, the PyInk project https://github.com/google/pyink has some good ideas for which configurations should be allowed.

---

_Comment by @helderco on 2023-04-20 09:40_

> Also, the PyInk project https://github.com/google/pyink has some good ideas for which configurations should be allowed.

PyInk was created as a way to ease transition for big projects that haven't used black since the beginning, so changing midway would be too disruptive.

My preference is for ruff to follow black's stance on minimal configuration and encouraging conventions. For transition situations than we have these other tools to make it easier.

> Pyink is intended to be an adoption helper, and we wish to remove as many patches as possible in the future.

\- [Why PyInk?](https://github.com/google/pyink#why-pyink)

It goes without saying that there's room for improvements, but I prefer reaching for a convention whenever possible rather than make it configurable and fragmenting styles.

I think it's also valuable to balance readability with **reducing noise in diffs**, for example in https://github.com/psf/black/issues/1811, which is one of the configurations in PyInk.

---

_Comment by @veritas9872 on 2023-04-20 10:04_

@helderco I agree that using PyInk as a benchmark would not be a good idea and that having too many options would be a bad idea. Still, I believe that the extra indentation for function arguments would not cause any extra diffs. It is also compliant with PEP8 and the PyCharm formatter. Perhaps the style should be fixed to use the different indentation scheme? However, I also understand that many people would appreciate the slightly larger extra space left for type hinting in the current Black function argument indentation scheme.

---

_Comment by @ofek on 2023-04-20 14:27_

Configurable indentation is I think something we should not support due to how uncommon it is to see anything other than the style Black supports. Things like single quotes I agree with since there is a sizable population that uses them.

---

_Comment by @nilsbunger on 2023-04-20 16:12_

I want to voice support for indentation config in function argument lists. Different formatters do it differently, and the `black` style is controversial. 

I don't think @ofek is right that it's "uncommon" to see other styles on this point. Formatters before `black` didn't do it that way. And I've worked with several large professional python codebases, none of which used that style. My experience is by no means exhaustive but I think it points to there being significant variability.

In terms of merits, I find it hard to distinguish between arguments and function body when the indentation is the same. I understand others feel differently and that's ok!

---

_Comment by @JimDabell on 2023-04-20 17:44_

Indentation style is often unfairly maligned as bikeshedding, but it’s an accessibility issue. Some people need the indent to be wide so they can see it better. Some people need the indent to be thin because their large font size makes lines go way over to the right otherwise. Eyesight isn’t the same for everybody. This isn’t people being precious about having settings to play with, it’s about avoiding putting barriers to entry in front of people because they don’t have great eyesight.

The solution that solves this problem is tabs for indentation. If you want to avoid configuration, the right solution is to enforce tabs for indentation. But there’s enough established precedent in the Python community that makes this unrealistic, so making it configurable is a decent compromise.

Please don’t build a formatter that people can’t use for accessibility reasons. It’s bad enough that Black shuts down the possibility of using tabs in all the projects that use it, but its maintainer coming here and telling you should do the same just makes the problem worse. There’s no reason everybody can’t get what they want, except for the people who want to stamp out different indentation styles altogether regardless of the accessibility problems. Stamping out different indentation styles is not worth causing problems for people who don’t have great eyesight.

What PEP8 actually says is:

> Spaces are the preferred indentation method.
> 
> Tabs should be used solely to remain consistent with code that is already indented with tabs.

That is not a concrete rule, that allows for reasonable flexibility. There are plenty of codebases out there that already use tabs. Enforcing spaces for indentation with your formatter would mean that they *can’t* do as PEP8 says and “use tabs to remain consistent with code that is already indented with tabs”. The attitude of “if you use tabs you should switch to spaces” is going against what PEP8 says. Also, PEP8 is not a religion – if I have to choose between causing accessibility problems and ignoring PEP8, I’m going to ignore PEP8 every time. Ruff is great, so I’d rather not have that prevent me from using it for formatting.

Yes, I 100% agree that configuration options should be kept to a minimum, and yes, I know everybody has different ideas about what that minimum should be, so if you don’t keep a tight grip on things it can spiral out of control. I’m completely sympathetic to the argument that configuration should be minimised and the default answer should be “no”. But that doesn’t mean being completely inflexible altogether in the face of real problems it causes. Please don’t listen to the people making this out to be bikeshedding. Other formatting preferences don’t cause accessibility problems, this one does.

This has been discussed in greater detail in Black [#2798](https://github.com/psf/black/issues/2798), where an accessibility-based organisation makes a good argument for it. If you’re going to participate in the discussion about this, I would recommend reading their comments first.


---

_Comment by @veritas9872 on 2023-04-21 01:33_

From what I could gather from the conversation in https://github.com/psf/black/issues/1178, Black continues to use its current indentation for arguments as changing it would be too disruptive, even though it diverges from PEP8. Perhaps setting the indentation differently is feasible in a new project as perfect parity with Black is not a goal. However, there may be many good reasons to keep the Black behavior.

---

_Comment by @WhyNotHugo on 2023-04-25 18:02_

I'd love to see auto-wrapping of text in docstrings.

I don't really care if it's 80 characters or 88. Using the already-in-use maximum width should be fine.

---

_Comment by @provinzkraut on 2023-04-25 18:17_

> I'd love to see auto-wrapping of text in docstrings.

That won't be quite that easy. No current tool I know of does it correctly and they all tend to break things from time to time (think references, tables, custom markdown syntax elements). For markdown in particular this is an issue since it does not have a formal specification, which means there's on one clear way to determine where it's okay to break a line.

If you're trying to do this "the right way", you'll inadvertently end up implementing a basic parser for these languages. I vaguely remember talks about extending ruff to support "Python adjacent" formats (e.g. markdown and ReST) as well, so maybe the docstring things would fit in there?

---

_Comment by @Avasam on 2023-06-10 21:31_

My 2c:
My main gripe with Black is how it forcibly unwraps method parameters (both for method calls and definitions), method chaining, comprehensions (list, sets, generators), ternary conditions, etc. when I put them on multiple lines for readability reasons to begin with. Even worse when a couple back to back methods are right under/over the line limit as it makes the code look inconsistent .

It also ironically goes against the idea of reducing git changes and conflicts when a list/param count is suddenly just short enough.
(It's fine for stubs where the definition is mainly interacted with through IDE/intellisense/type-checking anyway.)

Hence why I'm still using autopep8, even if I dislike the 2-blank-lines spacing. Similar reasons why I'm using pdrint instead of prettier for javascript/TypeScript. 

So the one configuration option I'd really be looking for is: `Don't unwrap short lines`

Also referencing this here since it's more formatting than linting: https://github.com/astral-sh/ruff/issues/3713
Edit: Some examples: https://github.com/jsh9/cercis/discussions/18#discussioncomment-6224080

---

_Comment by @jsh9 on 2023-06-19 19:30_

Hi @charliermarsh : here are my unsolicited two cents:

There are 2 main pain points to `Black`, which are completely orthogonal:
1. Speed (`Black` is native Python which is naturally slower)
2. Lack of configurability (which resulted in many heated and controversial conversations)

When it comes to the Ruff auto-formatter, it may be worth making a decision early on about which `Black` pain point you'd like to address.  The 1st one, the 2nd one, or both?

I personally do not believe in `Black`'s philosophy that "lack of configurability ends bike-shedding", which is why I created [my own fork of `Black`](https://github.com/jsh9/cercis) and implemented many configurable options for myself and other people.

I'm not the only one who forked `Black`.  There are quite a few of them out there (such as [pyink](https://github.com/google/pyink), [black-ex](https://github.com/mabeyj/black-ex), [tan](https://github.com/jleclanche/tan), [cblack](https://github.com/pausan/cblack)).

Therefore, I would strongly recommend the Ruff formatter to provide configurability as well -- not just double/single quotes and indentation styles, but potentially more.  Otherwise, people will either raise strong-worded issues again, or fork again, or simply not use it.

---

_Comment by @warsaw on 2023-06-22 16:02_

> There are quite a few of them out there (such as [pyink](https://github.com/google/pyink), [black-ex](https://github.com/mabeyj/black-ex), [tan](https://github.com/jleclanche/tan), [cblack](https://github.com/pausan/cblack)).

Don't forget [blue](https://blue.readthedocs.io/en/latest/).

---

_Comment by @jsh9 on 2023-06-23 05:59_

> > There are quite a few of them out there (such as [pyink](https://github.com/google/pyink), [black-ex](https://github.com/mabeyj/black-ex), [tan](https://github.com/jleclanche/tan), [cblack](https://github.com/pausan/cblack)).
> 
> Don't forget [blue](https://blue.readthedocs.io/en/latest/).

`Blue` technically is not a fork, which is why I didn't mention it in my comment.  But yes, `Blue` deserves a separate comment.

---

_Comment by @zmievsa on 2023-06-23 13:54_

We can first implement a zero-configuration option and then decide on whether we'd like to add extensive configuration, right? 

---

_Comment by @warsaw on 2023-06-23 16:03_

> We can first implement a zero-configuration option and then decide on whether we'd like to add extensive configuration, right?

For sure, and I do like the narrow configurability of `black` despite that I disagree with some of its choices.  I'd be careful not to try to be as configurable as say [YAPF](https://pypi.org/project/yapf/) which I've personally had a difficult time to wrangle into the style I *really* want.

TBH, if `black` just let me configure the default quoting style, I would be happy.  This is the main reason why `blue` was even created in the first place.

> Blue technically is not a fork, which is why I didn't mention it in my comment.

Fair enough and thanks!  Blue really is a monkeypatch wrapper around black.

---

_Comment by @toppk on 2023-08-06 21:24_

My one pet peeve with current solutions is that I seem to struggle adding `# noqa`  or `# type`  on long lines. the reformat comes, and moves the noqa/type on the wrong line, and moving it to the right new line, can cause another reformat and misplacement.

---

_Comment by @agronholm on 2023-08-07 06:51_

> My one pet peeve with current solutions is that I seem to struggle adding `# noqa` or `# type` on long lines. the reformat comes, and moves the noqa/type on the wrong line, and moving it to the right new line, can cause another reformat and misplacement.

Ditto

---

_Comment by @barraponto on 2023-08-09 13:31_

> My 2c: My main gripe with Black is how it forcibly unwraps method parameters (both for method calls and definitions), method chaining, comprehensions (list, sets, generators), ternary conditions, etc. when I put them on multiple lines for readability reasons to begin with.

If you put a comma at the end of the parameters list, it stays in multiple lines and never unwraps. See https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#the-magic-trailing-comma

---

_Comment by @zanieb on 2023-08-09 15:19_

> My one pet peeve with current solutions is that I seem to struggle adding `# noqa` or `# type` on long lines. the reformat comes, and moves the noqa/type on the wrong line, and moving it to the right new line, can cause another reformat and misplacement.

Thanks for sharing! Getting comment placement right is a high priority for us and this is an area where we might deviate from Black — it's also a very difficult problem for formatter implementations though so we can't make guarantees.

---

_Comment by @warsaw on 2023-08-09 16:09_

> Getting comment placement right is a high priority for us and this is an area where we might deviate from Black

Blue does it differently than Black, especially for hanging comments.  It doesn't always get it right, but I think it does a better job.

---

_Comment by @charliermarsh on 2023-09-12 17:17_

I'm excited to announce the **Alpha release of the Ruff formatter**, out now in Ruff v0.0.289.

The formatter is designed to be a drop-in replacement for Black, with an excessive focus on performance and direct integration with Ruff. At present, it achieves over > 99.9% compatibility on significant Black-formatted projects like Django and Zulip.

For the Alpha, our focus is on collecting feedback that we can address prior to a Beta release later this year.

The Alpha is primarily intended for experimentation -- once we release the Beta, we'll start recommending the formatter for production use. Of course, you're welcome to use it in production today (we already use it for our own projects), but the CLI, configuration options, and code formatting may change arbitrarily between the Alpha and the Beta.

For more on the formatter's design goals, and instructions on using it from the CLI or VS Code, check out the [README](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md).

**We'd love for you to try it out!** Notice any [undocumented deviations](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md#intentional-deviations) from Black? Capabilities that would prevent you from migrating? Let us know in the [feedback discussion](https://github.com/astral-sh/ruff/discussions/7310).

---

_Label `formatter` added by @MichaReiser on 2023-09-12 17:21_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-12 17:21_

---

_Comment by @magistau on 2023-09-12 17:55_

1. Black is known to violate PEP 8 sometimes, so "do whatever Black does" might not be the best option
2. Compatibility with [WPS](https://wemake-python-styleguide.readthedocs.io/) would be nice to have; maybe without auto-formatting for everything WPS dislikes but at least autoformatting a WPS accepted file shouldn't introduce any WPS violations, just like autopep8

---

_Comment by @warsaw on 2023-09-12 18:06_

Fantastic news!  I look forward to playing with it.

> Quote style (single vs. double).

Please!  This is the number one reason why [blue](https://blue.readthedocs.io/en/latest/) exists.  Is there a ticket/branch open for this yet?  It might be something I'd try to find some bandwidth to work on.

From the `blue` documentation:

> blue preserves the whitespace before the hash mark for right hanging comments.

This is another thing that I think `blue` does better than `black` (at least, the last time I tried it).

> blue supports multiple config files: pyproject.toml, setup.cfg, tox.ini, and .blue

Another important feature of `blue`.  Happy to just use `pyproject.toml` and `ruff.toml`.

---

_Comment by @zmievsa on 2023-09-12 18:13_

@thecaralice 
1. This entire discussion was in one way or another centered on this exact issue. I am very sure that Charlie has already taken this into consideration and made a judgment call.
2.  I am a huge fan of Nikita's work and an even bigger fan of his flake8/mypy plugins. However, trying to work well with WPS is a scary thing for a code formatter. First, WPS is a regular flake8 plugin, whose rules can (and for the most part have, as far as I know) be added into ruff separately. So I see no need for ruff-fmt to be checked against WPS, at least in the early stages of development. Second, WPS has a [very detailed section](https://wemake-python-styleguide.readthedocs.io/en/latest/pages/usage/integrations/auto-formatters.html) in its docs for formatters:
![image](https://github.com/astral-sh/ruff/assets/17561586/3294e0ed-17e2-421f-afa2-8edcd3aaa186)

In essence, WPS is already not compatible with isort, black, and yapf. It was **never** intended to be compatible. So trying to force compatibility here, in ruff, seems ineffective to me.


---

_Comment by @zanieb on 2023-09-12 18:27_

Regarding quotes, we have not exposed configuration options yet as we're deciding the best way to do so, but we already have an implementation that supports both single and double quotes: https://github.com/astral-sh/ruff/blob/179128dc54f67229d306735c3f29346ae4cb1f79/crates/ruff_python_formatter/src/expression/string.rs#L151
https://github.com/astral-sh/ruff/blob/179128dc54f67229d306735c3f29346ae4cb1f79/crates/ruff_python_formatter/src/options.rs#L33-L34 

---

_Comment by @agronholm on 2023-09-12 18:34_

Is it possible to use this via `pre-commit` yet?

---

_Comment by @MichaReiser on 2023-09-12 18:37_

> Is it possible to use this via `pre-commit` yet?

No, not yet. I created an issue so that we have pre commit support figured out for the beta/stable https://github.com/astral-sh/ruff/issues/7312

---

_Comment by @mkniewallner on 2023-09-12 19:12_

> > Is it possible to use this via `pre-commit` yet?
> 
> No, not yet. I created an issue so that we have pre commit support figured out for the beta/stable #7312

In the meantime, you can use this workaround to run `ruff format` in `pre-commit` without having to rely on a system hook:
```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: "v0.0.289"
    hooks:
      - id: ruff
        args: ["--exit-non-zero-on-fix"]
      - id: ruff
        name: Ruff format
        entry: ruff format
```
The downside is that this makes it impossible to invoke only `Ruff format` without Ruff the linter, so running `pre-commit run ruff -a` will run both. But as a temporary workaround solution, this might be ok.

---

_Comment by @zanieb on 2023-09-12 19:21_

See https://github.com/astral-sh/ruff-pre-commit/pull/50 for formatter support in pre-commit. Thanks for raising :)

---

_Comment by @jsh9 on 2023-09-12 19:25_

Can Ruff provide a toggle to enable 8-space indentation at function definition?

This is what I mean:

Instead of this style (`Black`'s default), which is not very readable in my humble opinion:

```
def some_function(
    arg1: int,
    arg2: bool,
    arg3: float,
    arg4_with_long_name: str,
) -> bool:
    ...
```

could you provide a toggle, so that users who dislike the style above can do this:

```
def some_function(
        arg1: int,
        arg2: bool,
        arg3: float,
        arg4_with_long_name: str,
) -> bool:
    ...
```

The 8-space style is also encouraged by PEP: https://peps.python.org/pep-0008/#indentation

`Black` does not offer this, not because it is technically difficult.  In fact it's easy to implement that.

---

_Comment by @jsh9 on 2023-09-12 19:32_

And thank you @charliermarsh @zanieb @MichaReiser @konstin  as well as the other authors of Ruff, for your willingness to add configuration options from the beginning, and more importantly for your open-mindedness in listening to the users and soliciting feedback!

---

_Comment by @zanieb on 2023-09-12 19:35_

For those who haven't been following our development closely, I want to note that @MichaReiser and @konstin are the lead developers for the formatter and have been doing astounding work. Charlie and I have had a small role in comparison
:)

---

_Comment by @warsaw on 2023-09-13 23:30_

> Regarding quotes, we have not exposed configuration options yet as we're deciding the best way to do so, but we already have an implementation that supports both single and double quotes

@zanieb - are there any hacks available to test this out with single quotes instead of double quotes (short of hacking the source I guess)?

---

_Comment by @ofek on 2023-09-14 01:19_

I am as interested in this as Barry is for single quotes!

---

_Comment by @MichaReiser on 2023-09-14 06:35_

> @zanieb - are there any hacks available to test this out with single quotes instead of double quotes (short of hacking the source I guess)?

Unfortunatunately not. You need to change the code manually. The relevant lines are

https://github.com/astral-sh/ruff/blob/e561f5783b30eb318c0fa1f7d7d24ad8d89c6e05/crates/ruff_cli/src/commands/format.rs#L84-L86

and change it to 

```rust
let options = PyFormatOptions::from_source_type(source_type)
                        .with_line_width(LineWidth::from(NonZeroU16::from(line_length)))
                        .with_preview(preview)
                        .with_quote_style(QuoteStyle::Single);
```

We plan to add configuration support soon

---

_Comment by @euan-reid on 2023-09-17 07:52_

Just going to chip in that, while it's entirely appropriate to support configuration of quotes and line length, it feels wrong that running ruff with all defaults against code copied from docs.python.org causes stylistic changes. Maybe it's a broader change to `Q000` (and possibly `E501` too), but favouring black's opinions over the stdlib docs, REPL output, and near-decade-old flake8-quotes re: quotes and PEP8 re: line length feels like the wrong way around. Maybe a `format.style = black` config option and/or `--style=black` CLI toggle would be the right way to enable matching black's double quote/88 rules, leaving the unadorned defaults as single quote/79?

---

_Comment by @zmievsa on 2023-09-17 08:31_

> favouring black's opinions over the stdlib docs, REPL output, and near-decade-old flake8-quotes re: quotes and PEP8 re: line length feels like the wrong way around

I feel like favoring the most popular style out there is fine. The default style is not the most popular, it's only the default from the docs. Besides ruff would produce errors for a lot of stdlib docs already simply because stdlib and its docs are not entirely consistent on following any rules at all :)

---

_Comment by @euan-reid on 2023-09-17 10:45_

> I feel like favoring the most popular style out there is fine. The default style is not the most popular, it's only the default from the docs.

Is Black demonstrably the most popular style? It's used in many newer projects, certainly, but lots of long-standing projects continue with the same autopep8 they've used for longer than black has existed - let's not confuse recent popularity with overall popularity. On the topic of quotes one need only look at the number of issues filed with black regarding them, both from individual preference and "we cannot as an org migrate to black until this is supported as an option", and particularly after it switched from its original preference of single quotes to double quotes to see that it's not a beloved choice.

> ruff would produce errors for a lot of stdlib docs already simply because stdlib and its docs are not entirely consistent on following any rules at all :)

I'd contest that assertion. While they're not always Pythonic, it's usually for a good and specific reason to explain something more clearly. They do usually lack type hints and other newer language features, granted, but by and large the stdlib docs _are_ very consistent in style, especially when it comes to complying with PEP8.

(edit: removed misremembered specific example of unpythonicness)

---

_Comment by @zmievsa on 2023-09-17 12:02_

> docs are very consistent in style, especially when it comes to complying with PEP8

Remember that things as basic as variable/function naming are not consistent: look unittest, logging, os, email, and many others.  Here's another example from the module I randomly clicked (mailbox):
![image](https://github.com/astral-sh/ruff/assets/17561586/98f91074-822a-42a7-9d47-1bc509dd916f)
![image](https://github.com/astral-sh/ruff/assets/17561586/90765fbc-f0be-4390-bae5-ab55e1d78732)

And there are hundreds of such cases where pep8 rules are broken for non-readability reasons. Besides, [black is consistent with PEP8](https://github.com/psf/black/issues/1252#issuecomment-580685882).

---

_Comment by @tjkuson on 2023-09-17 13:01_

> Just going to chip in that, while it's entirely appropriate to support configuration of quotes and line length, it feels wrong that running ruff with all defaults against code copied from docs.python.org causes stylistic changes.
> ...
> While they're not always Pythonic, it's usually for a good and specific reason to explain something more clearly.

The Python documentation code style is likely a good choice for its purpose: to document and educate. Code formatted for production has different priorities to documentation (such as minimising diffs, and ensuring consistency even at the expense of edge cases).

Thus, I don't think the Python documentation formatting should be used as the ideal for Python production code; instead, we have PEP8 (which both Black and Ruff follow).

---

_Comment by @euan-reid on 2023-09-17 15:37_

> example from the module I randomly clicked (mailbox)

Maybe it's my slightly sleep deprived brain this morning, but I'm not seeing where the naming is inconsistent in that image... >_>

Regardless, I definitely don't want to take us bikeshedding, so I'll simply say thank you for the robust discussion and hopefully it's in some way useful to folks implementing :) Will collapse this comment as it's treading slightly off-topic.

edit: formatting

---

_Comment by @warsaw on 2023-09-17 19:04_

> Is Black demonstrably the most popular style?

I don't know if that really matters.  I talk to lots of people who, like me, dislike some of black's choices.  I also talk to lots of people who don't care because use of black just short circuits most style discussions.  I wish blue was easier to maintain because I heard mostly positive things about its choices where it deviates from black. 

That all said, I predict a very quick adoption of ruff's autoformatting rules just because it's so dang much faster than everything else.

I'm not going to get into whether black supports PEP 8, or the consistency of the stdlib. 😜 

Also, ruff really needs to [support](https://github.com/astral-sh/ruff/issues/1567#issuecomment-1583077835) isort's [length-sort-straight](https://pycqa.github.io/isort/docs/configuration/options.html#length-sort-straight) option for it to be more stdlib friendly.

---

_Comment by @ambv on 2023-09-17 21:33_

> Is Black demonstrably the most popular style? It's used in many newer projects, certainly, but lots of long-standing projects continue with the same autopep8 they've used for longer than black has existed - let's not confuse recent popularity with overall popularity. 

I'll just say this: you can't argue with data.

If you look at PyPI download statistics, Black is downloaded over 950,000 times per day on average. That's **three times more** than YAPF (170,000 downloads/day) and autopep8 (125,000 downloads/day) combined.

Blue is a little over 1,000 downloads a day, pyink a bit above 1,000 per week. Barely blips on the map, and not for a lack of trying. They clearly demonstrate that the loud minority of people complaining about the style choices made by Black is outnumbered by the silent majority of people happy with those choices. Outnumbered 950:1! Just think about those numbers.

Then the only real competition is manual code formatting, in other words, projects that didn't adopt any auto-formatting tool at this point. Hard to talk numbers here, but there sure are many of them! However, I don't think Ruff's auto-formatting should focus on those right now because they weren't happy with any of the available formatters, be it Black forks or entirely different configurable formatters, so it is rather unlikely they would be happy with whatever choices Ruff made. Even among Black forks you'll note that they don't agree on why they forked the tool. You'll have 2-space formatters, tab-based formatters, single quotes, default line length-changing, docstring avoiding, etc. etc.

Given all this information, it makes perfect sense for Ruff to target Black with a possible delta in immaterial differences in formatting, improvements in established deficiencies in edge cases, as well as any configurability the Ruff maintainers might want that Black decides not to incorporate. Sounds like a great attack plan to me, and conceptually in line with what Ruff did with copying flake8 on the linter end.

---

_Comment by @jsh9 on 2023-09-17 21:56_

> They clearly demonstrate that the loud minority of people complaining about the style choices made by Black is outnumbered by the silent majority of people happy with those choices.

A logical flaw here: we can't equate _people using Black_ with _people being happy with Black_.

People may use a tool just because there are no better choice out there at the moment, not because they are happy with it.

> Even among Black forks you'll note that they don't agree on why they forked the tool. You'll have 2-space formatters, tab-based formatters, single quotes, default line length-changing, docstring avoiding, etc. etc.

I agree with this.  But **I think the diversity of people's opinions is something we should value and celebrate, rather than dismiss and shut down**.

I've gone through many style complaints/suggestions on Black's issue board, and some of the reasons that they need a particular style is very legit -- for example, (1) visually-challenged people wanting to use tabs, (2) 8-space indentation in function definition hurting readability, (3) using single quote.

So, if a particular style preference is within reason and not very difficult to maintain or implement, why not provide people such flexibility?

---

_Comment by @bbkane on 2023-09-18 02:37_

I value style consistency and ecosystem consolidation (like black IntelliJ/VS Code Integration) more than diversity of opinions about style, especially with the `fmt: off` escape hatch available when it's (rarely) needed. 

Given `black`'s download numbers, I bet a lot of other folks value consistency above all as well.

---

_Comment by @warsaw on 2023-09-18 03:11_

I have a hunch that if you really dug into those numbers, you'd find the thing most people value most about it is not arguing about style!  And yet here we are. 🤣 

---

_Comment by @KotlinIsland on 2023-09-18 04:14_

Just adding to the discourse:
https://github.com/dizballanze/gray
https://github.com/odwyersoftware/brunette
https://github.com/google/pyink

---

_Comment by @jsh9 on 2023-09-18 09:21_

I also forked Black because I wanted flexibility on some options.  https://github.com/jsh9/cercis

For example, my fork offers:
- Single quote vs double quote
- Ability to fine-tune indentation at different locations
- Line length config
- Tab vs space
- ...

Black's author said above that

> Even among Black forks you'll note that they don't agree on why they forked the tool. You'll have 2-space formatters, tab-based formatters, single quotes, default line length-changing, docstring avoiding, etc. etc.

as if that is a bad thing.  But that's totally fine, isn't it?  As long as the tool offers people sufficient flexibility, people can use a slightly different flavor in different projects.  And my fork is a good demonstration of that.

Only allowing one opinion (in the name of preventing bikeshedding) can have the following negative impact:

1. A fragmented community with discussions filled with emotionally charged wording
2. Multiple forks just to tweak a few things
3. And all the decreased productivity because of No. 1 and No. 2

---

_Comment by @ambv on 2023-09-18 09:51_

What's important for the Ruff maintainers to understand is that once a configuration toggle is made available, you can't remove it later. Black didn't cave to pressure to enable configuration toggles, because its entire premise is to remove debates about code style from day-to-day work within software development teams. I get numerous thanks from people over email, social media, and in person, who say the tool delivered on that for them.

But the bikeshedding never really goes away. It just moves to a different place. Before Black it happened during code review, or at the YAPF configuration file. After Black it moved to Black's issue tracker. Now it's this issue. I'm sure we'll hear from every Black fork author here, eventually 🫠

---

_Comment by @fnep on 2023-09-18 11:54_

Are you planning to improve the reporting for usage with the `--check` switch?

The current `x file would be reformatted, x files left unchanged` with no extra info what it is, is a bit too short for our ci pipeline reporting.

---

_Comment by @disrupted on 2023-09-18 11:58_

> Are you planning to improve the reporting for usage with the `--check` switch?
> 
> The current `x file would be reformatted, x files left unchanged` with no extra info what it is, is a bit too short for our ci pipeline reporting.

it would be ideal to generate a diff of the changes

---

_Comment by @MichaReiser on 2023-09-18 12:00_

> Are you planning to improve the reporting for usage with the `--check` switch?
> 
> The current `x file would be reformatted, x files left unchanged` with no extra info what it is, is a bit too short for our ci pipeline reporting.

We do. We don't know yet if we add a new `--diff` option to similar to black or rely on `--check` with a `--format` that shows diffs. https://github.com/astral-sh/ruff/issues/7231

---

_Comment by @charliermarsh on 2023-09-22 18:21_

For those that are interested in testing out quote or indentation styles, [v0.0.291](https://github.com/astral-sh/ruff/releases/tag/v0.0.291) exposes those configuration options along with a few others (see the [docs](https://docs.astral.sh/ruff/settings/#format)).

For example:

```toml
[tool.ruff.format]
# Use single quotes rather than double quotes.
quote-style = "single"
# Indent with tabs rather than spaces.
indent-style = "tab"
```


---

_Comment by @jni on 2023-09-25 06:21_

@ambv 

> I'll just say this: you can't argue with data.

The dominance of black and user satisfaction with all of black's choices are two different issues. I used Apple computers with butterfly keyboards for like 6 years because I like/am addicted to macOS, but also those keyboards were *garbage*. My use of that laptop does not imply I liked *all* of that laptop's choices. So, here's some "arguing with data":

(1) black has benefited from a privileged position by being folded into the PSF umbrella, which helps it in (2).
(2) in discussions about which formatter to use, one key factor is the longevity and maintainability of the formatter. This leads to a rich-get-richer effect ("no one gets fired for choosing IBM"), which leads to an effective monopoly, which renders the relative number of downloads useless for deciphering user preference.
(3) finally, as you mention, there is a huge mass of projects that don't use a formatter at all. Why not? Have you run any kind of user research to understand why? Maybe if black had some more configuration options, more projects would use it? 🤷 

tldr, black's dominance does *not* imply that a new formatter needs to adhere to black's design choices. Indeed it's a wasted opportunity to bring more projects into the auto-formatting fold — which I think is a good fold.

---

_Comment by @jni on 2023-09-25 06:23_

(At any rate: thanks to everyone working on the formatter landscape! I can see how hard it is. 😂 But I'd love it to be a proper landscape with lots of diversity! 😊🙏)

---

_Comment by @jsh9 on 2023-09-25 06:50_

Yeah, there are quite a few logical problems in that thought process to use data to "tell a story."

To add to @jni's comment above, here's another place with a logical problem:

> If you look at PyPI download statistics, Black is downloaded over 950,000 times per day on average. That's three times more than YAPF (170,000 downloads/day) and autopep8 (125,000 downloads/day) combined.

These 3 numbers alone don't tell us anything, because _**it's not a controlled experiment**_.

What is a controlled experiment?  It is one where the only thing different is the formatter itself, and all other factors are kept the same (including the users' psychological perception -- hence the need for the "placibo group" in pharmaceutical experiments).

So many things are different in this Black vs Yapf vs autopep8 "experiment".  To name a few:

- Black's repo sits under [Python Software Foundation](https://github.com/psf), giving people the illusion that it's an official Python package
- Yapf claims that it's not an official Google product and it's still in beta phase
- Black claims in its README that it's used in many popular projects, but Yapf doesn't (which probably has something to do with Google's brand image considerations)
- The [network effect](https://en.wikipedia.org/wiki/Network_effect) makes laypersons (who don't know the nuances of Python code formats or formatters) just gravitate towards the repo with the most stars

All these is to say: conclusions drawn from non-strictly-controlled experiments can oftentimes be misleading, overestimated, underestimated, or even in the wrong direction.

I'm not against drawing conclusions from data, but _**the interpretation of data is something that requires scientific rigor and critical thinking**_.  So I would personally be very very careful when I make any claims/suggestions/conslusions that start with the sentence "you can't argue with data."

---

_Comment by @smackesey on 2023-09-25 14:16_

Adding to the peanut gallery: the entire idea that it's a _feature_ that `black` lacks config options is misguided. It's also inconsistent with `ruff`, which has literally hundreds of knobs to turn-- which, from this "no flexibility is a good thing" perspective, is a dangerous bikeshedding minefield! And yet `ruff` is great and people seem happy with it. Why haven't software projects everywhere slowed to a crawl over arguments about import sorting style or whether to enable lint rule X?

It's because the real time-suck has never been decisions over formatting style. It is _enforcing_ a given format style. However, this problem is independent of how configurable a formatting tool is. As long as the tool runs well in CI and is integrated into dev workflows, no "manual' formatting enforcement need occur.

I think that black is a good tool, but its rise also coincided with widespread adoption of tools for in-editor formatting enforcement and use of a formatter check in CI. Aside from these factors, its success comes from simple messaging, good marketing, and polished implementation. I suspect the success has almost nothing to do with the fact that it has few config options, even though the tool claims this as its philosophy.

Indeed IMO black would be better tool with more config options, so I think that's the direction ruff should lean. By all means ruff should limit flexibility in the name of performance or keeping implementation bug-free and low-maintenance, but the "let's limit options so teams never even have the chance to argue about it" argument is silly.


---

_Comment by @ofek on 2023-09-25 14:27_

I enjoy meta-commentary just as much as the next person but please starting right now let's keep this specifically about asking for certain configuration options as well as the maintainers discussing the merits of those options and timelines for implementations.

I would simply unsubscribe but this functionality is incredibly important to me so I want to stay up to date on happenings related to it.

Thanks 🙂 

---

_Comment by @charliermarsh on 2023-09-25 17:28_

Thanks everyone. I've been reading all of these and I appreciate the input.

I will chime in to confirm that our goal for the initial release is to implement Black-compatible formatting with a limited set of [known deviations](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md#intentional-deviations) and a couple of configuration options that don't exist in Black (e.g., indentation style, quote style, line ending). That is, we don't plan to make the formatter any more configurable in the initial release.

But I'll also confirm that whether we expose additional configuration options in future releases is an unsettled question.

Speaking for myself and not for the project (to the degree that that's possible...), my biggest concern around configuration / flexibility is the increased maintenance and testing burden. Implementing a formatter already requires handling an enormous combination of expression, statement, and comment placements. Each additional configuration option adds another degree of freedom that we need to maintain and test in perpetuity. So even if we do decide to make the formatter more configurable, there will likely always be some tradeoffs to make in balancing flexibility with performance, maintenance burden, etc.

(Separately, again speaking for myself -- this time as a user -- I find myself really liking the configuration options exposed by [`rustfmt`](https://github.com/rust-lang/rustfmt/blob/master/Configurations.md) :))


---

_Comment by @jni on 2023-09-26 00:26_

Thanks for listening @charliermarsh! 😊 Would you suggest we make new issues as feature requests for specific configuration options? Then each config option can be evaluated independently.

---

_Comment by @LeonarddeR on 2023-09-27 20:03_

I'm really exited to see support for tabs has landed.
Now when talking about configuration, the only thing that that disappoints me about Black and conflicts with Pep8 is the missing extra level of indentation for function parameters,  e.g. Black does:
```
def foo(
    bar
):
```
instead of expected by pep8:
```
def foo(
        bar
):
```

---

_Comment by @ofek on 2023-09-27 20:15_

Black does not force extra indentation like that, I'm not sure what you mean

---

_Comment by @jsh9 on 2023-09-27 20:15_

> I'm really exited to see support for tabs has landed. Now when talking about configuration, the only thing that Black doesn't do that disappoints me and conflicts with Pep8 is the extra level of indentation for function parameters, e.g.:
> 
> ```
> def foo(
>     bar
> ):
> ```
> 
> instead of
> 
> ```
> def foo(
>         bar
> ):
> ```

Hey @leonardder, welcome to the club (of wanting this 8-space indentation feature)!  That is my main motivation of forking Black.  You can check out my fork here: https://github.com/jsh9/cercis

I do sincerely hope that the Ruff Formatter will support this, or at least make it configurable.

---

_Comment by @jsh9 on 2023-09-27 20:17_

> Black does not force extra indentation like that, I'm not sure what you mean

@ofek , I think @leonardder meant the opposite: Black uses 4-space indentation at function definition, rather than 8 spaces.  And a bunch of people would much prefer 8-space indentation (only at function definition, not in other places).

---

_Comment by @ofek on 2023-09-27 20:18_

Oh I misinterpreted, I thought they were saying that Black erroneously adds extra indentation rather than them saying they wish it did so 😅 

I personally very much dislike that, but to each their own.

---

_Comment by @akx on 2023-09-27 20:18_

I also dislike the 8-space indent - why is it suddenly 8 there where it's 4 everywhere else? Anyway, yeah, to each their own. 😄 

---

_Comment by @LeonarddeR on 2023-09-27 20:23_

@ofek I'm sorry for the confusion, I've edited my comment above to clarify what I meant.
@jsh9 thanks for the warm welcome. In fact I don't care much myself, but I'm a contributor of a project that follows PEP8 in this particular case, and I think we'd prefer to stick with that.

---

_Comment by @jsh9 on 2023-09-27 20:24_

> I also dislike the 8-space indent - why is it suddenly 8 there where it's 4 everywhere else? Anyway, yeah, to each their own. 😄

The reason I prefer 8-space indentation at function definition is the following:

If the indentation is only 4 spaces, the argument list aligns completely with the function name, making it hard for me to visually distinguish them.  Here's an example:

```
def my_function(
    my_function_arg_1,
    my_function_arg_2,
    my_function_arg_3,
    my_function_arg_4,
):
    pass
```

Some people may argue that there are syntax highlighting and function names have a different color.  But many times I need to look at plain-text logs without any syntax highlighting.

I do understand other people's desire for consistency (4-space indentation everywhere).  That's OK; people can have different preferences and priorities.

The once-and-for-all solution, in my humble opinion, is to just add a configurable nob for this, rather than forcing either camp to the other camp. 

---

_Comment by @warsaw on 2023-09-27 23:37_

I don't much like the 8 space indents for function definitions either, but there aren't a lot of good options.  Maybe use Guido's time machine and change `def` to `define`? 😛 

One thing [python-mode](https://gitlab.com/python-mode-devs/python-mode) does is support "hanging" arguments like so:

```python
def my_function(arg_1,
                arg_2,
                arg_3,
                ):
```

but only if the open paren is not the last non-whitespace character on the line.

---

_Comment by @ofek on 2023-09-27 23:41_

That particular code example makes me want to not have any configuration at all 😆 

---

_Comment by @jsh9 on 2023-09-28 00:06_

> I don't much like the 8 space indents for function definitions either, but there aren't a lot of good options. Maybe use Guido's time machine and change `def` to `define`? 😛

Yeah, `func` or `function` would be much better.
> One thing [python-mode](https://gitlab.com/python-mode-devs/python-mode) does is support "hanging" arguments like so:
> 
> ```python
> def my_function(arg_1,
>                 arg_2,
>                 arg_3,
>                 ):
> ```
> 
> but only if the open paren is not the last non-whitespace character on the line.

This "hanging "style can lead to maintenanbility problems in the future.  For example, if in the future people want to rename `my_function` into something such as `func1` (especially with the auto refactoring provided by IDEs), the code then becomes:

```python
def func1(arg_1,
                arg_2,
                arg_3,
                ):
```

It looks very ugly.

---

_Comment by @Samreay on 2023-09-28 00:12_

I'm not sure how many people here are subscribed like me, with the intention of hearing updates from the developers about the status the implementation work, rather than this much larger conversation about possible formatter customisations.

Is there any chance that sort of conversation could be moved into the dedicated GitHub discussion [for formatter feedback](https://github.com/astral-sh/ruff/discussions/7310) instead of the implementation issue?

Obviously not a dev for the project, so unsure what works best for them - I'm only coming from this as a "I can no longer see the messages from the devs because there are ten thousand emails about config in the way"

---

_Comment by @charliermarsh on 2023-09-28 00:18_

Thanks @Samreay -- that's a good suggestion, and sorry that I haven't done a better job of steering discussion out of the issue. (I don't mind reading through the comments personally, but it's easy for me to forget that there are others subscribed here for updates :))

**Humble request to put any future feedback in the [formatter discussion](https://github.com/astral-sh/ruff/discussions/7310) (be it feedback on configuration, current code style, future code style, etc.), and we'll keep this issue focused on announcements and project updates.**


---

_Comment by @zanieb on 2023-10-24 17:36_

We're excited to [announce the formatter is in Beta](https://astral.sh/blog/the-ruff-formatter) — you can see more details in the [changelog](https://github.com/astral-sh/ruff/blob/main/CHANGELOG.md#012).

---

_Comment by @Samreay on 2023-10-25 01:21_

@zanieb amazing! Congrats on the release! Is the [pre-commit](https://github.com/astral-sh/ruff-pre-commit) repo going to be getting a sweet 0.1.2 release as well, I'd love to just stick ruff *everywhere* now!

---

_Comment by @JayOfferdahl on 2023-10-25 01:24_

Switched over my organization last week -- it was a breeze. Thanks for all the work here!

---

_Comment by @zanieb on 2023-10-25 03:07_

@Samreay the pre-commit repo is automatically tagged on each release of Ruff, so you don't need to wait for GitHub releases to be published in order to use the latest version. I've added the corresponding GitHub releases anyway though.

---

_Comment by @charliermarsh on 2023-10-26 23:55_

Now that the formatter is out in Beta, I'm calling this complete :)

---

_Closed by @charliermarsh on 2023-10-26 23:55_

---

_Comment by @alexisthual on 2023-11-11 23:16_

> @WhyNotHugo - Black can fix that if you use `black --preview` :) Hopefully Ruff can support this too.

Out of curiosity, is it possible to split long strings with ruff already? :blush: 
I could not find it in the documentation.

---

_Comment by @charliermarsh on 2023-11-11 23:31_

@alexisthual -- Not yet, but we plan to support it.

---

_Comment by @giampaolo on 2024-02-18 19:18_

> Out of curiosity, is it possible to split long strings with ruff already?

> Not yet, but we plan to support it.

For the record, this is the only reason which made me go back from `ruff format` to `black` after a while. Use case: my editor (Sublime Text) executes both black and ruff on save ("fix" operation, not "check"). The ability to automatically split long lines on the fly, while I type code, instead of later via CLI, is very very valuable.

I'm not sure if this specific black option (split long lines) has been introduced recently, but I achieve it via this conf:

```toml
[tool.black]
preview = true
# https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html
enable-unstable-feature = ["string_processing"]
```

Black advertises this as an "unstable feature", so I understand it may be premature to add it into ruff at this time. I assume though (but it's just a mere assumption) that black devs' intention is to make this the default once they deem the feature reasonably stable.

---

_Comment by @giampaolo on 2024-02-18 19:24_

> I assume though (but it's just a mere assumption) that black devs' intention is to make this the default once they deem the feature reasonably stable.


It turns out my assumption was wrong, see related black ticket: https://github.com/psf/black/issues/4208. :)

---

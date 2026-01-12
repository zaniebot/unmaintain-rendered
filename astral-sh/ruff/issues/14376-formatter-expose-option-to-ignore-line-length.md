```yaml
number: 14376
title: "Formatter: expose option to ignore line length"
type: issue
state: closed
author: gvfarns
labels:
  - formatter
assignees: []
created_at: 2024-11-16T05:01:54Z
updated_at: 2024-11-16T21:49:04Z
url: https://github.com/astral-sh/ruff/issues/14376
synced_at: 2026-01-12T15:54:53Z
```

# Formatter: expose option to ignore line length

---

_@gvfarns_

The ruff formatter's decision to mimic black's defaults is a good one, but the decision to be as unwilling to allow any configuration as black is, I think, a mistake. We need at least a parameter to allow the formatter to ignore line length.

No matter how smart black or ruff formatters are, they will never be smart enough for line length decisions, in my opinion. They aggressively break lines up once they hit the specified line-length, and they aggressively join lines that are better split if they are under line-length. This is in contrast to autopep8, which will allow you to ignore line length decisions, but still implements other formatting properly.

One can have ruff (the linter) ignore line length issues by having it ignore E501, but the formatter does not obey this directive and is unwilling to consider anything but dogmatic line-length implementation. Providing a long line-length parameter results in dogmatically long lines, rather than just leaving alone the user's line-length decisions.

Black is not the ultimate in usability. It would be much better if there was a parameter to ignore line length decisions, as there is in the linter.

---

_Label `formatter` added by @MichaReiser on 2024-11-16 16:41_

---

_Comment by @MichaReiser on 2024-11-16 16:41_

Could you give me an example on how you would expect the formatter to format your code? Are you looking for a formatter that only normalizes e.g. the spaces between expressions but never changes line breaks? 

Maybe the pycodestyle E2XX rules are a better fit for what you're looking for? I don't think all of them have fixes, but some do.

---

_Comment by @gvfarns on 2024-11-16 19:44_

There are a variety of things formatters do: fixing spaces, indenting, replacing single with double quotes, managing comment placement, deleting or adding blank lines, ordering imports, etc. And then some formatters, such as this one and black, will also split lines that are longer than line-length, or will undo the manual splitting the author has done in order to force code onto a single line. Optimally, there would be an option to enable or disable each action, just as there are options to enable, warn, or disable linting errors. It seems pathological that I can have `ignore = ["E501"]` in my ruff configuration file and `ruff format` still undoes my careful decisions about which lines to split and which to let go long.

Among others, autopep8 has enough options to disable and enable the formatting things one wants to change. I imagine pycodestyle works as well, but I have no experience with it. I have been using autopep8 for some time and it works as desired, but I would like to convert to ruff as a single solution for linting, fixing, and formatting (and someday, static type checking, but that's a different issue). I see no reason autopep8 can do anything ruff can't.

More generally, I would like it if ruff did not embrace the ethos black is oddly proud of: "we know better than you lowly programmers, so we don't need to give you configuration options."

---

_Comment by @MichaReiser on 2024-11-16 21:24_

Thanks for the additional context. It helped me understand where you're coming from.

Ruff does have some lint rules, like isort and the `E2XX` rules that do what you describe. Not all of them are autofixable, but you can toggle individual rules. 

> More generally, I would like it if ruff did not embrace the ethos black is oddly proud of: "we know better than you lowly programmers, so we don't need to give you configuration options."

It's definitely not our opinion that we know better and I'm fairly certain that this isn't the black maintainer's opinion. The reason why the formatter only supports few options isn't because **we know better** or because it's the "the perfect code style". The goal is to have a consistent formatting, that can be applied automatically and is widely accepted by the community to end the debates about code style. [Prettier's philosophy](https://prettier.io/docs/en/option-philosophy) page explains it well in my view. 

Making the formatter as configurable as you're asking would mean that we remove the value proposition that many users find the most valuable in black's style guide: not having to discuss code formatting. That's why it's unlikely that we'll add many new options to the formatter (besides that there are also technical reasons). 

We understand that black's style guide isn't to everyone's taste. That's why we have some lint rules to enforce consistent formatting (as mentioned above) that you can use instead of the formatter. 


---

_Closed by @MichaReiser on 2024-11-16 21:24_

---

_Comment by @gvfarns on 2024-11-16 21:49_

Thanks for the additional info. There are many formatters, and I think it's a mistake for ruff to hone in on black as the one to emulate. Projects who don't want to argue about formatting can just agree to use the default configuration and get all the benefit of black. Everything you and prettier said could be said of the lint rules as well. Why allow users to configure anything? If linters and fixers just force them all to do it one way, then there are no more arguments. I can think of no reason the linter should allow configuration and the formatter shouldn't.  But you are closer to the project vision than I am.

Appreciate you pointing out the E2xx rules. I'll take a look and see if there are good formatting rules in the linter that are not enabled by default. Could be I just didn't do enough research on this stuff. Thanks for all your hard work!

---

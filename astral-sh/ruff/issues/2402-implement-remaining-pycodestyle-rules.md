---
number: 2402
title: "Implement remaining `pycodestyle` rules"
type: issue
state: open
author: charliermarsh
labels:
  - plugin
assignees: []
created_at: 2023-01-31T17:28:09Z
updated_at: 2025-12-31T10:33:17Z
url: https://github.com/astral-sh/ruff/issues/2402
synced_at: 2026-01-10T01:22:40Z
---

# Implement remaining `pycodestyle` rules

---

_Issue opened by @charliermarsh on 2023-01-31 17:28_

Note: some of the checked-off rules are still gated behind the `logical_lines` feature flag. To see the list of rules enabled in the current release, refer to the [docs](https://github.com/charliermarsh/ruff#error-e).

## E1  Indentation
- [x] `E101` ("indentation contains mixed spaces and tabs")
- [x] `E111` ("indentation is not a multiple of four")
- [x] `E112` ("expected an indented block")
- [x] `E113` ("unexpected indentation")
- [x] `E114` ("indentation is not a multiple of four (comment)")
- [x] `E115` ("expected an indented block (comment)")
- [x] `E116` ("unexpected indentation (comment)")
- [x] `E117` ("over-indented")
- [ ] `E122` ("continuation line missing indentation or outdented")
- [ ] `E124` ("closing bracket does not match visual indentation")
- [ ] `E125` ("continuation line with same indent as next logical line")
- [ ] `E127` ("continuation line over-indented for visual indent")
- [ ] `E128` ("continuation line under-indented for visual indent")
- [ ] `E129` ("visually indented line with same indent as next logical line")
- [ ] `E131` ("continuation line unaligned for hanging indent")

## E2  Whitespace
- [x] `E201` ("whitespace after ‘(’")
- [x] `E202` ("whitespace before ‘)’")
- [x] `E203` ("whitespace before ‘,’, ‘;’, or ‘:’")
- [x] `E211` ("whitespace before ‘(’")
- [x] `E221` ("multiple spaces before operator")
- [x] `E222` ("multiple spaces after operator")
- [x] `E223` ("tab before operator")
- [x] `E224` ("tab after operator")
- [x] `E225` ("missing whitespace around operator")
- [x] `E227` ("missing whitespace around bitwise or shift operator")
- [x] `E228` ("missing whitespace around modulo operator")
- [x] `E231` ("missing whitespace after ‘,’, ‘;’, or ‘:’")
- [x] `E251` ("unexpected spaces around keyword / parameter equals")
- [x] `E261` ("at least two spaces before inline comment")
- [x] `E262` ("inline comment should start with ‘# ‘")
- [x] `E265` ("block comment should start with ‘# ‘")
- [x] `E266` ("too many leading ‘#’ for block comment")
- [x] `E271` ("multiple spaces after keyword")
- [x] `E272` ("multiple spaces before keyword")
- [x] `E273` ("tab after keyword")
- [x] `E274` ("tab before keyword")
- [x] `E275` ("missing whitespace after keyword")

## E3  Blank line
- [x] `E301` ("expected 1 blank line, found 0")
- [x] `E302` ("expected 2 blank lines, found 0")
- [x] `E303` ("too many blank lines (3)")
- [x] `E304` ("blank lines found after function decorator")
- [x] `E305` ("expected 2 blank lines after end of function or class")
- [x] `E306` ("expected 1 blank line before a nested definition")


## E4  Import
- [x] `E401` ("multiple imports on one line")
- [x] `E402` ("module level import not at top of file")

## E5  Line length
- [x] `E501` ("line too long (82 > 79 characters)")
- [x] `E502` ("the backslash is redundant between brackets")

## E7  Statement
- [x] `E701` ("multiple statements on one line (colon)")
- [x] `E702` ("multiple statements on one line (semicolon)")
- [x] `E703` ("statement ends with a semicolon")
- [ ] `E704` ("multiple statements on one line (def)")
- [x] `E711` ("comparison to None should be ‘if cond is None:’")
- [x] `E712` ("comparison to True should be ‘if cond is True:’ or ‘if cond:’")
- [x] `E713` ("test for membership should be ‘not in’")
- [x] `E714` ("test for object identity should be ‘is not’")
- [x] `E721` ("do not compare types, use ‘isinstance()’")
- [x] `E722` ("do not use bare except, specify exception instead")
- [x] `E731` ("do not assign a lambda expression, use a def")
- [x] `E741` ("do not use variables named ‘l’, ‘O’, or ‘I’")
- [x] `E742` ("do not define classes named ‘l’, ‘O’, or ‘I’")
- [x] `E743` ("do not define functions named ‘l’, ‘O’, or ‘I’")

## E9  Runtime
- [x] `E901` ("SyntaxError or IndentationError")
- [x] `E902` ("IOError")

## W1  Indentation warning
- [x] `W191` ("indentation contains tabs")

## W2  Whitespace warning
- [x] `W291` ("trailing whitespace")
- [x] `W292` ("no newline at end of file")
- [x] `W293` ("blank line contains whitespace")

## W3  Blank line warning
- [x] `W391` ("blank line at end of file")

## W5. Line break warning
- [x] `W505` ("doc line too long (82 > 79 characters)")

## W6  Deprecation warning
- [x] `W605` ("invalid escape sequence ‘x’")


---

_Label `plugin` added by @charliermarsh on 2023-01-31 17:28_

---

_Referenced in [astral-sh/ruff#1073](../../astral-sh/ruff/issues/1073.md) on 2023-01-31 17:28_

---

_Comment by @charliermarsh on 2023-01-31 17:35_

I'm happy to support these, but I likely won't enable them by default -- I'd like them to be opt-in.

---

_Comment by @charliermarsh on 2023-01-31 17:35_

(Also that tabulation is incomplete, but I'll finish it up later!)

---

_Comment by @charliermarsh on 2023-01-31 17:35_

\cc @saadmk11 

---

_Referenced in [scipy/scipy#17892](../../scipy/scipy/pulls/17892.md) on 2023-01-31 18:03_

---

_Referenced in [astral-sh/ruff#2407](../../astral-sh/ruff/issues/2407.md) on 2023-01-31 19:15_

---

_Comment by @charliermarsh on 2023-01-31 22:09_

I've updated this lint to include all the current `pydocstyle` rules, less those that are ignored in the default configuration and those that deal with pre-Python 3.7 code.

---

_Comment by @charliermarsh on 2023-01-31 23:59_

First we need #1130.

---

_Comment by @charliermarsh on 2023-02-02 04:30_

So far I have seven more rules done in #1130.

---

_Comment by @pierrepo on 2023-02-02 19:41_

Thanks for this @charliermarsh 
I'm using a lot `pycodestyle` and `pydocstyle` with my students to teach them PEP8 and PEP257. Using `ruff` will be a great plus.

---

_Comment by @charliermarsh on 2023-02-05 00:55_

#1130 is up to 14 rules. Just trying to knock out a few each days.

---

_Referenced in [astral-sh/ruff#2653](../../astral-sh/ruff/pulls/2653.md) on 2023-02-08 03:24_

---

_Referenced in [astral-sh/ruff#2654](../../astral-sh/ruff/pulls/2654.md) on 2023-02-08 04:41_

---

_Comment by @charliermarsh on 2023-02-08 04:42_

Knocked out eight more rules behind the feature flag.

---

_Referenced in [astral-sh/ruff#2680](../../astral-sh/ruff/pulls/2680.md) on 2023-02-09 03:28_

---

_Comment by @charliermarsh on 2023-02-09 03:57_

Four more done.

---

_Referenced in [scipy/scipy#17874](../../scipy/scipy/pulls/17874.md) on 2023-02-12 22:34_

---

_Referenced in [astral-sh/ruff#3008](../../astral-sh/ruff/issues/3008.md) on 2023-02-18 18:01_

---

_Comment by @charliermarsh on 2023-02-26 21:33_

One more in #3225.

---

_Comment by @charliermarsh on 2023-02-27 15:36_

One more in https://github.com/charliermarsh/ruff/pull/3249.

---

_Comment by @charliermarsh on 2023-03-05 20:05_

One more in https://github.com/charliermarsh/ruff/pull/3344.

---

_Referenced in [mne-tools/mne-python#11534](../../mne-tools/mne-python/issues/11534.md) on 2023-03-07 13:11_

---

_Comment by @kapilt on 2023-03-14 09:52_

rust newb, if someone wants to try this feature set, how do we build ruff with logical-line feature flag support and these style rules enabled?

[update]
this seems to do it, following along on contributing.md

```shell
git clone https://github.com/charliermarsh/ruff.git
cd ruff
cargo build --all-features --release
cp target/release/ruff ~/bin
```


---

_Comment by @spaceone on 2023-03-14 10:40_

> rust newb, if someone wants to try this feature set, how do we build ruff with logical-line feature flag support and these style rules enabled?

@charliermarsh I would like to have a pip release for ruff with that special feature as pip/setup.py "package option" / "extra"/ "extras_require" enabled. So that you can call `pip install ruff[logical-line]` and have this enabled. Maybe it's possible to achieve that?


---

_Comment by @charliermarsh on 2023-03-14 19:12_

I'm hoping that we can finish and release this soon enough that it's not worth shipping an `extra_requires` variant. (And if not, I'd probably rather ship with a command-line or feature flag rather than an `extras`, so that it's available regardless of how you install Ruff.)


---

_Referenced in [astral-sh/ruff#3576](../../astral-sh/ruff/issues/3576.md) on 2023-03-17 21:33_

---

_Referenced in [astral-sh/ruff#3720](../../astral-sh/ruff/issues/3720.md) on 2023-03-24 19:07_

---

_Referenced in [vega/altair#3008](../../vega/altair/pulls/3008.md) on 2023-04-01 19:01_

---

_Referenced in [wagtail/wagtail#10324](../../wagtail/wagtail/pulls/10324.md) on 2023-04-08 15:48_

---

_Comment by @jamesbraza on 2023-04-27 04:52_

This conquest is awesome!  I see many of these as being checked (e.g. `E2` whitespace is done).

Just a friendly request that these get eventually added to the Rules in docs: https://beta.ruff.rs/docs/rules/#error-e

---

_Comment by @calumy on 2023-04-27 11:51_

> Just a friendly request that these get eventually added to the Rules in docs: https://beta.ruff.rs/docs/rules/#error-e

I believe this will be added when #3689 lands, as documenting a rule before it has been enabled will potentially cause some confusion.

---

_Comment by @hoel-bagard on 2023-05-24 08:07_

I'll take a look at E303.

EDIT: I'm doing all of `E3 Blank line` since they are closely related.

---

_Referenced in [astral-sh/ruff#4635](../../astral-sh/ruff/pulls/4635.md) on 2023-05-24 15:16_

---

_Referenced in [cockpit-project/bots#4799](../../cockpit-project/bots/pulls/4799.md) on 2023-05-24 16:03_

---

_Comment by @devbis on 2023-05-25 06:54_

E241 and E242 is missing in the list for pycodestyle.

E241  | multiple spaces after ','
E242 | tab after ','


Can you please add it to the checks?



---

_Comment by @charliermarsh on 2023-05-25 13:30_

I believe those are intentionally omitted -- we're not planning to implement the rules that are omitted from pycodestyle's default configuration ("not unanimously accepted, and PEP 8 does not enforce them"), at least not to start.


---

_Referenced in [astral-sh/ruff#4666](../../astral-sh/ruff/issues/4666.md) on 2023-05-26 09:35_

---

_Referenced in [astral-sh/ruff#4694](../../astral-sh/ruff/pulls/4694.md) on 2023-05-28 17:08_

---

_Referenced in [pymor/pymor#2069](../../pymor/pymor/issues/2069.md) on 2023-06-16 12:13_

---

_Comment by @hoel-bagard on 2023-06-17 07:53_

I'm looking into `E122` (and I'll follow with the other `E1` rules after unless someones is working on them).

---

_Comment by @MartinRoth on 2023-07-06 07:45_

Hello,

I just stumbled about a peculiar behaviour of E714. I had fixed the lint but forgot to remove the first not, i.e. I changed the first if statement to the second below and ruff (I am using ruff version v0.0.275 and v.0.0.277) didn't complain anymore.

```
a = 8

if not a is None:
    print(a)

if not a is not None:
    print(a)
```

I think the lint should also check for the second version, or am I wrong?

---

_Referenced in [astral-sh/ruff#5564](../../astral-sh/ruff/issues/5564.md) on 2023-07-06 15:42_

---

_Referenced in [Unidata/MetPy#3115](../../Unidata/MetPy/pulls/3115.md) on 2023-07-15 19:32_

---

_Referenced in [astral-sh/ruff#6094](../../astral-sh/ruff/pulls/6094.md) on 2023-07-26 12:55_

---

_Comment by @spaceone on 2023-08-03 13:44_

> I'm hoping that we can finish and release this soon enough that it's not worth shipping an `extra_requires` variant. (And if not, I'd probably rather ship with a command-line or feature flag rather than an `extras`, so that it's available regardless of how you install Ruff.)

I think this will take a lot of extra time right? Maybe it's possible to add that command line flag already?

---

_Referenced in [Cray-HPE/libCSM#47](../../Cray-HPE/libCSM/pulls/47.md) on 2023-08-14 19:27_

---

_Referenced in [jorenham/mainpy#2](../../jorenham/mainpy/pulls/2.md) on 2023-08-26 19:57_

---

_Referenced in [astral-sh/ruff#7388](../../astral-sh/ruff/issues/7388.md) on 2023-09-14 11:23_

---

_Referenced in [astral-sh/ruff#7414](../../astral-sh/ruff/issues/7414.md) on 2023-09-15 18:44_

---

_Comment by @ksunden on 2023-09-20 00:10_

Gentle voice of support for E122 and E302 (the latter being included in #4694 which has been in draft for a while.) (ping @hoel-bagard, as both author of that PR and as the one who said you were looking into E122)

These are the last two rules from Matplotlib's flake8 config. Once available, I'd look into swapping over CI/pre-commit hooks to ruff (and then perhaps look into adding more checks, though for now have gone with "parity")

We do not use `black`, so these are checks which fall into the category of "black would just handle them".

---

_Comment by @hoel-bagard on 2023-09-20 00:35_

@ksunden Sorry for taking a long time, for E302, I have started to see if I could find a way to handle comments with the tools currently present in ruff (otherwise a new tool/way of tracking tokens will need to be implemented as suggested in the PR). I was a bit busy recently, but I should be able to spend more time on it in the near future.

For E122, I've just edited my comment saying I was looking into it. A while ago I asked a question on the discord, but did not get an answer (and then I went back to working on the E302 rule and forgot about it). I could restart working on it assuming that having a token emitted for the backslash is fine, in which case it shouldn't take too much time.

> I'm looking into adding the E122 rule of pycodestyle (https://www.flake8rules.com/rules/E122.html). It's a rule that checks for indentation within continuation lines.


> Implementing it using logical lines seemed natural (and that's what pycodestyle does). It seems like it would work for continuation lines made using parentheses since a NonLogicalNewline is emitted for the new line.
> However, when using a backslash (case covered by pycodestyle), no NonLogicalNewline token is created, rendering it impossible to know if there is a newline within the logical line.
> 
> Could a NonLogicalNewline be emitted when there is a backslash, or does the rule need to be implemented using physical lines ?
> 
> Example with parentheses:
> ```python
> print("E122", (
> "dent"))
> ```
> 
> Example using a backslash:
> ```python
> if some_very_very_very_long_variable_name or var \
> or another_very_long_variable_name:
>     raise Exception()
> ```

---

_Referenced in [astral-sh/ruff#7655](../../astral-sh/ruff/pulls/7655.md) on 2023-09-25 13:31_

---

_Comment by @shikari08 on 2023-10-26 15:41_

Is there an update on when E3 Blank line will get released? 

---

_Comment by @hoel-bagard on 2023-10-28 11:12_

I've been doing the `E122` rule. I'm at a point where it's working, but I need to clean things up ~~and add the autofix~~ and the config options. I'll go back to the E3 rule once that's done.

Edit:
- I've opened [a PR for `E122`](https://github.com/astral-sh/ruff/pull/8557) (without the autofix).
- I've opened [a PR for the `E3` rules](https://github.com/astral-sh/ruff/pull/8720) and their autofix.

---

_Referenced in [astral-sh/ruff#8557](../../astral-sh/ruff/pulls/8557.md) on 2023-11-08 13:09_

---

_Referenced in [astral-sh/ruff#8720](../../astral-sh/ruff/pulls/8720.md) on 2023-11-16 13:54_

---

_Referenced in [PyLops/pylops#485](../../PyLops/pylops/issues/485.md) on 2023-11-26 06:14_

---

_Comment by @Avasam on 2023-12-14 07:34_

`W391` is checked but I don't see the rule in Ruff

---

_Referenced in [Joinee-IM/backend#77](../../Joinee-IM/backend/pulls/77.md) on 2023-12-15 05:38_

---

_Referenced in [commaai/openpilot#30792](../../commaai/openpilot/pulls/30792.md) on 2023-12-19 00:47_

---

_Referenced in [astral-sh/ruff#9266](../../astral-sh/ruff/pulls/9266.md) on 2023-12-24 13:33_

---

_Referenced in [astral-sh/ruff#9544](../../astral-sh/ruff/issues/9544.md) on 2024-01-16 07:53_

---

_Referenced in [sphinx-doc/sphinx#11902](../../sphinx-doc/sphinx/pulls/11902.md) on 2024-01-21 20:28_

---

_Referenced in [canonical/operator#1114](../../canonical/operator/pulls/1114.md) on 2024-01-26 03:50_

---

_Referenced in [canonical/operator#1120](../../canonical/operator/pulls/1120.md) on 2024-02-02 07:28_

---

_Referenced in [astral-sh/ruff#9811](../../astral-sh/ruff/issues/9811.md) on 2024-02-05 00:33_

---

_Referenced in [odoo/odoo#151483](../../odoo/odoo/pulls/151483.md) on 2024-02-07 08:52_

---

_Comment by @hoel-bagard on 2024-02-18 10:05_

@charliermarsh Since #9266 got merged, the `E3` rules can be checked here.

---

_Comment by @spaceone on 2024-02-18 13:18_

Are meanwhile all above checked rules now available, at least behind the [preview](https://docs.astral.sh/ruff/preview/#enabling-preview-mode) flag?

---

_Comment by @charliermarsh on 2024-02-18 14:17_

@spaceone - Yup, they're all behind `--preview`.

---

_Referenced in [slack-samples/bolt-python-starter-template#32](../../slack-samples/bolt-python-starter-template/issues/32.md) on 2024-02-23 22:53_

---

_Comment by @buhtz on 2024-02-28 15:41_

How to handle problems with `--preview` error codes? Report them here or open an extra issue?

E303 (too-many-blank-lines) not working on this file for example:

```
# foo.py
class Foo:
    def __init__(self):
        pass


    def two(self):
        pass
```

Calling ruff 0.2.2 via `ruff foo.py --preview`.

---

_Comment by @hoel-bagard on 2024-02-28 23:25_

@buhtz Have you selected the E303 rule ? For example with:

```console
ruff foo.py --preview --select E3
```


---

_Comment by @buhtz on 2024-02-29 08:45_

> @buhtz Have you selected the E303 rule ? For example with:
> 
> ```
> ruff foo.py --preview --select E3
> ```

No, I have not. As [discussed here](https://github.com/astral-sh/ruff/discussions/10153#discussioncomment-8622602) and how I understand [the docs](https://docs.astral.sh/ruff/preview/#enabling-preview-mode) the `--preview` flag to select all "preview" (unstable, new) rules.

**EDIT**: IMHO the docs are misleading.

```
$ ruff foo.py --preview --select E303
foo.py:7:5: E303 [*] Too many blank lines (2)
  |
7 |     def two(self):
  |     ^^^ E303
8 |         pass
  |
  = help: Remove extraneous blank line(s)

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @baco on 2024-02-29 12:32_

> > @buhtz Have you selected the E303 rule ? For example with:
> > ```
> > ruff foo.py --preview --select E3
> > ```
> 
> No, I have not. As [discussed here](https://github.com/astral-sh/ruff/discussions/10153#discussioncomment-8622602) and how I understand [the docs](https://docs.astral.sh/ruff/preview/#enabling-preview-mode) the `--preview` flag to select all "preview" (unstable, new) rules.

Well... that's the issue. As for now, all the `E3` rules have just arrived and are on their testing period, hence, the only way to try them is with the `--preview` flag.

You can see that here, in the [Rules](https://docs.astral.sh/ruff/rules/), and see that all the rules available under the `--preview` are marked with 🧪 (a test tube emoji). And the rules you are looking for:
![image](https://github.com/astral-sh/ruff/assets/102958/652ed358-19ac-4b7e-84e2-d54f43de5779)
all have the test tube emoji

---

_Comment by @buhtz on 2024-02-29 12:50_

> the only way to try them is with the `--preview` flag.

No, you are wrong. That flag alone does not help. You have to combine `--preview` with `--select`. That is the missing key fact here. I know you know it. But some readers might misunderstand that. Wording is very important here.

---

_Comment by @baco on 2024-02-29 13:01_

Ahhh, ok, ok, that's right, the combination of both flags. `--preview` doesn't turn on all the “unstable rules”. The flag rather stops filtering out rules matching the “selection rules” (if that's a thing) that happen to be unstable.

Is that what you are pointing out here?

---

_Comment by @buhtz on 2024-02-29 13:13_

> Is that what you are pointing out here?

Exactly.



---

_Referenced in [astral-sh/ruff#10243](../../astral-sh/ruff/pulls/10243.md) on 2024-03-06 05:35_

---

_Referenced in [astral-sh/ruff#10292](../../astral-sh/ruff/pulls/10292.md) on 2024-03-08 04:03_

---

_Referenced in [sphinx-doc/sphinx#12163](../../sphinx-doc/sphinx/pulls/12163.md) on 2024-03-21 17:22_

---

_Comment by @kaddkaka on 2024-03-23 09:13_

> Note: some of the checked-off rules are still gated behind the `logical_lines` feature flag. To see the list of rules enabled in the current release, refer to the [docs](https://github.com/charliermarsh/ruff#error-e).

Is this still true? Is there a better way than cross referencing https://docs.astral.sh/ruff/rules/#pycodestyle-e-w with the list in this issue?

Where can I find info about "logical_lines"? A search in the docs gave 0 matches.

---

_Comment by @MichaReiser on 2024-03-25 08:21_

> Note: some of the checked-off rules are still gated behind the logical_lines feature flag. To see the list of rules enabled in the current release, refer to the [docs](https://github.com/charliermarsh/ruff#error-e).

No, that's no longer the case. You can opt in by using `--preview` and enabling the desired rules (e.g. `--extend-select=E3`) 

All checked rules in this issue are implemented as stable or preview rules and the rules use the same codes as `pycodestyle`. So there should be no need to cross-reference except if you need to know if the rule is preview only.

---

_Referenced in [astral-sh/ruff#10677](../../astral-sh/ruff/issues/10677.md) on 2024-03-30 23:25_

---

_Comment by @shikari08 on 2024-04-11 20:42_

Anyone know how to use the preview E3 Blank line rules in vscode with the ruff extension. I have the preview version installed in vscode. 

---

_Comment by @dhruvmanila on 2024-04-12 03:10_

@Daku24 By preview, we mean https://docs.astral.sh/ruff/preview/, you don't need to have the _preview_ version of ruff-vscode extension. If you have a local config for Ruff, you can enable preview and include `E3`:

```toml
[tool.ruff.lint]
preview = true
extend-select = ["E3"]
```

If you don't have a Ruff config file or can't create or edit it, you can use [`lint.args`](https://github.com/astral-sh/ruff-vscode#settings).

I hope this helps :)

---

_Referenced in [astral-sh/ruff#11038](../../astral-sh/ruff/issues/11038.md) on 2024-04-19 13:50_

---

_Referenced in [astral-sh/ruff#11349](../../astral-sh/ruff/pulls/11349.md) on 2024-05-09 01:54_

---

_Referenced in [astral-sh/ruff#12140](../../astral-sh/ruff/pulls/12140.md) on 2024-07-02 11:29_

---

_Comment by @Alex-ley-scrub on 2024-07-02 22:22_

This is awesome work 👏 

Once E11, E12, and E13 rules are added with automatic fixes, if we run something like this:
```
ruff check foo.py --fix --preview --select E1
```
would we would get the ~same result as something like this?:
```
autopep8 foo.py --in-place [--aggressive] 
```
If so, that would be amazing! autopep8 is really good at fixing broken indentation and adding this to ruff would be 😍 

E1 rules don't have automatic fixes at the minute - is that just a case of priority/time or is there a technical challenge / performance concern / reluctance for this to be default behaviour etc. as somewhat discussed in https://github.com/astral-sh/ruff/pull/1130

---

_Referenced in [astral-sh/ruff#13585](../../astral-sh/ruff/pulls/13585.md) on 2024-10-01 10:17_

---

_Referenced in [astral-sh/ruff#13897](../../astral-sh/ruff/issues/13897.md) on 2024-10-23 17:18_

---

_Referenced in [jonescompneurolab/hnn-core#934](../../jonescompneurolab/hnn-core/pulls/934.md) on 2024-11-08 21:57_

---

_Referenced in [astral-sh/ruff#14651](../../astral-sh/ruff/issues/14651.md) on 2024-11-28 06:32_

---

_Referenced in [astral-sh/ruff#14673](../../astral-sh/ruff/issues/14673.md) on 2024-11-29 21:40_

---

_Referenced in [sphinx-doc/sphinx#13204](../../sphinx-doc/sphinx/pulls/13204.md) on 2025-01-02 20:50_

---

_Comment by @kaddkaka on 2025-01-07 12:36_

Most of the pycodestyle rules are in preview set. When will they be upgraded to stable rules?

---

_Comment by @153957 on 2025-01-07 13:04_

I think `ruff format` would format code to be fully (unless there are changes to the format style) compatible with those rules, so there is probably no need to have these in stable. If you have code that you do not format with ruff then you can explicitly enable these preview rules for those files.
(I think that is the reason why they are not in stable, but may be wrong.)

---

_Comment by @kaddkaka on 2025-01-07 13:20_

I don't think that should be a reason to not promote those rules to stable if they are acting correctly.

Details:
1. We have a lot of files that we are not formatting with ruff.
2. Turning on `--preview` is unwanted since that together with `PL` would enable other unwanted rules like PLC1901 (see https://github.com/astral-sh/ruff/issues/14602)

---

_Comment by @MartinRoth on 2025-01-07 13:23_

Is there any update on when the rules E122 - E131 enter the preview stage? 

---

_Referenced in [Voyz/ibind#45](../../Voyz/ibind/issues/45.md) on 2025-02-12 04:38_

---

_Referenced in [astral-sh/ruff#17031](../../astral-sh/ruff/issues/17031.md) on 2025-03-28 07:57_

---

_Comment by @beauxq on 2025-04-05 20:04_

In this list, E704 has a check mark,
but E704 is not in the list here https://docs.astral.sh/ruff/rules/#error-e
What's going with that rule?

---

_Comment by @ntBre on 2025-04-07 13:57_

@beauxq You're right. E704 was added in https://github.com/astral-sh/ruff/pull/2680 but removed in https://github.com/astral-sh/ruff/pull/2773. I believe the concerns around the ignore list have been resolved, so we could bring it back. I'll remove the check mark for now.

---

_Comment by @MichaReiser on 2025-04-07 14:02_

> I believe the concerns around the ignore list have been resolved, so we could bring it back. I'll remove the check mark for now.

Can you expand on which concerns were addressed? I think the concern that it isn't an *on-by-default* rule still applies

---

_Comment by @ntBre on 2025-04-07 14:07_

> Can you expand on which concerns were addressed? I think the concern that it isn't an _on-by-default_ rule still applies

Oh I assumed that this just meant a way to disable rules and that this didn't exist at the time. I forgot the E rules are on by default.


---

_Comment by @beauxq on 2025-04-07 14:28_

If we don't want it as "E" because "E" is too default, maybe we could have the rule somewhere else, like "RUF"?

But I think the better way to go would be to keep it in "E", but match black's treatment (https://github.com/astral-sh/ruff/issues/2763) allowing the `...` for `.pyi` and overloads (which should be done even if it is moved somewhere else, like "RUF").
That way, I think people wouldn't mind it being on by default.

---

_Comment by @kaddkaka on 2025-05-02 10:00_

> I don't think that should be a reason to not promote those rules to stable if they are acting correctly.
> 
> Details:
> 
> 1. We have a lot of files that we are not formatting with ruff.
> 2. Turning on `--preview` is unwanted since that together with `PL` would enable other unwanted rules like PLC1901 (see [How to deal with counter-argument to rules? Concrete example PLC1901 #14602](https://github.com/astral-sh/ruff/issues/14602))

@MichaReiser you presented an argument against stabilizing these rules as they are already covered by ruff format. (https://github.com/astral-sh/ruff/discussions/14331#discussioncomment-11263890) 

I would like to run just the linter part without the formatter. If that is not intended usecase of ruff and these rules will not be stabilized we will stick with other linters. (I would prefer just one, ruff 😇) 

Any chance of re-evaluation? Or some middle-ground like bringing the rules into stable but making them "more" opt-in by grouping them in a subcategory of E? (I guess it's quite common to enable the E rules which means just stabilizing them would make them appear in many people's lint setup automatically. Maybe that's not a problem though.)

---

_Comment by @buhtz on 2025-05-02 10:25_

I do support the argument of kaddkaka .
A rule covered by formater is something very different from being covered by the linter.

Beside of that using formaters at all is a bad habit and prevent fresh coders from learning. They should try to write good code from the beginning and fix linter warnings them self. There are users avoid using the formater.

---

_Referenced in [astral-sh/ruff#18795](../../astral-sh/ruff/issues/18795.md) on 2025-06-19 15:00_

---

_Comment by @jgbishop on 2025-06-25 23:35_

Are E122 - E131 still planned?

---

_Comment by @hoel-bagard on 2025-06-25 23:56_

There is [a PR](https://github.com/astral-sh/ruff/pull/13585) implementing the E12X rules.

---

_Referenced in [archlinux/archweb#579](../../archlinux/archweb/pulls/579.md) on 2025-09-01 09:40_

---

_Comment by @kaddkaka on 2025-11-14 15:16_

> > I don't think that should be a reason to not promote those rules to stable if they are acting correctly.
> > Details:
> > 
> > 1. We have a lot of files that we are not formatting with ruff.
> > 2. Turning on `--preview` is unwanted since that together with `PL` would enable other unwanted rules like PLC1901 (see [How to deal with counter-argument to rules? Concrete example PLC1901 #14602](https://github.com/astral-sh/ruff/issues/14602))
> 
> [@MichaReiser](https://github.com/MichaReiser) you presented an argument against stabilizing these rules as they are already covered by ruff format. ([#14331 (comment)](https://github.com/astral-sh/ruff/discussions/14331#discussioncomment-11263890))
> 
> I would like to run just the linter part without the formatter. If that is not intended usecase of ruff and these rules will not be stabilized we will stick with other linters. (I would prefer just one, ruff 😇)
> 
> Any chance of re-evaluation? Or some middle-ground like bringing the rules into stable but making them "more" opt-in by grouping them in a subcategory of E? (I guess it's quite common to enable the E rules which means just stabilizing them would make them appear in many people's lint setup automatically. Maybe that's not a problem though.)

Over half a year has passed, could we discuss this? 

---

_Referenced in [astral-sh/ruff#21785](../../astral-sh/ruff/issues/21785.md) on 2025-12-04 09:20_

---

_Comment by @NN--- on 2025-12-30 14:46_

I tried to move to ruff but found that not all flake8 rules are applied in ruff.
So I probably need both tools ?

---

_Comment by @buhtz on 2025-12-30 15:11_

I am not related to astral/ruff. But as upstream maintainer I do combine several tools because of the issues you observed.

1. Ruff - Because it is fast.
2. Flake8 - Covers some rules Ruff don't.
3. PyLint - Very slow, but damn "pedantic", not only about coding style but about code smells.
4. Additionally I add `reuse lint` and `codespell` at the end in most of my projects.

e.g. See this for a real world example where I add all this linters as unit tests. https://github.com/bit-team/backintime/blob/dev/common/test/test_lint.py

---

_Comment by @NN--- on 2025-12-30 15:25_

Is there a point to run ruff if you still run flake8?
Yes it is super fast but doesn't have some important rules. 

---

_Comment by @buhtz on 2025-12-30 18:09_

> Is there a point to run ruff if you still run flake8?

Yes, it definitely is, because Ruff does not implement all Flake8 rules. But they are working on it. In my projects it is very rare that Flake8 (after running ruff) give some errors. In my foggy memory I would say it is often PEP8 coding style stuff, like to much blank lines or something like this.

But it is also the other way around. Ruff catch some errors that Flake8 won't.

---

_Comment by @xmatthias on 2025-12-30 19:03_

> But it is also the other way around. Ruff catch some errors that Flake8 won't.

Exactly this! 

> Yes, it definitely is, because Ruff does not implement all Flake8 rules. 

But it implements most rules (at least I personally) often violate - so having `ruff check` pass is almost always sufficient.
Flake8 is still used for whitespace issues - but once this issue resolves - i expect to remove flake8 from my projects.

---

_Comment by @153957 on 2025-12-30 19:11_

Wouldnt `ruff format` handle most of your whitespace issues?

---

_Comment by @timj on 2025-12-30 19:20_

> Wouldnt ruff format handle most of your whitespace issues?

That was the conclusion I came to. `ruff format` makes all these missing checks irrelevant because you know that they can't ever happen.

---

_Comment by @kaddkaka on 2025-12-30 19:24_

> > Wouldnt ruff format handle most of your whitespace issues?
> 
> That was the conclusion I came to. `ruff format` makes all these missing checks irrelevant because you know that they can't ever happen.

But that's irrelevant as `ruff format` is not `ruff check` and everyone running linters might not want to run formatters. This has already been up several times. 

---

_Comment by @timj on 2025-12-30 19:27_

I understand that check is not format. I've been following this GitHub issue a very long time hoping that the checks would turn up at some point for code repos where I'm not allowed to use `format`. I was noting that in some cases it is thankfully irrelevant for me.

---

_Comment by @pebaha- on 2025-12-30 19:38_

> > Wouldnt ruff format handle most of your whitespace issues?
> 
> That was the conclusion I came to. `ruff format` makes all these missing checks irrelevant because you know that they can't ever happen.

`ruff format` and `ruff check` are seperate commands and serve different purposes. If you use `ruff format` you also buy into black-compatible formatting, which not all projects subscribe to.

---

_Comment by @buhtz on 2025-12-31 10:04_

Yes, `ruff format` is not linting but auto-formating. I am not a friend of that (or `black`).
You won't learn writing PEP8-conform code when a tool does make the job for you. It is always a good advise not to use things like that.

---

_Comment by @MichaReiser on 2025-12-31 10:32_

Please avoid off-topic conversations on this issue. There are many users subscribed to it and each comment notifies all of them. Instead, open a new issue or discussion specific about your concern.

---

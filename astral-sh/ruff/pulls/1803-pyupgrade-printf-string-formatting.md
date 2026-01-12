```yaml
number: 1803
title: "Pyupgrade: Printf string formatting"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: PrintfStringFormatting
created_at: 2023-01-12T01:30:14Z
updated_at: 2023-01-21T14:37:22Z
url: https://github.com/astral-sh/ruff/pull/1803
synced_at: 2026-01-12T15:55:07Z
```

# Pyupgrade: Printf string formatting

---

_@colin99d_

A part of #827. This one is complicated so it will take me some more time. 

Please Note: I purposely did NOT implement the `--keep-percent-format` flag because it looks like the last flag I added was removed. If you want this flag in, just let me know.

Note: the issue of the /N{dog} format will plague us in UP030 and UP032. Whatever solution we find here needs to be applied in both of those places. Right now I am just thinking of looking for it, and if it exists, only showing a warning but not attempting top fix. I have NEVER seen this syntax in practice, and I think it is reasonable to give the people using it a warning, but don't fix.

---

_Comment by @colin99d on 2023-01-14 01:48_

@charliermarsh I have hit a blocker and I would love some assistance. Pyupgrade uses the regex ["(?<!\\)(?:\\\\)*(\\N\{[^}]+\})"](https://github.com/asottile/pyupgrade/blob/main/pyupgrade/_string_helpers.py#L9) to get an outcome like below:
<img width="504" alt="Screenshot 2023-01-13 at 8 41 24 PM" src="https://user-images.githubusercontent.com/72827203/212444866-ef183cd1-6d96-4702-a57e-0698d1159869.png">

We cannot use this regex in rust, because the regex uses look behinds, which are [not implemented in rust](https://stackoverflow.com/questions/61485063/is-there-alternative-regex-syntax-to-avoid-the-error-look-around-including-loo). I tried taking the post's advice and reimplementing the regex without look behinds, "[^\\](?:\\\\)*(\\N\{[^}]+\})", but then the character before is included in the string, which causes the program to break:
<img width="446" alt="Screenshot 2023-01-13 at 8 44 51 PM" src="https://user-images.githubusercontent.com/72827203/212445008-10a79e35-8ddc-4dd2-a4ae-ebe2625b7f67.png">
[This SO post](https://stackoverflow.com/questions/3926451/how-to-match-but-not-capture-part-of-a-regex) seems to indicate that there is NOT a way to accomplish what I am looking to do.

After this I tried using the `fancy_regex` create; however, they [do not implement the `split` method](https://github.com/fancy-regex/fancy-regex/issues/104) which is required. With this I am unsure of what to do next. We could check for my ugly regex, and just ignore those (or report them as issues but not fix them). But that really bothers the perfectionist in me. Let me know how you think we should handle this.

If we do go the ignore/only report a check route I believe this is the regex we should use: "(?:\\\\)*(\\N\{[^}]+\})"

---

_Comment by @charliermarsh on 2023-01-14 03:25_

Why do we need / why does the regex exist? Is it to avoid treating named unicode characters as `{}`-style formatting placeholders?

---

_Comment by @colin99d on 2023-01-14 03:33_

I know its for statements like these: `"%s \N{snowman}" % (a,)`, because they are currently causing issues in my code. However, I am not sure the specific reason they are needed.

---

_Comment by @colin99d on 2023-01-14 19:39_

It looks like it exists so that `"%s \N{snowman}" % (a,)` does not turn into `"%s \N{{snowman}}" % (a,)`. If this happens we get the following error in python:
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 3-14: unknown Unicode character name

But we DO need to convert `"brace {} %s" % (1,)` => `"brace {{}} {}".format(1)`

---

_Comment by @charliermarsh on 2023-01-14 20:20_

Is there a way that we could run the regex, extract the match, then run a _second_ regex over the match to get a more precise range?

---

_Comment by @colin99d on 2023-01-14 20:28_

> Is there a way that we could run the regex, extract the match, then run a _second_ regex over the match to get a more precise range?

Right now the regex I have developed that is the closest is `"[^\\](?:\\\\)*(\\N\{[^}]+\})"`. It has two issues:
1. It grabs one character before the match starts (this could in theory be fixed by your solution)
2. It would never match: `\N{snowman}" % (a,)` because there is no character before the strong (we can't make it optional because then it would lose all effect), I am not sure how to overcome this shortcoming

Additionally, I am not sure if we could use this "double regex" with methods like `split`. I have been trying to think of other ways we could do this. I am sure we could do something with tokens, but not sure how we could turn this back into a string simply.

Basically all we need to confirm is that there is an odd number of `\` before a `N{something}`

---

_Comment by @charliermarsh on 2023-01-14 22:03_

Ok, let me take a closer look. Don't worry about resolving it for now. I have to spend some time with the code to understand the problem before I can offer good advice :)

---

_Comment by @colin99d on 2023-01-14 22:06_

Sounds good! I am working on the dict implementation, once I get that done this is solved besides the regex issue.

---

_Comment by @colin99d on 2023-01-15 21:01_

@charliermarsh I am moving this out of draft now. This one was pretty complicated so I left as many notes as I could. Everything is working as I want it to besides the regex we discussed above. However, please note I departed from pyupgrade for two negative test cases (test cases where the linter should NOT run):
```
        # don't rewrite string-joins in dict literal
        '"%(ab)s" % {"a" "b": 1}',
```
Rustpython_ast automatically makes this string-join "ab". This means that not linting here would take more logic on our end, and would end up being less helpful for the user. If you can think of an edge case where this is an issue, let me know and I can fix.

```
        # don't rewrite strangely styled things
        '"%(a)s" % {"a"  :  1}',
```
The rust_python ast we are using doesnt have the spaces factored in, so they do not create issues. Fixing this one will require adding complicated logic, to decrease the user experience. As before, if you disagree just let me know.

---

_Marked ready for review by @colin99d on 2023-01-15 21:01_

---

_Comment by @charliermarsh on 2023-01-15 21:37_

Thanks. I know I owe you a review here! Hasn’t slipped my mind. Just trying to find time since it’s a big PR and requires focus :)

---

_Comment by @colin99d on 2023-01-15 21:39_

No worries! I figured it might be a day or two before you get this one! By far the most complicated one I have done.

---

_Comment by @charliermarsh on 2023-01-17 00:55_

(Reviewing now.)

---

_Comment by @charliermarsh on 2023-01-17 01:07_

I haven't finished reviewing the code yet, but I notice this is failing on a few cases:

```diff
@@ -244,8 +244,8 @@ def run(
     print(
-        "Writing merged info for %s slides (%s unrecognized)"
-        % (len(merged_slide_data), unrecognized_count)
+        "Writing merged info for {%s unrecognized:"}
+       .format(len(merged_slide_data), unrecognized_count)
     )
     merged_slide_data = sorted(
         merged_slide_data, key=lambda row: cast(str, row["Slides_slide_barcode"])
```

```diff
     def composite_image(self, files: List[str]) -> List[str]:
         if len(files) != len(self.channels):
             raise ValueError(
-                "Expected one image (got %d) per channel (got %d)"
-                % (len(files), len(self.channels))
+                "Expected one image (got {got %d:"}
+               .format(len(files), len(self.channels))
             )

         composited_image = None
```


---

_Renamed from "Printf string formatting" to "Pyupgrade: Printf string formatting" by @colin99d on 2023-01-17 01:11_

---

_Comment by @colin99d on 2023-01-17 01:14_

I see you are making changes. Let me know once you are done and I can attack issues you find.

---

_Comment by @charliermarsh on 2023-01-17 01:16_

I'll stop for now :)

---

_Comment by @charliermarsh on 2023-01-17 01:17_

> I have NEVER seen this syntax in practice, and I think it is reasonable to give the people using it a warning, but don't fix.

FWIW, that's fine with me for now.

---

_Comment by @charliermarsh on 2023-01-17 01:21_

> --keep-percent-format

Yeah we don't need to implement these flags IIUC because that's just equivalent to `--ignore UP031`.

---

_Review comment by @andersk on `src/rules/pyupgrade/helpers.rs`:9 on 2023-01-17 02:02_

It’s `\N{dog}`, not `/N{dog}`.

---

_@andersk reviewed on 2023-01-17 02:02_

---

_Review comment by @colin99d on `src/rules/pyupgrade/helpers.rs`:9 on 2023-01-17 02:06_

\N{facepalm}

---

_@colin99d reviewed on 2023-01-17 02:06_

---

_Comment by @colin99d on 2023-01-17 02:13_

> 

Pyupgrade won't event attempt to fix these, time to find out why and get it fixed.

---

_Comment by @colin99d on 2023-01-17 03:28_

@charliermarsh, unfortunately I was not able to fix the two issues you found tonight. However, I was able to determine that `parse_percent_format` is the function that is causing the issue. I was also able to run the same strings through the actual pyupgrade library, to determine the structs that should have been generated. This allowed me to create two failing unit tests. I am hoping to get this issue fixed by Sunday night at the latest. Also, I am wondering why pyupgrade does not change the strings at all, because the statement does not seem to violate any rules laid out in their unit tests.

---

_Comment by @charliermarsh on 2023-01-17 04:18_

@colin99d - My suggestion is to try and use `CFormatString` from RustPython instead of regular expressions. You can see how it works in the f-string PR. But e.g.:

```rs
let Ok(format_string) = CFormatString::from_str(&left_string[1..left_string.len() - 1]) else {
    return;
};
```

This gives you structured data about the string that I _think_ we can use to reconstruct it. And the parser will be very robust. I can also take a crack at this if you're too tired of this one!

---

_Comment by @colin99d on 2023-01-17 12:40_

> 

I have no problem doing it (always good to learn more tools for later PRs). Will try to have it done by this Sunday night.

---

_Comment by @charliermarsh on 2023-01-17 12:41_

Ok cool, thank you -- as always, appreciate all the effort here! Let me know how it goes. I can't guarantee that it will work, since I haven't done the exercise myself, but I suspect it _should_, and would lead to a really strong solution.

---

_Comment by @colin99d on 2023-01-17 12:44_

> Ok cool, thank you -- as always, appreciate all the effort here! Let me know how it goes. I can't guarantee that it will work, since I haven't done the exercise myself, but I suspect it _should_, and would lead to a really strong solution.

No problem! Also, would love to chat on Discord or something about next steps after I get pyupgrade done in the next couple weeks. Right now I am thinking about attacking pylint because that takes the most time in our CI and in pre-commit.

---

_Comment by @charliermarsh on 2023-01-17 12:55_

@colin99d - Sounds great. I did setup a Discord, but... I haven't shared it with anyone yet, so it's just me :joy:

---

_Comment by @colin99d on 2023-01-17 12:57_

> @colin99d - Sounds great. I did setup a Discord, but... I haven't shared it with anyone yet, so it's just me 😂

Awesome! Honestly a small private discord is probably the way to go, where you just invite people actively contributing. That way you dont get bots and spammers. Also, ruff should just work so once this tool is ready community members should not need a lot of support.

---

_Comment by @charliermarsh on 2023-01-17 20:29_

One thing I'm realizing, just to confirm my understanding, is that in many cases, these will actually be fixed yet again by the f-string rule, right?

E.g. `'%s %s' % (a, b)` to `'{} {}'.format(a, b)` to `f'{a} {b}'`.

---

_Comment by @colin99d on 2023-01-17 20:41_

> One thing I'm realizing, just to confirm my understanding, is that in many cases, these will actually be fixed yet again by the f-string rule, right?
> 
> E.g. `'%s %s' % (a, b)` to `'{} {}'.format(a, b)` to `f'{a} {b}'`.

That is correct!

---

_Comment by @colin99d on 2023-01-18 01:35_

> One thing I'm realizing, just to confirm my understanding, is that in many cases, these will actually be fixed yet again by the f-string rule, right?
> 
> E.g. `'%s %s' % (a, b)` to `'{} {}'.format(a, b)` to `f'{a} {b}'`.

Also, the f-strings purposely ignores strings with format specifiers, so that UP030 can handle it first.

---

_Comment by @colin99d on 2023-01-18 03:28_

@charliermarsh, I began working on the conversion to CFormatQuantity. This definitely looks like it will be a better way to do things! So far I have one question, how do I convert CConversionFlags to a string? So far I have been digging in [here](https://docs.rs/rustpython-common/0.2.0/rustpython_common/cformat/struct.CConversionFlags.html), and I have not been able to come up with anything.

---

_Comment by @charliermarsh on 2023-01-18 03:41_

Hmm, I think I'd look at the [RustPython source](https://github.com/RustPython/RustPython/blob/59b191c789c891a500659d2c3b29a645883f780a/common/src/cformat.rs#L410) for those flags to understand how they're created, then implement the inverse? Does that work?

---

_Comment by @colin99d on 2023-01-18 16:40_

> 

Oh perfect, thanks

---

_Comment by @colin99d on 2023-01-19 00:15_

@charliermarsh, if you dont mind digging into this if you get some free time I think it might have some differences that are too big. For example, if the precision is just ".", this library decides to [make it 6](https://github.com/RustPython/RustPython/blob/59b191c789c891a500659d2c3b29a645883f780a/common/src/cformat.rs#L147). This is different to python, which would round to 0 decimals if precision is just 0. If we decide this doesn't work we can go back to the old way of doing it.


---

_Comment by @charliermarsh on 2023-01-19 00:56_

(Will try to take a look tonight.)

---

_Comment by @charliermarsh on 2023-01-19 22:39_

I've started poking at this but there's a lot to read :)

Are there any other discrepancies beyond what you've described? (Would it make sense to fix those in RustPython, if they're actual CPython incompatibilities?)

---

_Comment by @colin99d on 2023-01-19 22:40_

> 

There are a couple, yeah. Also, I would be interested in knowing why they chose 6. If you run the unit tests you can see them. We could attempt a PR there, or I could Just try to fix the one we have (using the function ported from pyupgrade)? Let me know what you want me to do. 

---

_Comment by @charliermarsh on 2023-01-19 23:04_

I think it's because of this?

```py
>>> "%f" % 2.12
'2.120000'
```

---

_Comment by @colin99d on 2023-01-20 00:22_

You are right, but the issue is they are treating the below code as a 6. This means if we get a 6 we dont know if it should be a 0 or a 6.
<img width="248" alt="Screenshot 2023-01-19 at 7 21 48 PM" src="https://user-images.githubusercontent.com/72827203/213590975-e0826968-2a02-4928-ab9c-1411765c7141.png">


---

_Comment by @charliermarsh on 2023-01-20 00:24_

Yeah that, I think, should be fixed. I'll look into fixing it as I review this. Are there other limitations though?

---

_Comment by @colin99d on 2023-01-20 00:37_

It looks like this and pyupgrade are disagreeing on another one. But I am not sure who is right. The pyupgrade tests is [here](https://github.com/asottile/pyupgrade/blob/main/tests/features/percent_format_test.py#L23). Pyupgrade wants to convert this into a statement, but rustpython doesn't. It looks like rustpython might be right though:
```
>>> "%%" % 2.12
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: not all arguments converted during string formatting
>>> "%%" % "hello"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: not all arguments converted during string formatting
>>>
```
This is the same error you get if you submit a string that definitely does not work:
```
>>> "hello" % "goodbye"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: not all arguments converted during string formatting
>>>
```
This is the only other limitation.

---

_Comment by @charliermarsh on 2023-01-20 00:42_

Yeah that seems ok. So if I go and fix the RustPython parse limitation, does this seem good to go from your perspective?

---

_Comment by @colin99d on 2023-01-20 00:47_

I am getting a little more weird behavior, but for right now im guessing its on me. I will let you know by EOD today if this is the only issue.

---

_Comment by @charliermarsh on 2023-01-20 00:51_

I guess a related issue is that with:

```
"%f" % 1
```

I think RustPython reports that as precision(6), so I'm not sure we can reconstruct it exactly?

---

_Comment by @colin99d on 2023-01-20 00:53_

> 

Yes this issue is definitely on them, but there is some other stuff im seeing that is weird. This is good though. We get to help the whole rust/python ecosystem on this stuff.

---

_Comment by @charliermarsh on 2023-01-20 00:54_

Yeah, my preference is to use the parser if we can. I think it will be faster and more robust (and easier for us to maintain, reason about, etc.) than regular expressions. So I want to push us down that path unless we can find a reason that it's ~impossible.

---

_Comment by @charliermarsh on 2023-01-20 00:54_

I'll put together a PR to fix those limitations in RustPython now.

---

_Comment by @charliermarsh on 2023-01-20 00:56_

To be clear, though, I am impressed by your regex abilities!

---

_Comment by @colin99d on 2023-01-20 00:57_

> To be clear, though, I am impressed by your regex abilities!

Haha, 2 weeks ago I would not have known that. Regex101, and a buddy I know who did them full time for a couple months got me a lot further along than I should be (and some are just copied from pyupgrade).

---

_Comment by @colin99d on 2023-01-20 00:59_

> Yeah, my preference is to use the parser if we can. I think it will be faster and more robust (and easier for us to maintain, reason about, etc.) than regular expressions. So I want to push us down that path unless we can find a reason that it's ~impossible.

Yes. I would much rather rewrite now than deal with 100 edge case bugs once the feature gets merged.

---

_Comment by @charliermarsh on 2023-01-20 01:33_

https://github.com/RustPython/RustPython/pull/4463

---

_Comment by @charliermarsh on 2023-01-20 01:36_

BTW, I think pyupgrade handles `%.f` incorrectly, maybe?

This code:

```py
x = 1
y = 2
z = 3

print('%.2f %.f %f' % (x, y, z))
```

Gets changed to:

```py
x = 1
y = 2
z = 3

print('{:.2f} {:.f} {:f}'.format(x, y, z))
```

Which yields:

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/ruff/foo.py", line 5, in <module>
    print('{:.2f} {:.f} {:f}'.format(x, y, z))
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
ValueError: Format specifier missing precision
```

---

_Comment by @colin99d on 2023-01-20 01:49_

> 

Looks like you are right, maybe we keep the 6 as a default if they do not specify, OR we could just issue a warning but not fix. I like the first option a little more, but as always up to you.

Edit: Actually I dont like the first one, it changes how people's code operates with no warning. Maybe we add a 0?

---

_Comment by @charliermarsh on 2023-01-20 01:50_

@colin99d - Mind if I do a pass on this from the current state of the code?

---

_Comment by @colin99d on 2023-01-20 01:50_

> @colin99d - Mind if I do a pass on this from the current state of the code?

I have some bad code im fixing. Unit tests caught a new edge case. Let me finish and then she's all yours.

---

_Comment by @charliermarsh on 2023-01-20 01:51_

Okay thanks. Just LMK when it's ready. I'll point to the updated RustPython branch and get it into a finished state, hopefully.

---

_Comment by @charliermarsh on 2023-01-20 01:51_

And yeah -- I think we map `%.f` to `{:.0f}`. Same with `%.s` to `{:.0}` etc.

---

_Comment by @colin99d on 2023-01-20 02:27_

@charliermarsh I am ready for you. Running `cargo test printf_string_formatting` currently results in two failures. Both are related to the precision issue.

---

_Comment by @charliermarsh on 2023-01-20 05:05_

Working my way through this one, found at least one bug but I think it might be in our logic and not the parser.

---

_Comment by @charliermarsh on 2023-01-21 02:24_

@colin99d - Ok, I think this is passing all tests now. I can think of a few ways that it could be improved so I might iterate on it a bit more. (For example: it'd be nice if we could avoid transforms that create really long string format calls.) It's a really significant transform, so I want it to be rock-solid before it goes out.


---

_Comment by @colin99d on 2023-01-21 02:27_

> 

One thing I did in another check, is that if the new string is longer I would not do the change. But sounds good! Thanks for helping me out on this. It was definitely a lot trickier than I thought it would be.

---

_Comment by @charliermarsh on 2023-01-21 02:31_

I guess I'm also thinking about whether we should be fixing multi-line strings.

---

_Comment by @colin99d on 2023-01-21 02:35_

I can see it either way. Pyupgrade did NOT fix the strings that you sent me earlier that broke.

---

_Comment by @charliermarsh on 2023-01-21 04:59_

Alright, I'm gonna merge this as soon as https://github.com/RustPython/RustPython/pull/4463 is merged.

---

_Merged by @charliermarsh on 2023-01-21 14:37_

---

_Closed by @charliermarsh on 2023-01-21 14:37_

---

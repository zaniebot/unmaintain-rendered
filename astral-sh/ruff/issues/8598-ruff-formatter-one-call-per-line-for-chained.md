```yaml
number: 8598
title: "ruff formatter: one call per line for chained method calls"
type: issue
state: closed
author: MartinBernstorff
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-10T11:43:47Z
updated_at: 2025-12-15T15:29:51Z
url: https://github.com/astral-sh/ruff/issues/8598
synced_at: 2026-01-10T11:09:50Z
```

# ruff formatter: one call per line for chained method calls

---

_Issue opened by @MartinBernstorff on 2023-11-10 11:43_

This is something I miss _tremendously_ from the rust ecosystem. One example:

![image](https://github.com/astral-sh/ruff/assets/8526086/1cf147a0-8959-4c5c-b9d0-b588444e9622)

When formatted becomes:

![image](https://github.com/astral-sh/ruff/assets/8526086/fe0bc898-a0be-451f-862c-210e78d713b6)

I think the first example is _much_ more readable, and would **love** a Python formatter which supported it ❤️

**Meta:**
I was uncertain about where to make a feature request for the ruff formatter, hope this is the right place! 

---

_Label `formatter` added by @zanieb on 2023-11-10 16:19_

---

_Label `style` added by @zanieb on 2023-11-10 16:19_

---

_Comment by @MichaReiser on 2023-11-27 11:10_

Hy @MartinBernstorff 

This is something that I also noticed coming from JS where Prettier formats call chains as outlined by you above (except without the outer parentheses). 

My reasoning for Black/Ruff's call chain formatting is that Black tries to avoid parenthesizing expressions which is more idiomatic (I think):


```python
if a + long_call(
	arg1, 
	arg2
): 
	pass
```

instead of 

```
if (
	a + 
	long_call(
		arg1, 
		arg2
	)
): 
	pass
```

The way this is implemented is that anything inside of parentheses gets a higher split priority (try to split the content inside parentheses first). 

I'm not saying that we can't improve the formatting. I'm just trying to explain where I believe black's formatting decision is coming from. 

---

_Comment by @MartinBernstorff on 2023-11-27 12:42_

@MichaReiser Appreciate the context! I completely agree your first example is more readable, so there's some work on defining the goal here.

From a very brief consideration, I can only come up with chained method calls as an example of my desired splitting, so perhaps this is a special case? Love to lean on your experience here!

---

_Comment by @nick4u on 2024-02-21 07:55_

I would love to see some option to preserve chain methods-call-per-line
SQLAlchemy code is so much more readable when those new lines are preserved - it almost looks like real SQL
```python
some_query = (
  select(whatever.id, x.something)
  .join(x, x.y == whatever.y)
  .where(x > 12)
  .order_by(whatever.id)
)
```
anyway - ruff rules! 

---

_Comment by @Red-Eyed on 2024-03-01 09:08_

Hello, first of all, thank you for a great piece of software!  

Came here to ask for a similar functionality mentioned above: I would like to see chaining methods on new line:  

It is heavily used in the [expression](https://github.com/dbrattli/Expression?tab=readme-ov-file#composition) library:

```python
(Seq(find_node(labels, polygons_predicate))
    .choose(lambda x: x)
    .map(parse_points)
    .choose(lambda x: x),
)
```


---

_Comment by @mattharrison on 2024-03-20 21:07_

As a big proponent of chaining in Pandas and Polars, I would love to see some options to help with code readability with these libraries. Starting each line with a period makes the code easier to understand.

---

_Comment by @mchccc on 2024-03-28 10:37_

As a sort of workaround, you can add comments in between the lines:
```
        plot_df = (
            self.data
            # Round costs
            .assign(cost=np.round((self.data.cost / 1000), decimals=0))
        )
```
One might even argue it helps _more_ with readability ;)

---

_Comment by @thomas-mckay on 2024-05-10 18:41_

As mentioned by nick4u, this would be a great feature for SQLAlchemy, and I'll add Django to the list:
```python
result = (
    SomeModel.objects
    .filter(...)
    .annotate(...)
    .prefetch_related(...)
    .order_by(...)
)
```
We format like this naturally at my current job, and the main blocking-point of adopting a formatter is that it mangles this syntax regardless of line length (i.e.: the formatter will reformat even code that is within line-length limits). To be fair, the larger pain-point is that formatters in general don't care about what the developer's intent was regarding readability, it just assumes it knows better (or that the dev didn't care to begin with). So both a carefully-crafted, readable piece of code and a long unreadable string will be overridden regardless. The magic-comma is a big step in the right direction, as it lets developers tell the formatter what is more readable. Thankfully, that covers a lot of cases, but not all.

As tmke8 commented on [#9577](https://github.com/astral-sh/ruff/issues/9577#issuecomment-1900178928), maybe the solution to circumvent the complexity of implementing this type of formatting, at least for now, would be for the formatter to understand "magic-parenthesis". In other words, if it encounters this formatting, apply formatting within the parenthesis, but don't remove them. The example above would be left unchanged, but 

```python
result = (
    SomeModel.objects
    .filter(some_very_long_column_name_1__startswith='some_very_long_value', some_very_long_column_name_2__startswith='some_very_long_value')
    .order_by(...)
)
```
would become 

```python
result = (
    SomeModel.objects
    .filter(
        some_very_long_column_name_1__startswith='some_very_long_value',
        some_very_long_column_name_2__startswith='some_very_long_value',
    )
    .order_by(...)
)
```

Also, this would have a great side-effect for multi-line conditions, I think:

Currently, this code:
```python
if (
    self.task
    and not self.task.is_canceled
    and not self.task.is_finished
): ...
```

gets auto-formatted to
```python
if self.task and not self.task.is_canceled and not self.task.is_finished: ...
```

But with "magic-parenthesis", the first formatting would be left as-is.

PS: I realize, this comment may be taking the issue in a different direction. If so, let me know and I'll open a new one.
PPS: Thank you for Ruff :heart: 


---

_Comment by @parikls on 2024-05-23 16:01_

+1 for this. using such code-style constantly for queries for both alchemy and django

---

_Comment by @shner-elmo on 2024-06-01 22:10_

Great suggestion for a great linter!

Has there been any progress so far on this feature?

---

_Comment by @aayushchhabra1999 on 2024-06-02 19:23_

+1 
Need this for method chaining and sqlalchemy formatting.

CC: @MichaReiser @charliermarsh - Any work or plans for this anytime soon?

---

_Comment by @the-infinity on 2024-06-12 10:57_

+1 too, also at SQLAlchemy, this would really improve readability if you have this option.

---

_Comment by @yamplum on 2024-08-13 18:14_

Just wanted to raise another voice in support of this feature. It seems like Black supports this style of formatting? https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#call-chains

I would really prefer to stick with a Rust-based tool but this might be worth switching over.

---

_Comment by @MichaReiser on 2024-08-14 06:49_

> Just wanted to raise another voice in support of this feature. It seems like Black supports this style of formatting? [black.readthedocs.io/en/stable/the_black_code_style/current_style.html#call-chains](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#call-chains)

Ruff should support call-chain formatting the same as Black. If there are cases where Black applies call-chain formatting and Ruff doesn't, then that's a bug. 

[Black](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AF4AMRdAD2IimZxl1N_WlbvK5V89Lw1SkhVsHyCyFI3Ge-c_SY0TY6P_OLyAFiRQMuVoFY03JMyZhZN-7ASJqq2VZCUup6xZH5v_zwde2-zdNfCZbvFEFX_KGNn5o_M_WL_SihxF35F2tjqgX6o52pPk9LHKmz0GMSc0y7DI3t7cahkVFjBBB85STD5GrbbkG9moy1aXtJl7ZsMs2ryKfyrtPbVzx7MNBU5H2VvrkYgfAtgkH_RlVRE_GyUt4ICVVtGU2E0bkbZSUgAVZfIsddZUJ8AAeAB-QIAAKxrmxCxxGf7AgAAAAAEWVo%3D)
[Ruff](https://play.ruff.rs/d3a035bc-9cbb-4853-80cc-d2c07f3128b8?secondary=Format)

---

_Comment by @yamplum on 2024-08-14 07:26_

> Ruff should support call-chain formatting the same as Black. If there are cases where Black applies call-chain formatting and Ruff doesn't, than that's a bug.

Sorry, you're right — it seems that Ruff matches Black here, however both "fail" in this manner when there is only one method in the "chain", [like here](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AEZAK1dAD2IimZxl1N_WlbvK5V89Lw1SkhVsHyCx9GZ6qFyX6O6Dgbtf1mqAYh1u3gpTGEoS6HRi596QCkIoR9bReFJ-fCjKtsup1JLDK1oJbjnHHTMgVTh5_VrxhQjS-kug2cB10E1FF8z8BNBFrsQAAmPIoLpBG9LifBwPOklX1KCXMtEtqxKQDQcxf9KYiCYskYM0J_4xhvzGYS9NJSZNfzwwm91m3N-r6kHooO6bakAAAAAAK_zSAH6PwTvAAHJAZoCAAC_Z6sZscRn-wIAAAAABFla). It would be great if cases like that could also be formatted sanely.

---

_Comment by @MichaReiser on 2024-08-14 07:28_

> Sorry, you're right — it seems that Ruff matches Black here, however both "fail" in this manner when there is only one method in the "chain", [like here](https://black.vercel.app/?version=stable&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AEZAK1dAD2IimZxl1N_WlbvK5V89Lw1SkhVsHyCx9GZ6qFyX6O6Dgbtf1mqAYh1u3gpTGEoS6HRi596QCkIoR9bReFJ-fCjKtsup1JLDK1oJbjnHHTMgVTh5_VrxhQjS-kug2cB10E1FF8z8BNBFrsQAAmPIoLpBG9LifBwPOklX1KCXMtEtqxKQDQcxf9KYiCYskYM0J_4xhvzGYS9NJSZNfzwwm91m3N-r6kHooO6bakAAAAAAK_zSAH6PwTvAAHJAZoCAAC_Z6sZscRn-wIAAAAABFla). It would be great if cases like that could also be formatted sanely.

No worries. I do agree that better call chain formatting would be great.

---

_Comment by @leotrs on 2024-11-26 07:36_

Any movement here? A similar conversation over at the black repository ended up in a less than welcoming response.

---

_Comment by @tranhd95 on 2024-11-28 12:29_

Seems like putting `# fmt: skip` next to the closing parentheses works as a quick hack.

e.g. using the snippet above like so
```py
(
    select(whatever.id, x.something)
    .join(x, x.y == whatever.y)
    .where(x > 12)
)  # fmt: skip
```


---

_Comment by @olofahlen on 2024-12-06 11:25_

Adding my voice of support for this feature. @mattharrison taught me to embrace the fluent interface and now I'm working hard to convert my colleagues, and also sell them on Black or Ruff as a formatter. Without nice formatting of call chains it's harder. Also switched from Black to Ruff hoping that this community might be more open for something like magic parentheses, or at least one call per line.

Thanks for your great work!

---

_Comment by @MichaReiser on 2024-12-06 12:03_

> Any movement here? A similar conversation over at the black repository ended up in a less than welcoming response.

No. We consider this important but it didn't made the cut for the upcoming 2025 release. We're all just a bit thin stretched

---

_Comment by @leotrs on 2024-12-06 20:45_

Understandable! Is there any way the community can nudge the development priorities? :) 

---

_Comment by @MichaReiser on 2024-12-07 13:11_

Upvoting the issue is the way to go. We frequently look at the most upvoted issues to decide on priorities and this one has a fair amount of upvotes.

---

_Comment by @MichaReiser on 2024-12-31 15:42_

> Any movement here? A similar conversation over at the black repository ended up in a less than welcoming response.

Do you have a link to the issue? I'm interested in understanding their reasoning. I only found https://github.com/psf/black/issues/1037, which has some interesting context on why it is the way it is today.

---

_Comment by @olofahlen on 2024-12-31 19:34_

> Do you have a link to the issue? I'm interested in understanding their reasoning. I only found [psf/black#1037](https://github.com/psf/black/issues/1037), which has some interesting context on why it is the way it is today.

Short on time as it's NYE (happy new year), but here are a few more:
https://github.com/psf/black/issues/571
https://github.com/psf/black/issues/1723
https://github.com/psf/black/issues/2920
https://github.com/psf/black/issues/67 (I suppose this was the "original")

There's also talk outside of Github on the matter, for example this lonely dude: https://stackoverflow.com/questions/77921369/black-formatting-of-fluent-method-calls


---

_Comment by @genevieve-me on 2025-02-28 02:40_

Does anyone know if there's currently a workaround for keeping the chained method in the third snippet of this playground link? https://play.ruff.rs/b5cb5ba0-f15c-4c37-a35e-c51733358203

The example code is obviously nonsense but I am hitting this on real code where ruff, like black, allows method chaining in parentheses on unindented code, but will compress it for some reason in indented blocks.

---

_Comment by @dhirschfeld on 2025-02-28 05:51_

> Does anyone know if there's currently a workaround

`# fmt: skip`

---

_Comment by @MichaReiser on 2025-02-28 07:17_

> The example code is obviously nonsense but I am hitting this on real code where ruff, like black, allows method chaining in parentheses on unindented code, but will compress it for some reason in indented blocks.

I don't think this is related to the indentation but that the chain is shorter and fits into the configured line length. Using the second chain gives consistent formatting https://play.ruff.rs/184cf06e-2246-44b3-98a9-f92abfd4bd1e (I'm not arguing that flattning the chain looks a bit silly)

---

_Comment by @luksquaresma on 2025-04-24 17:56_

I'm also in need of this functionality.

Something that turns this:
```python
variable = something.fist_method("some string").second_method("other string").third_method("another string").forth_method("yet another string")
```

Into this:
```python
variable = (
    something
    .fist_method("some string")
    .second_method("other string")
    .third_method("another string")
    .forth_method("yet another string")
)
```

But **NOT this**:
```python
variable = (
    something.fist_method("some string")
    .second_method("other string")
    .third_method("another string")
    .forth_method("yet another string")
)
```

---

_Comment by @olofahlen on 2025-04-30 12:03_

@luksquaresma agreed. Also worth adding **NOT this**

```python
variable = (
    something.loc["2024"]
    .fist_method("some string")
    .second_method("other string").loc["2024-01"]
    .third_method("another string")
    .forth_method("yet another string")
)
```

instead we want this

```python
variable = (
    something
    .loc["2024"]
    .fist_method("some string")
    .second_method("other string")
    .loc["2024-01"]
    .third_method("another string")
    .forth_method("yet another string")
)
```

Black and ruff only do line breaks for method calls, but here `.loc` (drawing on pandas `.loc` as an example) is something else (for example a property) so it stays on the same line. It would be desirable if any attribute (method or not) accessed with the `.`-syntax is done on a separate line.

---

_Comment by @bakerbs on 2025-07-11 13:34_

Would also love for this to be implemented.

---

_Comment by @nicpottier on 2025-07-13 21:01_

I'll also put in my hat tip here. Fluent interfaces are common in Python and ruff/black mangling these is a big problem in wider adoption.

---

_Comment by @luksquaresma on 2025-07-13 21:40_

One workaround I've been able to use is to put the first object inside parenthesis to achieve the same effect.

It results in something like this:

```
variable = (
    (something)  # parenthesis here
    .fist_method("some string")
    .second_method("other string")
    .third_method("another string")
    .forth_method("yet another string")
)
```

It's not ideal, but works ok for me.

---

_Comment by @olofahlen on 2025-07-15 10:03_

> One workaround I've been able to use is to put the first object inside parenthesis to achieve the same effect.
> ...

Thanks that's a good tip, I will be using that. Unfortunately it doesn't go "all the way", for example this
```python
variable = (
    (something)
    .fist_method("some string")
)
```
gets reformatted to
```python
variable = (something).fist_method("some string")
```
and interestingly this
```python
variable = (
    (something)  # a comment
    .fist_method("some string")
)
```
gets reformatted to
```python
variable = (something)  # a comment
.fist_method("some string")
```
which is a syntax error, so there's a bug in ruff here.


---

_Comment by @alexreinking on 2025-07-28 14:05_

Another example from Halide...

```python
            (
                g.output_buf.bound(x, 0, g.input_buf.width())
                .bound(y, 0, g.input_buf.height())
                .bound(c, 0, 3)
                .reorder(c, x, y)
                .tile(x, y, xi, yi, 32, 32, hl.TailStrategy.RoundUp)
                .tile(xi, yi, xii, yii, 2, 2)
                .gpu_blocks(x, y)
                .gpu_threads(xi, yi)
                .unroll(xii)
                .unroll(yii)
                .unroll(c)
            )
```

https://github.com/halide/Halide/blob/d1c7b6136d6444cb9bcfd6bcdcfff4230f6da5c5/python_bindings/apps/interpolate_generator.py#L94-L106

It would really be great if the `.output_buf` and (at least) the subsequent `.bound(x, 0, g.input_buf.width())` could get their own lines. 

---

As an aside, when we were formatting this code manually, we preferred to write the following, but I understand newline escapes are frowned upon in Python-land.

```python
            g.output_buf \
             .bound(x, 0, g.input_buf.width()) \
             .bound(y, 0, g.input_buf.height()) \
             .bound(c, 0, 3) \
             .reorder(c, x, y) \
             .tile(x, y, xi, yi, 32, 32, hl.TailStrategy.RoundUp) \
             .tile(xi, yi, xii, yii, 2, 2) \
             .gpu_blocks(x, y) \
             .gpu_threads(xi, yi) \
             .unroll(xii) \
             .unroll(yii) \
             .unroll(c)
```

---

_Comment by @jmnunezd on 2025-08-07 15:32_

> I'm also in need of this functionality.
> 
> Something that turns this:
> 
> variable = something.fist_method("some string").second_method("other string").third_method("another string").forth_method("yet another string")
> Into this:
> 
> variable = (
>     something
>     .fist_method("some string")
>     .second_method("other string")
>     .third_method("another string")
>     .forth_method("yet another string")
> )
> But **NOT this**:
> 
> variable = (
>     something.fist_method("some string")
>     .second_method("other string")
>     .third_method("another string")
>     .forth_method("yet another string")
> )


LOVE IT! This is what we would love to see!

Ruff has been fantastic in my workflow, but this issue annoys me every now and then. I often work with fluent APIs in Polars, and the dot-leading layout really helps me visually align and understand the transformation pipeline.

It would be amazing to have this style as an optional formatting mode in Ruff, Thanks!


---

_Comment by @MoritzHorch on 2025-10-08 06:25_

I kindly would like to ask what the status of this issue is.

There are many more fluent APIs which are widely used the same way as Polars: PySpark, Ibis and others which were already mentioned.

The biggest bummer for me personally is that you can't disable line breaks formatting for specific data classes or in general (e.g. warn only when line is too long). Commenting every code block that uses such an API is not readable and annoying to work with. Or is it? Otherwise it sadly is a deal breaker for such a good tool and we would really like to adopt the astral-tooling.

Kind regards

---

_Comment by @GniLudio on 2025-11-18 15:14_

Workaround until there is proper support:
```python
counts = rdd \
    .map(lambda x: (x[0], 1)) \
    .reduceByKey(lambda a, b: a + b)  # fmt: skip
```

```python
(
    df.groupBy("city")
    .count()
    .toPandas()
    .set_index("city")
    .plot.pie(
        subplots=True,
        autopct="%1.1f%%",
        ylabel="",
    )
)
```

---

_Comment by @laundmo on 2025-12-04 16:35_

I think everyone here would appreciate at least *some* update on this. Its the most upvoted formatter style issue by 100 👍 - its clearly something people want.

Are you willing to accept contributions for some version of this as a `preview` feature? I dont want to spend effort on a change thats likely to be denied, even if previous comments don't indicate it would be, and i can only imagine a lot of people who want this feel similar.

Or, considering this part of the docs:
> The initial goal of the Ruff formatter is not to innovate on code style, but rather, to innovate on performance, and provide a unified toolchain across Ruff's linter, formatter, and any and all future tools.
> [...]
> Specifically, the formatter is intended to emit near-identical output when run over existing Black-formatted code.
> [...]
> Like Black, the Ruff formatter does not support extensive code style configuration; however, unlike Black, it does support configuring the desired quote style, indent style, line endings, and more.

Should everyone here take this issue to Black? It seems like they're open to contributions for implementing something like this: https://github.com/psf/black/issues/571#issuecomment-2499791828 - but then again that message is not very specific, and locking the issue isn't a good sign.
While i understand the goal of following Black formatting, Ruff is at a stage in its popularity where being more open to features for improving upon percieved mistakes in Blacks style could be a great boon.

---

_Comment by @ntBre on 2025-12-04 16:59_

There's a draft PR exploring this right now in https://github.com/astral-sh/ruff/pull/21369, so it's certainly on our mind.

---

_Comment by @laundmo on 2025-12-04 17:02_

Oh very nice! I didnt see any PRs linked, good to know that work is ongoing!

---

_Comment by @laundmo on 2025-12-04 22:11_

In the Draft PR an interesting point was raised:

```py
(
    np
    .arange(shape)
    .foo()
    .bar()
)
# vs
s = "str"
(
    s
    .replace("a", "b")
    .replace("c", "d")
)
```
Here, breaking up np.arange can feel wrong since `np` being the module name can almost become part of the function name, like you may read its function name as "np arange" instead of "arange". But because `s` is a variable it doesn't get this effect.

This presents a dilemma, as part of the point of this issue and the PR is exactly to break the call across multiple lines, but now we have a case where that's less obviously the correct choice. In the PR it was already discussed that the formatter doesn't have semantic information, like np being a module.

I'm putting this here so people can weigh in ideas about this situation.

---

_Comment by @luksquaresma on 2025-12-04 22:16_

I disagree here @laundmo . For me, both cases are ok. I don't like the idea of not breaking in the module.

> In the Draft PR an interesting point was raised:
> 
> ```py
> (
>     np
>     .arange(shape)
>     .foo()
>     .bar()
> )
> # vs
> s = "str"
> (
>     s
>     .replace("a", "b")
>     .replace("c", "d")
> )
> ```
> Here, breaking up np.arange feels wrong since `np` being the module name can almost become part of the function name, like you may read its function name as "np arange" instead of "arange". But because `s` is a variable it doesn't get this effect.
> 
> This presents a dilemma, as part of the point of this issue and the PR is exactly to break the call across multiple lines, but now we have a case where that's less obviously the correct choice. In the PR it was already discussed that the formatter doesn't have semantic information, like np being a module.
> 
> I'm putting this here so people can weigh in ideas about this situation.



---

_Closed by @dylwil3 on 2025-12-15 15:29_

---

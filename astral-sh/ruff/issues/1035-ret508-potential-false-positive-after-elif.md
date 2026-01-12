```yaml
number: 1035
title: RET508 potential false positive after elif
type: issue
state: closed
author: LefterisJP
labels:
  - bug
assignees: []
created_at: 2022-12-04T15:06:51Z
updated_at: 2023-11-04T23:42:53Z
url: https://github.com/astral-sh/ruff/issues/1035
synced_at: 2026-01-12T15:54:40Z
```

# RET508 potential false positive after elif

---

_@LefterisJP_

This is with `ruff==0.0.155`

I tried to drill it down to a simpler example but it did not happen there. Can you try to run ruff with `RET508` enabled on [this file](https://github.com/rotki/rotki/blob/a916d692cd1cc6210370ffb38278a4ecec1b2829/rotkehlchen/tests/api/test_async.py), like
`ruff rotkehlchen/tests/api/test_async.py `

It's returning

```
rotkehlchen/tests/api/test_async.py:75:13: RET508 Unnecessary `else` after `break` statement
rotkehlchen/tests/api/test_async.py:137:9: RET508 Unnecessary `else` after `break` statement
```

Which I am confused how it makes sense.

If we remove the else and put the Assertion (or any other code) after the `if/elif` the resulting code won't be the same.

I got similar questionable results with the other RET codes btw.

Also a question on pyproject toml. I have ended up with somethine like:

```
select = ["E", "F", "W", "C", "N", "B", "T", "UP", "YTT", "C4", "T10", "T201"]
```

Isn't `T` a superset of `T201` and `T210`? So I can omit those? Same for `C4` and `C`?

---

_Comment by @charliermarsh on 2022-12-04 15:14_

Will clone and take a look. It could just be a limitation in the plugin logic (i.e., does it reproduce in the original `flake8-return`?). Or it could be a bug in the translation to Rust...


---

_Comment by @charliermarsh on 2022-12-04 15:14_

And yeah, your configuration is equivalent to:

```
select = ["E", "F", "W", "C", "N", "B", "T", "UP", "YTT"]
```


---

_Comment by @LefterisJP on 2022-12-04 15:52_

> Will clone and take a look. It could just be a limitation in the plugin logic (i.e., does it reproduce in the original `flake8-return`?). Or it could be a bug in the translation to Rust...

Well I think I have added some flake8 checkers thanks to ruff, that I did not use before. So yes we never used `flake8-return` in rotki before.

I can see the exact same error for that file with flake8-return.

Perhaps I am too tired or not focused but does it not seem wrong?



---

_Comment by @charliermarsh on 2022-12-05 17:08_

> rotkehlchen/tests/api/test_async.py:75:13: RET508 Unnecessary `else` after `break` statement

I think the idea here is that the code, as written, is:

```py
while True:
    # and now query for the task result and assert on it
    response = requests.get(
        api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
    )
    assert_proper_response(response)
    json_data = response.json()
    if json_data['result']['status'] == 'pending':
        # context switch so that the greenlet to query balances can operate
        gevent.sleep(1)
    elif json_data['result']['status'] == 'completed':
        break
    else:
        raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
```

But this is equivalent to the following:

```py
while True:
    # and now query for the task result and assert on it
    response = requests.get(
        api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
    )
    assert_proper_response(response)
    json_data = response.json()
    if json_data['result']['status'] == 'pending':
        # context switch so that the greenlet to query balances can operate
        gevent.sleep(1)
    elif json_data['result']['status'] == 'completed':
        break    
    raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
```

...since this will only ever be reached if the `elif` evaluates to `False`.

So I think the check is right, based on a very cursory look, but whether you want to enforce those rules is maybe a different question :)


---

_Closed by @charliermarsh on 2022-12-05 17:08_

---

_Comment by @LefterisJP on 2022-12-09 19:19_

> > rotkehlchen/tests/api/test_async.py:75:13: RET508 Unnecessary `else` after `break` statement
> 
> I think the idea here is that the code, as written, is:
> 
> ```python
> while True:
>     # and now query for the task result and assert on it
>     response = requests.get(
>         api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
>     )
>     assert_proper_response(response)
>     json_data = response.json()
>     if json_data['result']['status'] == 'pending':
>         # context switch so that the greenlet to query balances can operate
>         gevent.sleep(1)
>     elif json_data['result']['status'] == 'completed':
>         break
>     else:
>         raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
> ```
> 
> But this is equivalent to the following:
> 
> ```python
> while True:
>     # and now query for the task result and assert on it
>     response = requests.get(
>         api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
>     )
>     assert_proper_response(response)
>     json_data = response.json()
>     if json_data['result']['status'] == 'pending':
>         # context switch so that the greenlet to query balances can operate
>         gevent.sleep(1)
>     elif json_data['result']['status'] == 'completed':
>         break    
>     raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
> ```
> 
> ...since this will only ever be reached if the `elif` evaluates to `False`.
> 
> So I think the check is right, based on a very cursory look, but whether you want to enforce those rules is maybe a different question :)

Huh ... so weird. I could swear I thought it made no sense but now it does look like it makes sense. Brain fart maybe? Apologies for wasting your time with this.

---

_Comment by @charliermarsh on 2022-12-09 19:27_

No worries at all, it’s a hard one to reason about.

---

_Comment by @actionless on 2022-12-09 23:31_

i made a simpler example for reproducing this bug:

```python
def func(arg1: int) -> None:
    for i in range(2):
        if arg1 == i:
            if arg1 == 1:
                break
            print("don't break")
        elif arg1 == 2:
            break
        else:
            print('else')
```

```console
$ ruff test_ruff_ret_508.py
Found 1 error(s).
test_ruff_ret_508.py:7:9: RET508 Unnecessary `else` after `break` statement
```

---

_Comment by @LefterisJP on 2022-12-11 23:12_

> No worries at all, it’s a hard one to reason about.

Hey I took another stab at this @charliermarsh, by activating the `RET` module again and I think that what you posted above (and  have 2-3 similar false positives in our repo for) is after all incorrect -- or I am totally asleep.

### Original Code

```python
while True:
    # and now query for the task result and assert on it
    response = requests.get(
        api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
    )
    assert_proper_response(response)
    json_data = response.json()
    if json_data['result']['status'] == 'pending':
        # context switch so that the greenlet to query balances can operate
        gevent.sleep(1)
    elif json_data['result']['status'] == 'completed':
        break
    else:
        raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
```

### Code you posted as equivalent due to the linting warning

```python
while True:
    # and now query for the task result and assert on it
    response = requests.get(
        api_url_for(server, "specific_async_tasks_resource", task_id=task_id),
    )
    assert_proper_response(response)
    json_data = response.json()
    if json_data['result']['status'] == 'pending':
        # context switch so that the greenlet to query balances can operate
        gevent.sleep(1)
    elif json_data['result']['status'] == 'completed':
        break    
    raise AssertionError(f"Unexpected status: {json_data['result']['status']}")
```

The problem here is that it's not equivalent in the first if condition being `True`. So `if json_data['result']['status'] == 'pending':`.

The original code will wait for 1 second (`gevent.sleep()`), and then continue after the code.

The modified as equivalent code will wait for 1 second then get out of the if/elif and hit the assertion.

---

_Comment by @charliermarsh on 2022-12-11 23:17_

Yeah I think you’re right.

---

_Comment by @charliermarsh on 2022-12-11 23:18_

(They’re not equivalent, not sure why I thought they were.)

---

_Reopened by @charliermarsh on 2022-12-11 23:20_

---

_Comment by @LefterisJP on 2022-12-11 23:26_

The problem is with the following.

RET505, RET506, RET507, RET508

Simple program to reproduce them all

```python
def foo(a: int) -> int:
    if a == 1:
        result = 2
    elif a == 3:
        raise ValueError()
    else:
        raise AssertionError('Evil')

    return result

def foo2(a: int) -> int:
    while True:
        if a == 1:
            result = 2
        elif a == 3:
            break
        else:
            raise AssertionError('Evil')

        return result

def foo3(a: int) -> int:
    if a == 1:
        result = 2
    elif a == 2:
        return 42
    else:
        raise AssertionError('Evil')

    return result

def foo4(a: int) -> int:
    while True:
        if a == 1:
            result = 2
        elif a == 3:
            continue
        else:
            raise AssertionError('Evil')

        return result
```

Seems to only happen for indented code. So if it's inside a function. If it's in the global space it does not happen.

```python
var = 1
if var == 1:
    var_result = 2
elif var == 3:
    raise ValueError()
else:
    raise AssertionError('Evil')

print(var)
```

---

_Comment by @charliermarsh on 2022-12-12 00:54_

Taking a look at this now.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-12 00:54_

---

_Label `bug` added by @charliermarsh on 2022-12-12 00:54_

---

_Comment by @charliermarsh on 2022-12-12 01:08_

`flake8-return` seems to have the same behavior (not that it's correct).

---

_Comment by @charliermarsh on 2022-12-12 01:15_

I'm considering just disabling this for `elif` for now. It's pretty hard to reason about.

---

_Comment by @charliermarsh on 2022-12-12 01:59_

I kind of think the logic is not very good for any of these rules beyond RET501, RET502, and RET503.

---

_Comment by @charliermarsh on 2022-12-12 03:38_

I can change it to only operate on simple `if-else` blocks (no `elif`), but I'm guessing that the plugin wants you to change...

```py
def foo(a: int) -> int:
    if a == 1:
        result = 2
    elif a == 3:
        raise ValueError()
    else:
        raise AssertionError('Evil')

    return result
```

...to...

```py
def foo(a: int) -> int:
    if a == 1:
        result = 2
    else:
        if a == 3:
            raise ValueError()
        raise AssertionError('Evil')

    return result
```

Or maybe the plugin logic is just not good.

---

_Comment by @LefterisJP on 2022-12-12 16:31_

In any case such a suggestion is bad as it's not an improvement in anyway. I could swear I have a flake8 or other tool with similar~ish suggestions for no else after break or raise. But as you can see these ones seem to be wrong.

---

_Comment by @henryiii on 2023-02-09 15:52_

PyLint's handling of this is much better, FWIW. I don't think I've ever had a false positive from PyLint's else after control flow checks.

---

_Closed by @charliermarsh on 2023-02-14 02:51_

---

_Comment by @doolio on 2023-11-04 23:20_

I'm hitting this error (RET508 Unnecessary `elif` after `break` statement) with the below using `ruff v0.1.4`. I'm just a beginner so quite likely it could be avoided with better code.

````python
def main() -> None:
    """Play a game of tic-tac-toe."""
    print('Welcome to tic-tac-toe!')
    game: dict[str, str] = create_board()
    current_player, other_player = X, O  # X goes first.

    while True:
        print(game_state(game))
        move: str = BLANK
        # Keep asking the player for a non-blank space to mark.
        while not is_space_blank(game, move):
            move = input(f"What is {current_player}'s move? (1-9) ")
        make_move(game, move, current_player)
        # Check if the game is over.
        if is_player_winner(game, current_player):  # First check for victory.
            print(game_state(game))
            print(f'{current_player} has won the game!')
            break
        elif is_board_full(game):  # Then check for a tie.
            print(game_state(game))
            print('The game is a tie!')
            break
        # Swap turns.
        current_player, other_player = other_player, current_player
    print('Thanks for playing!')
````

---

_Comment by @henryiii on 2023-11-04 23:35_

You don’t need else after a control flow statement. That’s `return`, `yield`, `break`, or `continue`. Remove the “el” in front of the elif.

---

_Comment by @doolio on 2023-11-04 23:42_

I see. Thank you so much for pointing that out.

---

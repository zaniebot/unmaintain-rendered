```yaml
number: 7438
title: "[New Rule] Match positional argument name"
type: issue
state: closed
author: jd-solanki
labels:
  - rule
assignees: []
created_at: 2023-09-16T14:44:28Z
updated_at: 2023-09-19T03:42:53Z
url: https://github.com/astral-sh/ruff/issues/7438
synced_at: 2026-01-10T11:09:49Z
```

# [New Rule] Match positional argument name

---

_Issue opened by @jd-solanki on 2023-09-16 14:44_

Hi 👋🏻

> **Note**
> Except issue title I might have interchanged the arguments & parameters

## Problem

ATM, if we pass arguments to function it's error-prone:

```py
def greet(name: str, greeting: str):
    print(f'{greeting}, {name}')

name = 'john'
greeting = 'Hello'

greet(name, greeting) # Hello, john

# Issue 1: Passing arguments in the wrong order
greet(greeting, name) # john, Hello

# Issue 2: Have to be explicit
greet(name=name, greeting=greeting) # Hello, john
```

To avoid passing wrong arguments best way is to be explicit, but with three arguments code is less readable. For example here's FastAPI example:

```py
# Issue 1: Due to positional args we can pass wrong arguments
crud.create_user(db, user, fn_hash_pwd=hash.hash_pwd)

# Issue 2A: With keyword arguments we have to be explicit (more code) & less readable (one big line)
# Issue 2B: If we change single arg name or fn name, we get git diff for all arguments & function
crud.create_user(db=db, user=user, fn_hash_pwd=hash.hash_pwd)

# Issue 3: Perfect but more lines of code
crud.create_user(
    db=db,
    user=user,
    fn_hash_pwd=hash.hash_pwd
)
```

## Solution

We can implement a rule that checks passed argument must match the name of the function argument. If they don't match, we get an error.

```py
# Benefit 1: With this can be sure that we are passing arguments in the right order
# Benefit 2: Don't have to be explicit to avoid error
# Benefit 3: We can ensure function definition and function call are in sync
greet(name, greeting)

# If we pass arguments in the wrong order we get an error
greet(greeting, name) # Error: Passed argument name doesn't match function argument

# 
greet(user_name, greeting) # Error: Argument name doesn't match function argument
# Solution: greet(name=user_name, greeting=greeting)
```

With FastAPI example:

```py
# We are sure that db is the first argument and user is the second argument
crud.create_user(db, user, fn_hash_pwd=hash.hash_pwd)

# Even if author didn't provided the types, we can still catch the error
crud.create_user(user, db, fn_hash_pwd=hash.hash_pwd) # Error
```

We can catch more errors like if author of the function changes the name of the argument due to implementation changes with the same type.

```py
def greet(name: str, surname: str):
    print(f'Welcome {name} {surname}')

# With rule => We can catch this error and update our code
greet(name, greeting) # Error: Passed argument surname doesn't match function argument

# Without rule => We don't get any error and our code behaves unexpectedly
greet(name, greeting) # Welcome john Hello
```

---

_Comment by @charliermarsh on 2023-09-19 03:42_

Hey! I appreciate the clear and thorough issue. While not unreasonable, my take here is that this best solved by using keyword-only arguments, even if the resulting call feels less readable to you than using positional arguments. Requiring that positional arguments exactly match the name of the parameter feels like a highly specific and pedantic rule, and for us to implement it as a first-class rule in Ruff, we'd want to have some confidence that it would be useful to a large proportion of the community. Separately, it's probably hard for us to implement this reliably right now, since we don't support tracing functions calls across files and across third-party dependencies.

I'm going to close as we're unlikely to implement this as-is -- sorry to disappoint but hopefully my reasoning is clear.

---

_Closed by @charliermarsh on 2023-09-19 03:42_

---

_Label `rule` added by @charliermarsh on 2023-09-19 03:42_

---

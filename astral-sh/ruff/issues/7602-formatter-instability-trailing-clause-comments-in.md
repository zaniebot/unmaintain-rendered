```yaml
number: 7602
title: "Formatter instability: trailing clause comments in `if` statement"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-22T16:10:49Z
updated_at: 2023-09-22T22:12:33Z
url: https://github.com/astral-sh/ruff/issues/7602
synced_at: 2026-01-12T15:54:47Z
```

# Formatter instability: trailing clause comments in `if` statement

---

_@charliermarsh_

Given:

```python
def Get_x_csrf_token(cookie, showErrors=bool):
    try:
        try:
            socket.create_connection(("google.com", 80))
        except socket.gaierror:
            if showErrors == True:
                raise APIConnectFailed(
                    "No internet connection, check your internet connection and try again."
                )
            else:
                return
            # req.urlopen('http://google.com')
            # print("No internet connection, quiting.")
            # exit(-1)
        # try:
        # except:
        xsrfRequest = requests.post(
            "https://auth.roblox.com/v2/logout", cookies={".ROBLOSECURITY": cookie}
        )
        if xsrfRequest.headers:
            if xsrfRequest.headers.get("x-csrf-token") == None:
                if showErrors == True:
                    raise InvalidCookie(
                        "Invalid .ROBLOSECURITY cookie, are you sure you're using your current cookie ?"
                    )
                else:
                    return
            else:
                return xsrfRequest.headers["x-csrf-token"]
        else:
            if showErrors == True:
                raise NoHeadersError(
                    "The API didn't return any response headers. Check your internet connection."
                )
            else:
                return
                # APIError("The request returned no result headers, Response: ",xsrfRequest.status_code)

            # else:
            # print(xsrfRequest.headers["x-csrf-token"],xsrfRequest.headers)

    except Exception as e:
        raise GetTokenException(
            f"'Get x-csrf-token' process returned an Exception: {e}'"
        )
```

We're removing the newline after `# print(xsrfRequest.headers["x-csrf-token"],xsrfRequest.headers)`, which leads to an instability.

The above code is the once-formatted version of:

```python
def Get_x_csrf_token(cookie,showErrors=bool):
    try:
        try:
            socket.create_connection(('google.com', 80))
        except socket.gaierror:
            if showErrors == True:
                raise APIConnectFailed("No internet connection, check your internet connection and try again.")
            else:
                return
        #try:
            #req.urlopen('http://google.com')
        #except:
            #print("No internet connection, quiting.")
            #exit(-1)
        xsrfRequest = requests.post("https://auth.roblox.com/v2/logout",
                                    cookies={".ROBLOSECURITY": cookie})
        if xsrfRequest.headers:

            if xsrfRequest.headers.get("x-csrf-token") == None:
                if showErrors == True:
                    raise InvalidCookie("Invalid .ROBLOSECURITY cookie, are you sure you're using your current cookie ?")
                else:
                    return
            else:
                return xsrfRequest.headers["x-csrf-token"]
        else:
            if showErrors == True:
                raise NoHeadersError("The API didn't return any response headers. Check your internet connection.")
            else:
                return


            #else:
                #APIError("The request returned no result headers, Response: ",xsrfRequest.status_code)
            #print(xsrfRequest.headers["x-csrf-token"],xsrfRequest.headers)

    except Exception as e:
        raise GetTokenException(f"'Get x-csrf-token' process returned an Exception: {e}'")
```

Sourced from https://github.com/astral-sh/ruff/issues/7590 (`A_14230110707883433080.py`).

---

_Label `bug` added by @charliermarsh on 2023-09-22 16:10_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 16:10_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-22 16:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-22 16:44_

---

_Comment by @MichaReiser on 2023-09-22 16:51_

This is fixed on main

---

_Closed by @MichaReiser on 2023-09-22 16:51_

---

_Reopened by @charliermarsh on 2023-09-22 17:01_

---

_Comment by @MichaReiser on 2023-09-22 17:16_

More minimal repro. It's related with `b` having a larger indent than `a` and `c`

https://play.ruff.rs/7723b186-8aa4-4572-8f1e-ec871229be42

---

_Closed by @charliermarsh on 2023-09-22 22:12_

---

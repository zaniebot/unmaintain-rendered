```yaml
number: 2358
title: Resolver panics due to missing key in inverse
type: issue
state: closed
author: david-pl
labels:
  - bug
assignees: []
created_at: 2024-03-11T15:33:50Z
updated_at: 2024-03-12T07:36:39Z
url: https://github.com/astral-sh/uv/issues/2358
synced_at: 2026-01-12T15:58:37Z
```

# Resolver panics due to missing key in inverse

---

_@david-pl_

I have a requirements.txt with the following contents (somewhat shortened):

```
alembic==1.13.1
aiohttp==3.9.3
ansible==9.3.0
bcrypt==4.1.2
beautifulsoup4==4.12.3
b2sdk==1.32.0
boto3==1.34.52
celery[redis]==5.2.7
email-validator==2.1.1
Flask==3.0.2
Flask-Caching==2.1.0
flask-debugtoolbar==0.14.1
Flask-Migrate==4.0.5
Flask-SQLAlchemy==3.1.1
Flask-WTF==1.2.1
Flask-Mail==0.9.1
google-api-python-client==2.120.0
google-auth==2.28.0
google-auth-httplib2==0.2.0
google-auth-oauthlib==1.2.0
google-cloud-compute==1.17.0
google-cloud-dns==0.35.0
google-cloud-secret-manager==2.18.2
google-cloud-storage==2.14.0
grpcio==1.62.0
gunicorn==21.2.0
hcloud==1.33.2
hubspot-api-client==8.2.1
Jinja2==3.1.3
jira==3.6.0
kombu==5.2.4
ldap3==2.9.1
llama-index==0.4.40
MarkupSafe==2.1.5
oauth2client==4.1.3
oauthlib==3.2.2
octodns==1.5.0
octodns-googlecloud==0.0.3
octodns-route53==0.0.6
openai==0.28.1
```

Doing
```
uv venv venv
source venv/bin/activate
uv pip install -r requirements.txt
```
Errors with
```
thread 'main' panicked at crates/uv-resolver/src/resolution.rs:230:50:
no entry found for key
```


---

_Label `bug` added by @charliermarsh on 2024-03-11 16:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-11 16:42_

---

_Comment by @charliermarsh on 2024-03-11 16:42_

Wow nice, thanks! I'll take these.

---

_Closed by @charliermarsh on 2024-03-11 17:51_

---

_Comment by @david-pl on 2024-03-12 07:36_

Wow, that was quick! Awesome, thanks.

---

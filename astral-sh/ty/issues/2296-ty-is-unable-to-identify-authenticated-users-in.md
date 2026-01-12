```yaml
number: 2296
title: ty is unable to identify Authenticated Users in Django
type: issue
state: closed
author: KavyanshKhaitan2
labels:
  - library
assignees: []
created_at: 2026-01-01T13:07:57Z
updated_at: 2026-01-06T00:33:01Z
url: https://github.com/astral-sh/ty/issues/2296
synced_at: 2026-01-12T15:54:26Z
```

# ty is unable to identify Authenticated Users in Django

---

_@KavyanshKhaitan2_

### Summary

I have these views:
```py
class AuthRequiredView(LoginRequiredMixin, View):
    def dispatch(self, request, *args, **kwargs):
        request = self.request
        if not request.user.is_authenticated:
            return super().dispatch(request, *args, **kwargs)
        if not request.user.onboarding_complete():  # ty:ignore[unresolved-attribute]
            return redirect("authentication.onboarding")
        return super().dispatch(request, *args, **kwargs)


class ClinicEnabledRequiredView(AuthRequiredView):
    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_authenticated:
            return super().dispatch(request, *args, **kwargs)
        if not request.user.clinic_enabled:
            return redirect("user.dashboard")
        return super().dispatch(request, *args, **kwargs)
```

But then, when I try to use it in this view:
```py
class ClinicPatientRequestsView(ClinicEnabledRequiredView):
    def get(self, request, *args, **kwargs):
        appointments = models.Appointment.objects.filter(
            service__clinic=self.request.user.clinic, status="BOOKED"
        )
        return render(
            request,
            "dashboard/clinic/patient_requests.html",
            context={"appointments": appointments},
        )
```

`ty` throws an error:

<img width="1021" height="233" alt="Image" src="https://github.com/user-attachments/assets/78e2d4a5-ec20-4db6-8ee3-460b1ac59067" />

It would be GREAT if I had a way to tell ty that this property is of X type.
Also, even when I do `request.user: User = request.user`, it doesnt show up in the other view(s).

### Version

ty 0.0.8

---

_Comment by @dhruvmanila on 2026-01-05 10:33_

I'm not a Django expert but this looks similar (if not same?) as #1018.

---

_Label `library` added by @dhruvmanila on 2026-01-05 10:34_

---

_Comment by @carljm on 2026-01-06 00:33_

I don't think this is the same as #1018. I think this is actually a duplicate of #1479, because there is an `AbstractBaseUser | AnonymousUser` union, and `AbstractBaseUser` has [an `is_authenticated` property that is always `True`](https://github.com/typeddjango/django-stubs/blob/master/django-stubs/contrib/auth/base_user.pyi#L29), whereas `AnonymousUser` [has one that is always `False`](https://github.com/typeddjango/django-stubs/blob/master/django-stubs/contrib/auth/models.pyi#L149). With #1479 we should be able to filter that union according to the `if request.user.is_authenticated` checks in the code above.

Closing as duplicate of #1479. Thanks for the report!

---

_Closed by @carljm on 2026-01-06 00:33_

---

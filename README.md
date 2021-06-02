# Django Helm Chart

A generic Django (plus Celery) Helm chart.

# Preparing your Django app

This chart supports a web plus optional celery and beat deployments. Be prepared to extend it as necessary.

Django settings will be managed by environment variables. `django-environ` works well for this and can parse the DATABASE_URL connection string. This chart expects SECRET_KEY and DATABASE_URL variables.

Kubernetes works best when it is able to determine application health. You Django app should have a `/_health/` view such as

```
def health(request):
    return HttpResponse("ok", content_type="text/plain")

urlpatterns = [
    path("_health/", health),
...
```

# Usage

1. Add our Helm chart repo `helm repo add django https://burke-software.gitlab.io/django-helm-chart/`
2. Review our values.yaml. At a minimum you'll need to set env.secret.SECRET_KEY and env.secret.DATABASE_URL.
3. Install the chart `helm install your-app django/django -f your-values.yml`

# Tips

- Do you really need kubernetes? It's very complex.
- Don't use helm without [helm diff](https://github.com/databus23/helm-diff). One typo will wipe your app without warning otherwise.
- While supported, I don't suggest running stateful services like PostgreSQL in kubernetes. Upgrades will likely involve downtime or extensive and arcane knowledge.
- It's fine to use this chart as a reference for your own chart instead of directly using it.

## Managing environment variables and secrets

I suggest either

- Keep them in a values.yml file in a private repo
- Make use of --reuse-values and --set
- Keep them in a non helm chart managed service

## Deploying in CI

I use lwolf/helm-kubectl-docker with Gitlab CI. [Example](https://gitlab.com/glitchtip/glitchtip-frontend/-/blob/master/.gitlab-ci.yml).

# Support development

Maintaining this chart takes time. Considering supporting it by

- [Donating on liberapay](https://liberapay.com/burke-software/)
- Check out [GlitchTip](https://glitchtip.com) error tracking, which is where this project started

Commercial support is available - email info@burkesoftware.com

# Contributing

Contributions are welcome. Report bugs on gitlab issues. Please only open feature requests that you'd like to implement yourself or pay for.
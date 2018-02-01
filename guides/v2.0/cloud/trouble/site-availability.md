---
layout: default
group: cloud
title: Site availability
version: 2.0
github_link: cloud/trouble/site-availability.md
functional_areas:
  - Cloud
  - Configuration
  - Services
---

If you are having site availability issues, the first thing you should do is review your [deployment logs]({{page.baseurl}}cloud/trouble/environments-logs.html#log-deploy-log) to see if you can identify the problem.

You may be able to resolve your issue by searching your logs for examples in this topic and trying the associated solution.

## CredisException
This exception is caused by a known issue with how Magento handles simultaneous connections to Redis during static content deployment.

    [2018-01-30 18:56:52] Generating static content for locales: en_US
    [2018-01-30 18:56:52] Command:php ./bin/magento setup:static-content:deploy --jobs=3  en_US

      [CredisException]
      read error on connection

During static content deployment, the default number of processing jobs is set to `3`. We recommend setting the number of processing jobs to `1` as a workaround.

### Symptoms
-   Your site is not functioning at all. HTTP requests result in 50x errors.
-   Your site is functioning normally, but fails to refresh static assets.

### Solution
Modify the deploy phase using the `STATIC_CONTENT_THREADS` environmental variable and redeploy your site.

1.  In a terminal, log in to your project.

        magento-cloud-login

1.  Set the variable.

        magento-cloud variable:set STATIC_CONTENT_THREADS '1' -e <environment>

Refer to [Manage variables]({{page.baseurl}}cloud/env/environment-vars_over.html) and [Redis and static-content deployment]({{page.baseurl}}guides/v2.0/cloud/trouble/redis-troubleshooting.html#static-content) for more information.

<div class="bs-callout bs-callout-info" markdown="1">
For Pro projects **created before October 23, 2017**, you must open a [support ticket]({{page.baseurl}}cloud/bk-cloud.html#gethelp) to add this environment variable to your production and staging environments.
</div>
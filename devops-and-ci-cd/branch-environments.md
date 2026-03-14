# Branch Environments

Being able to preview changes on every branch is one of the most useful features of a modern developer platform.

Airbase lets you deploy and destroy a preview environment for each of your branches in just 90 seconds.

## Before you start

You’ll need to:

* create and inject Airbase credentials for your CI / CD environment:
  * `AIRBASE_ACCESS_KEY_ID` - Identifies the Airbase Access Key
  * `AIRBASE_SECRET_ACCESS_KEY` - Actual Access Key (Secret)
* add a suitable build step to your CI / CD pipeline (e.g. `airbase build`)

### Deploying a Branch Environment

```
airbase container deploy --yes --output deploy.env "$branch_name"
```

Inside the `deploy.env` file, you’ll find these entries:

* `DYNAMIC_ENVIRONMENT_URL`: the URL pointing to this branch deployment.

### Destroying a Branch Environment

Once a branch has been merged and the code review / pull request has been concluded, destroy the branch environment with this command:

```
airbase container destroy --yes "$branch_name"
```

This will shut down the environment for this branch’s preview URL, which will become inaccessible.

### Deploying to production

Production deployments can be triggered manually (as part of a deployment workflow) or automatically (as part of continuous delivery).\
To deploy to production, run this command:

```
airbase container deploy --yes
```

The web application will be deployed to the production URL and become visible on the Airbase Console.

To redeploy or update production, simply re-run this command from the pipeline with the new build and environment variables.

## Conclusion

It’s simple to preview your branches by integrating the Airbase CLI into your existing development workflow.

If you’re using SHIP GitLab, have a look at our CI templates! They come batteries included and are easy to use.

[Environment Variables](environment-variables.md) [Integrations](./)

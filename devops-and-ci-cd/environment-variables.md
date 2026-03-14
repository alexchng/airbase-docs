# Environment Variables

Environment variables may be supplied to your deployments via `.env` files.

The environment variable files are loaded and merged in this order (highest-to-lowest priority):

* `.env.${environment}`
* `.env`
* `.env.local`

### Configuring variables for an environment

Remember that the **production** environment (with no suffix) is called the `default` environment.

When using a command line interface to create a preview environment (for example: `preview-new-feature`):

#### Create an env file for the corresponding environment (if necessary)

The env file suffix should match the environment name, for example:

* `preview-new-feature` -> `.env.preview-new-feature`
* `development` -> `.env.development`
* `staging` -> `.env.staging`
* `default` -> `.env.default` (production)

```
# .env.preview-new-feature
EXAMPLE_PARAMETER=preview-new-feature-1234567890abcdef
```

#### Deploy application to Airbase normally

```
airbase container deploy preview-new-feature
```

### Using CI/CD

When committing your code to source code repository, care should be taken to not accidentally commit secrets.

When using the [Airbase GitLab CI integration](gitlab-ci-cd.md) and the provided pipeline template, we can store the Environment Variables and CI/CD variables, and inject them to the `.env` file at build time.

To customise the environment variables for each environment from GitLab CI environments:

1. In the GitLab Console, visit **CI/CD Settings > Variables**
2. Then add a new variable and configure it appropriately.
3. Set the **Environment scope** to the desired environment (e.g. `development`, `review/*`, `staging`, `production`, etc.)

![Screenshot of GitLab CI / CD Environment Variables](https://console.v2.airbase.sg/docs/_next/image?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fgitlab-cicd-env.9a0376fb.png\&w=3840\&q=75)

The **Environment scope** will be matched on the environment parameter in the **.gitlab-ci.yml** file (see below)

```
review:
...
  environment:
    name: review/$CI_COMMIT_REF_SLUG
...

deploy:
...
  environment:
    name: production
...
```

Follow the instructions here to [set up GitLab CI/CD](gitlab-ci-cd.md) for your project.

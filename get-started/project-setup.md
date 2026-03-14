# Project Setup

## Project Setup

We will set up the development environment of the sample application that we had [deployed earlier](deploying-a-project.md), so that we can make changes to the application.

{% hint style="info" %}
The sample application uses Python 3 to run the HTTP server, so ensure that you have Python 3 installed on your system.
{% endhint %}

Here’s the [source code](https://sgts.gitlab-dedicated.com/innersource/sgts/runtime/airbase/demo/days) of the Countdown application that we had deployed earlier.

Set up your project folder and install all the dependencies:

```bash
git clone git@sgts.gitlab-dedicated.com:innersource/sgts/runtime/airbase/demo/days.git
cd days
```

Alternatively, you can also check out our [featured templates](../templates.md).

### Develop and build your app

Use your favourite Integrated Development Environment (IDE), or start with just one command:

```bash
./run.sh
```

You can preview your changes by going to http://localhost:8000

Once you’re ready to deploy your application, read on.

{% hint style="info" %}
Linking your project to Airbase is optional — you can also specify the project in the `deploy` command.
{% endhint %}

### Link your project to Airbase

Run the following in your project folder to connect it to an Airbase project:

```bash
airbase link
```

### Build container image for Airbase

The sample application provides a Dockerfile that is suitable for deployment on Airbase.

See [Docker images](../container-images/) for considerations if you would like to use your own Dockerfile.

To build the image for Airbase, run:

```bash
airbase container build
```

### Deploy your changes

Deploy your application to Airbase by running the following command, and selecting “Yes” in the confirmation prompt.

```bash
airbase container deploy
```

Once your application is deployed, a link will be displayed. Example output:

```
? Deploy gdssingapore/airbase:demo-days to days in default (production) (demo/demo-days:default) Yes
Deployed days in default (demo/demo-days:default)
Visit https://demo-days.app.tc1.airbase.sg
```

### That’s it!

Your changes should be available at the URL shown in the terminal.

### What’s next?

Now that you’ve made changes and shipped your first project, consider these guides.

Development

* [Templates](../templates.md)
* [Docker Images](../container-images/)

Operations

* [Viewing Logs](../airbase-console/logs.md)
* [Environment Variables](../devops-and-ci-cd/environment-variables.md)
* [Branch Environments](../devops-and-ci-cd/branch-environments.md)
* [SGTS GitLab CI Pipelines](../devops-and-ci-cd/gitlab-ci-cd.md)

Additional

* [Deploying Projects](deploying-a-project.md)
* [Templates](../templates.md)

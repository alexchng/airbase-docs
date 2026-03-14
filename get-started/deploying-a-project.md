# Deploying a Project

## Deploying a Project

{% hint style="info" %}
Airbase deploys container workloads, so you would need to build your application as a Docker image for deployments. In the next section, we will build and deploy an image from source.

Here, we will deploy a sample application that has a pre-built Docker image.
{% endhint %}

### Before you begin

Here’s what we’ve covered so far:

**Signed up for an Airbase account**

* Created a team and an Airbase project to deploy into

**Installed the Airbase CLI**

* Logged in to Airbase

### Deploying Projects

#### Pull application image

Start by pulling the application image to be deployed

{% hint style="warning" %}
Airbase only supports linux/amd64 image. Take extra care to pull linux/amd64 image!
{% endhint %}

```bash
docker pull --platform linux/amd64 gdssingapore/airbase:demo-days
```

You should see something like this. Note that the digest value may be slightly different.

```
demo-days: Pulling from gdssingapore/airbase
Digest: sha256:e99fe672b29b1ee848ff162f6ae7303ace7c523ae6792f3e443f16d783b91eac
Status: Image is up to date for gdssingapore/airbase:demo-days
docker.io/gdssingapore/airbase:demo-days
```

#### Deploy your application

Deploy your application to Airbase by running the following command, replacing `$TEAM_HANDLE` and `$PROJECT_HANDLE` with your team handle and project handle respectively.

```bash
airbase container deploy --project $TEAM_HANDLE/$PROJECT_HANDLE --image gdssingapore/airbase:demo-days
```

That’s it! Confirm the deployment of your application by selecting “Yes” in the confirmation prompt. Once your application is deployed, a link will be displayed at the bottom of your screen, like this.

```
$ airbase container deploy --project demo/demo-days --image gdssingapore/airbase:demo-days
? Deploy gdssingapore/airbase:demo-days to days in default (production) (demo/demo-days:default) Yes
Deployed days in default (demo/demo-days:default)
Visit https://demo-days.app.tc1.airbase.sg
```

### That’s it!

![Screenshot of demo app deployed on Airbase](https://console.v2.airbase.sg/docs/_next/image?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fairbase-demo-days.fb4b990b.png\&w=1920\&q=75)

Your application is now deployed to Airbase. The application can be viewed at the URL in the terminal.

Read on to see how to make changes to the sample project.
